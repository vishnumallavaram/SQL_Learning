# Module 1: SQL Fundamentals — DDL & DML Practical Work

## Overview

This workbook provides hands-on exercises for learning SQL Server's Data Definition Language (DDL) and Data Manipulation Language (DML).

You will practise creating databases and tables, defining constraints, inserting and modifying data, changing table structures, and managing temporary tables.

**Difficulty:** Beginner to Intermediate  
**Database:** Microsoft SQL Server  
**Learning approach:** Attempt each exercise before checking its solution.

## Learning Objectives

By completing this workbook, you will be able to:

- Create and manage databases and tables.
- Select appropriate SQL Server data types.
- Apply `PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`, `NOT NULL`, `CHECK`, and `DEFAULT` constraints.
- Insert, update, and delete records.
- Modify table structures using `ALTER TABLE`.
- Understand the differences between `DELETE`, `TRUNCATE`, and `DROP`.
- Create temporary tables and copy data into new tables.
- Execute SQL scripts safely and validate the results.

---

## 1. Getting Started

### Exercise 1.1: Create a practice database

**Task**

Create a database named `SQLLearning_Module1_Practice`.

**Expected outcome**

A database with the specified name should exist in SQL Server.

**Solution**

```sql
IF DB_ID('SQLLearning_Module1_Practice') IS NULL
BEGIN
    CREATE DATABASE SQLLearning_Module1_Practice;
END;
GO

USE SQLLearning_Module1_Practice;
GO
```

**Explanation**

- `CREATE DATABASE` creates a database.
- `DB_ID()` checks whether a database exists.
- `USE` selects the database for subsequent statements.
- `GO` separates batches in tools such as SQL Server Management Studio (SSMS).

The existence check makes the script safer to rerun.

### Exercise 1.2: Verify the current database

**Task**

Display the name of the database currently selected.

**Expected outcome**

The result should be `SQLLearning_Module1_Practice`.

**Solution**

```sql
SELECT DB_NAME() AS CurrentDatabase;
```

---

## 2. Creating Tables and Understanding Data Types

### Exercise 2.1: Create a Customers table

**Task**

Create a table named `Customers` with the following columns:

| Column | Data type | Requirement |
|---|---|---|
| CustomerID | INT | Primary key |
| FirstName | VARCHAR(50) | Required |
| LastName | VARCHAR(50) | Required |
| Email | VARCHAR(100) | Unique |
| City | VARCHAR(50) | Optional |
| CreatedAt | DATETIME2 | Default to the current date and time |

**Expected outcome**

The table should be created with all six columns and their specified constraints.

**Solution**

```sql
CREATE TABLE dbo.Customers
(
    CustomerID INT PRIMARY KEY,
    FirstName VARCHAR(50) NOT NULL,
    LastName VARCHAR(50) NOT NULL,
    Email VARCHAR(100) UNIQUE,
    City VARCHAR(50) NULL,
    CreatedAt DATETIME2 NOT NULL
        DEFAULT SYSDATETIME()
);
```

**Explanation**

- `PRIMARY KEY` uniquely identifies each customer and prevents NULL values.
- `NOT NULL` requires a value.
- `UNIQUE` prevents duplicate non-NULL email values under the applicable SQL Server constraint rules.
- `DEFAULT` supplies a value when the column is omitted from an insert.
- `DATETIME2` supports date and time values with configurable fractional-second precision.

### Exercise 2.2: Create a Products table

**Task**

Create a table named `Products` with:

- `ProductID`: integer primary key.
- `ProductName`: required text, maximum 100 characters.
- `Price`: decimal with two decimal places, required.
- `StockQuantity`: integer, required, default value of zero.
- `Category`: text, maximum 50 characters.

**Expected outcome**

The table should enforce the required fields and supply zero when `StockQuantity` is omitted.

**Solution**

```sql
CREATE TABLE dbo.Products
(
    ProductID INT PRIMARY KEY,
    ProductName VARCHAR(100) NOT NULL,
    Price DECIMAL(10,2) NOT NULL,
    StockQuantity INT NOT NULL
        DEFAULT 0,
    Category VARCHAR(50) NULL
);
```

**Explanation**

`DECIMAL(10,2)` allows up to ten total digits, including two digits after the decimal point. This makes it suitable for storing prices when the required precision fits the chosen range.

### Exercise 2.3: Inspect table structures

**Task**

Retrieve the column definitions for `Customers` and `Products`.

**Solution**

```sql
EXEC sp_help 'dbo.Customers';
EXEC sp_help 'dbo.Products';
```

You can also use:

```sql
SELECT
    TABLE_NAME,
    COLUMN_NAME,
    DATA_TYPE,
    IS_NULLABLE,
    CHARACTER_MAXIMUM_LENGTH,
    NUMERIC_PRECISION,
    NUMERIC_SCALE
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_SCHEMA = 'dbo'
  AND TABLE_NAME IN ('Customers', 'Products')
ORDER BY TABLE_NAME, ORDINAL_POSITION;
```

**Expected outcome**

You should see each column's data type, nullability, and applicable length or numeric precision.

---

## 3. Primary Keys, Foreign Keys, and Constraints

### Exercise 3.1: Create an Orders table

**Task**

Create an `Orders` table with these requirements:

- `OrderID`: primary key.
- `CustomerID`: required integer referencing `Customers`.
- `OrderDate`: required date and time, default to the current date and time.
- `OrderStatus`: required text, default to `Pending`.
- `TotalAmount`: required decimal value.

**Solution**

```sql
CREATE TABLE dbo.Orders
(
    OrderID INT PRIMARY KEY,
    CustomerID INT NOT NULL,
    OrderDate DATETIME2 NOT NULL
        DEFAULT SYSDATETIME(),
    OrderStatus VARCHAR(20) NOT NULL
        DEFAULT 'Pending',
    TotalAmount DECIMAL(12,2) NOT NULL,

    CONSTRAINT FK_Orders_Customers
        FOREIGN KEY (CustomerID)
        REFERENCES dbo.Customers(CustomerID)
);
```

**Expected outcome**

The table should be created with a foreign key linking orders to customers.

A customer must exist before an order referencing that customer can be inserted.

### Exercise 3.2: Add a CHECK constraint

**Task**

Modify the `Products` table so that a product cannot have a negative price.

**Solution**

```sql
ALTER TABLE dbo.Products
ADD CONSTRAINT CK_Products_Price
CHECK (Price >= 0);
```

**Expected outcome**

An attempt to insert or update a product with a negative price should fail.

### Exercise 3.3: Add a UNIQUE constraint

**Task**

Create an `Employees` table with an employee ID, name, and unique email address.

**Solution**

```sql
CREATE TABLE dbo.Employees
(
    EmployeeID INT PRIMARY KEY,
    EmployeeName VARCHAR(100) NOT NULL,
    Email VARCHAR(100) NOT NULL,

    CONSTRAINT UQ_Employees_Email
        UNIQUE (Email)
);
```

**Expected outcome**

Two employees cannot share the same email address.

### Exercise 3.4: Test constraint violations

**Task**

Try inserting a product with a negative price.

**Solution**

```sql
INSERT INTO dbo.Products
    (ProductID, ProductName, Price, StockQuantity, Category)
VALUES
    (999, 'Invalid Product', -10.00, 5, 'Testing');
```

**Expected outcome**

SQL Server should reject the insert because it violates `CK_Products_Price`.

**Learning point:** Constraints protect data quality at the database level, even when data is inserted by different applications or users.

---

## 4. INSERT Statement Practice

### Exercise 4.1: Insert a single customer

**Task**

Insert the following customer:

- Customer ID: 1
- First name: Rahul
- Last name: Sharma
- Email: rahul.sharma@example.com
- City: Hyderabad

**Solution**

```sql
INSERT INTO dbo.Customers
    (CustomerID, FirstName, LastName, Email, City)
VALUES
    (1, 'Rahul', 'Sharma',
     'rahul.sharma@example.com', 'Hyderabad');
```

**Expected outcome**

One customer should be inserted. `CreatedAt` should receive its default value.

### Exercise 4.2: Insert multiple customers

**Task**

Insert two additional customers.

| CustomerID | FirstName | LastName | Email | City |
|---:|---|---|---|---|
| 2 | Priya | Reddy | priya.reddy@example.com | Chennai |
| 3 | Arjun | Kumar | arjun.kumar@example.com | Bengaluru |

**Solution**

```sql
INSERT INTO dbo.Customers
    (CustomerID, FirstName, LastName, Email, City)
VALUES
    (2, 'Priya', 'Reddy',
     'priya.reddy@example.com', 'Chennai'),
    (3, 'Arjun', 'Kumar',
     'arjun.kumar@example.com', 'Bengaluru');
```

**Expected outcome**

Three customers should now exist in the table.

### Exercise 4.3: Insert data into selected columns

**Task**

Insert a customer without specifying the `City` column.

**Solution**

```sql
INSERT INTO dbo.Customers
    (CustomerID, FirstName, LastName, Email)
VALUES
    (4, 'Sneha', 'Rao', 'sneha.rao@example.com');
```

**Expected outcome**

The customer should be inserted, and `City` should be `NULL`.

**Learning point:** An omitted column receives its default value if one is defined; otherwise, it receives `NULL` only if the column permits it. If the column is required and has no default, the insert fails.

### Exercise 4.4: Insert products

**Task**

Insert three products.

| ProductID | ProductName | Price | StockQuantity | Category |
|---:|---|---:|---:|---|
| 101 | Laptop | 65000.00 | 10 | Electronics |
| 102 | Mouse | 750.00 | 50 | Accessories |
| 103 | Keyboard | 1500.00 | 25 | Accessories |

**Solution**

```sql
INSERT INTO dbo.Products
    (ProductID, ProductName, Price, StockQuantity, Category)
VALUES
    (101, 'Laptop', 65000.00, 10, 'Electronics'),
    (102, 'Mouse', 750.00, 50, 'Accessories'),
    (103, 'Keyboard', 1500.00, 25, 'Accessories');
```

**Expected outcome**

Three products should be available for subsequent exercises.

### Exercise 4.5: Use a default value

**Task**

Insert a product without specifying `StockQuantity`.

**Solution**

```sql
INSERT INTO dbo.Products
    (ProductID, ProductName, Price, Category)
VALUES
    (104, 'USB Cable', 250.00, 'Accessories');
```

**Expected outcome**

The new product should have `StockQuantity = 0`.

---

## 5. UPDATE Statement Practice

> **Safety rule:** Always check which rows will be affected before executing an `UPDATE`. A missing `WHERE` clause can modify every row in the table.

### Exercise 5.1: Update a customer's city

**Task**

Change Rahul Sharma's city from Hyderabad to Pune.

**Solution**

```sql
UPDATE dbo.Customers
SET City = 'Pune'
WHERE CustomerID = 1;
```

**Expected outcome**

Only the customer with ID 1 should be updated.

### Exercise 5.2: Update a product's price

**Task**

Increase the price of the Mouse by 10%.

**Solution**

```sql
UPDATE dbo.Products
SET Price = Price * 1.10
WHERE ProductID = 102;
```

**Expected outcome**

The Mouse price should become `825.00`.

### Exercise 5.3: Update multiple columns

**Task**

Change the Keyboard's price to `1600.00` and its stock quantity to `30`.

**Solution**

```sql
UPDATE dbo.Products
SET
    Price = 1600.00,
    StockQuantity = 30
WHERE ProductID = 103;
```

**Expected outcome**

Only the Keyboard record should be modified.

### Exercise 5.4: Update using a condition

**Task**

Increase the price of every product in the Accessories category by 5%.

**Solution**

```sql
UPDATE dbo.Products
SET Price = Price * 1.05
WHERE Category = 'Accessories';
```

**Expected outcome**

All matching Accessories products should receive the price increase. Other categories should remain unchanged.

**Discussion:** How many rows should this update affect? Check the table contents before and after executing it.

---

## 6. DELETE Statement Practice

> **Safety rule:** `DELETE` without a `WHERE` clause removes every row from the target table. Review the matching rows first.

### Exercise 6.1: Delete a specific product

**Task**

Delete the USB Cable product using its product ID.

**Solution**

```sql
SELECT *
FROM dbo.Products
WHERE ProductID = 104;
```

After verifying the result:

```sql
DELETE FROM dbo.Products
WHERE ProductID = 104;
```

**Expected outcome**

The product with ID 104 should be removed.

### Exercise 6.2: Delete using a condition

**Task**

Delete products whose stock quantity is zero.

**Solution**

First inspect the affected records:

```sql
SELECT *
FROM dbo.Products
WHERE StockQuantity = 0;
```

Then execute:

```sql
DELETE FROM dbo.Products
WHERE StockQuantity = 0;
```

**Expected outcome**

Only products with zero stock should be deleted.

### Exercise 6.3: Understand foreign key restrictions

**Task**

Attempt to delete Rahul's customer record while an order references that customer.

**Solution**

First insert an order:

```sql
INSERT INTO dbo.Orders
    (OrderID, CustomerID, TotalAmount)
VALUES
    (5001, 1, 65000.00);
```

Then attempt the deletion:

```sql
DELETE FROM dbo.Customers
WHERE CustomerID = 1;
```

**Expected outcome**

SQL Server should reject the deletion because `Orders` contains a referencing record and the foreign key does not specify cascading deletes.

**Learning point:** Referential integrity prevents orphaned records. A referenced customer cannot be deleted until the dependent records are handled appropriately.

---

## 7. ALTER TABLE Practice

### Exercise 7.1: Add a column

**Task**

Add a `PhoneNumber` column to the `Customers` table. It should allow up to 20 characters and permit `NULL`.

**Solution**

```sql
ALTER TABLE dbo.Customers
ADD PhoneNumber VARCHAR(20) NULL;
```

**Expected outcome**

The table should contain a new `PhoneNumber` column.

### Exercise 7.2: Change a column's data type

**Task**

Change `PhoneNumber` to allow up to 30 characters.

**Solution**

```sql
ALTER TABLE dbo.Customers
ALTER COLUMN PhoneNumber VARCHAR(30) NULL;
```

**Expected outcome**

The column should support values up to 30 characters.

### Exercise 7.3: Add a CHECK constraint

**Task**

Create an `Age` column on the `Employees` table and ensure that age values must be at least 18.

**Solution**

```sql
ALTER TABLE dbo.Employees
ADD Age INT NULL;

ALTER TABLE dbo.Employees
ADD CONSTRAINT CK_Employees_Age
CHECK (Age IS NULL OR Age >= 18);
```

**Expected outcome**

An employee may have an unknown age (`NULL`), but any non-NULL age must be at least 18.

**Learning point:** Before adding a constraint to an existing table, check whether existing rows satisfy the new rule.

---

## 8. DELETE vs TRUNCATE vs DROP

### Exercise 8.1: Compare the commands

**Task**

Explain the difference between `DELETE`, `TRUNCATE TABLE`, and `DROP TABLE`.

**Solution**

| Command | Effect | `WHERE` supported? | Table structure remains? |
|---|---|---|---|
| `DELETE` | Removes selected or all rows | Yes | Yes |
| `TRUNCATE TABLE` | Removes all rows by deallocating data pages | No | Yes |
| `DROP TABLE` | Removes the table definition and its data | No | No |

Additional SQL Server considerations:

- `DELETE` logs row deletions and can activate applicable delete triggers.
- `TRUNCATE TABLE` resets an identity counter to its seed when applicable, but cannot be used on a table referenced by a foreign key.
- `DROP TABLE` removes the table object, including its data and associated object definitions.
- These commands can participate in transactions in SQL Server, but rollback is possible only while the transaction remains open and the operation has not been committed.

### Exercise 8.2: Practise TRUNCATE safely

**Task**

Create a temporary practice table, insert sample rows, and remove all rows using `TRUNCATE TABLE`.

**Solution**

```sql
CREATE TABLE dbo.TruncatePractice
(
    ID INT IDENTITY(1,1) PRIMARY KEY,
    Description VARCHAR(100)
);

INSERT INTO dbo.TruncatePractice (Description)
VALUES
    ('First row'),
    ('Second row'),
    ('Third row');

SELECT * FROM dbo.TruncatePractice;

TRUNCATE TABLE dbo.TruncatePractice;

SELECT * FROM dbo.TruncatePractice;
```

**Expected outcome**

The first `SELECT` returns three rows. The second returns zero rows, while the table still exists.

### Exercise 8.3: Practise DROP safely

**Task**

Remove the practice table created in Exercise 8.2.

**Solution**

```sql
DROP TABLE IF EXISTS dbo.TruncatePractice;
```

**Expected outcome**

The table should no longer exist.

**Important:** Do not use `DROP DATABASE` or destructive commands on databases containing important data.

---

## 9. Temporary Tables

### Exercise 9.1: Create a local temporary table

**Task**

Create a temporary table named `#TempProducts` containing three products.

**Solution**

```sql
CREATE TABLE #TempProducts
(
    ProductID INT,
    ProductName VARCHAR(100),
    Price DECIMAL(10,2)
);

INSERT INTO #TempProducts
    (ProductID, ProductName, Price)
VALUES
    (201, 'Monitor', 12000.00),
    (202, 'Webcam', 2500.00),
    (203, 'Headset', 1800.00);

SELECT * FROM #TempProducts;
```

**Expected outcome**

The temporary table should contain three rows and be available in the current SQL Server session.

### Exercise 9.2: Create a temporary table using SELECT INTO

**Task**

Copy all current products into a temporary table using `SELECT INTO`.

**Solution**

```sql
SELECT *
INTO #ProductsCopy
FROM dbo.Products;

SELECT * FROM #ProductsCopy;
```

**Expected outcome**

`#ProductsCopy` should contain the product rows and column definitions derived from the source query.

**Learning point:** `SELECT INTO` copies the selected columns and data, but does not automatically reproduce every source constraint, index, trigger, or other object property.

### Exercise 9.3: Understand temporary table scope

**Task**

Explain the difference between local and global temporary tables.

**Solution**

| Feature | Local temporary table | Global temporary table |
|---|---|---|
| Example | `#TempProducts` | `##SharedProducts` |
| Visibility | Current session | Multiple sessions, subject to SQL Server lifetime rules |
| Typical use | Session-specific work | Shared temporary work |
| Lifetime | Normally until the creating session ends, or until explicitly dropped | Remains until the creating session ends and concurrent references have finished |

**Example**

```sql
CREATE TABLE ##SharedProducts
(
    ProductID INT,
    ProductName VARCHAR(100)
);

INSERT INTO ##SharedProducts
VALUES (1, 'Shared Example');

SELECT * FROM ##SharedProducts;

DROP TABLE ##SharedProducts;
```

**Expected outcome**

The global temporary table can be accessed by other sessions while it exists and is visible to them.

**Caution:** Global temporary tables can introduce concurrency and naming conflicts. Use them only when cross-session sharing is genuinely needed.

---

## 10. Copying and Cloning Tables

### Exercise 10.1: Copy selected data into a new table

**Task**

Create a new table containing only products priced above `1000.00`.

**Solution**

```sql
SELECT
    ProductID,
    ProductName,
    Price,
    StockQuantity,
    Category
INTO dbo.ExpensiveProducts
FROM dbo.Products
WHERE Price > 1000.00;
```

**Expected outcome**

The new table should contain only qualifying products.

### Exercise 10.2: Create an empty copy of a table's columns

**Task**

Create a new table with the same column names and data types as `Products`, without copying its rows.

**Solution**

```sql
SELECT TOP (0)
    ProductID,
    ProductName,
    Price,
    StockQuantity,
    Category
INTO dbo.ProductsStructureCopy
FROM dbo.Products;
```

**Expected outcome**

The new table should have the selected columns and zero rows.

**Learning point:** SQL Server does not support the generic `CREATE TABLE new_table LIKE existing_table` syntax used by some other database systems. `SELECT INTO` is useful for copying query results, but it does not create a complete clone of all source table metadata.

---

## 11. Integrated Mini-Project: Retail Database

In this mini-project, you will create a small retail database structure and practise the concepts from this module.

### Exercise 11.1: Create an OrderItems table

**Task**

Create an `OrderItems` table with:

- `OrderItemID`: primary key.
- `OrderID`: foreign key referencing `Orders`.
- `ProductID`: foreign key referencing `Products`.
- `Quantity`: required and greater than zero.
- `UnitPrice`: required and non-negative.

**Solution**

```sql
CREATE TABLE dbo.OrderItems
(
    OrderItemID INT PRIMARY KEY,
    OrderID INT NOT NULL,
    ProductID INT NOT NULL,
    Quantity INT NOT NULL,
    UnitPrice DECIMAL(10,2) NOT NULL,

    CONSTRAINT CK_OrderItems_Quantity
        CHECK (Quantity > 0),

    CONSTRAINT CK_OrderItems_UnitPrice
        CHECK (UnitPrice >= 0),

    CONSTRAINT FK_OrderItems_Orders
        FOREIGN KEY (OrderID)
        REFERENCES dbo.Orders(OrderID),

    CONSTRAINT FK_OrderItems_Products
        FOREIGN KEY (ProductID)
        REFERENCES dbo.Products(ProductID)
);
```

**Expected outcome**

The table should enforce valid quantities, prices, and references to existing orders and products.

### Exercise 11.2: Insert order items

**Task**

Insert two items for order `5001`:

| OrderItemID | OrderID | ProductID | Quantity | UnitPrice |
|---:|---:|---:|---:|---:|
| 1 | 5001 | 101 | 1 | 65000.00 |
| 2 | 5001 | 102 | 2 | 750.00 |

**Solution**

```sql
INSERT INTO dbo.OrderItems
    (OrderItemID, OrderID, ProductID, Quantity, UnitPrice)
VALUES
    (1, 5001, 101, 1, 65000.00),
    (2, 5001, 102, 2, 750.00);
```

**Expected outcome**

Two order-item records should be inserted, assuming the referenced order and products exist.

### Exercise 11.3: Update stock quantity

**Task**

Reduce stock by one for product `101`.

**Solution**

```sql
UPDATE dbo.Products
SET StockQuantity = StockQuantity - 1
WHERE ProductID = 101
  AND StockQuantity >= 1;
```

**Expected outcome**

The stock quantity should decrease by one if the product exists and has at least one item in stock.

**Discussion:** Why is the stock condition included in the `WHERE` clause?

### Exercise 11.4: Validate the mini-project

**Task**

Write queries to:

1. Display all customers.
2. Display products with a price above `1000.00`.
3. Display orders for customer ID 1.
4. Display all items belonging to order ID 5001.
5. Count the number of rows in each main table.

**Solution**

```sql
-- 1. Customers
SELECT * FROM dbo.Customers;

-- 2. Products above 1000
SELECT *
FROM dbo.Products
WHERE Price > 1000.00;

-- 3. Orders for customer 1
SELECT *
FROM dbo.Orders
WHERE CustomerID = 1;

-- 4. Items for order 5001
SELECT *
FROM dbo.OrderItems
WHERE OrderID = 5001;

-- 5. Table row counts
SELECT 'Customers' AS TableName, COUNT(*) AS TotalRows
FROM dbo.Customers
UNION ALL
SELECT 'Products', COUNT(*) FROM dbo.Products
UNION ALL
SELECT 'Orders', COUNT(*) FROM dbo.Orders
UNION ALL
SELECT 'OrderItems', COUNT(*) FROM dbo.OrderItems;
```

**Expected outcome**

Each query should return the data matching its purpose. Row counts will depend on which exercises you have executed.

---

## 12. Final Assessment

Attempt these questions independently before reviewing the earlier solutions.

### Beginner Level

1. Create a table called `Departments` with a primary key and a required department name.
2. Insert three departments.
3. Add a location column to the table.
4. Update the location of one department.
5. Delete one department by its ID.

### Intermediate Level

6. Create an `Inventory` table with a primary key, product name, quantity, and unit price.
7. Add a constraint that prevents negative quantities and prices.
8. Insert multiple inventory records in one statement.
9. Increase the price of all products in a selected category by 8%.
10. Create a temporary table containing inventory items with a quantity below five.
11. Copy inventory records into a new permanent table using `SELECT INTO`.
12. Explain why dropping a parent record may fail when a foreign key references it.

### Assessment Solutions

**1. Create Departments**

```sql
CREATE TABLE dbo.Departments
(
    DepartmentID INT PRIMARY KEY,
    DepartmentName VARCHAR(100) NOT NULL
);
```

**2. Insert departments**

```sql
INSERT INTO dbo.Departments
    (DepartmentID, DepartmentName)
VALUES
    (1, 'Engineering'),
    (2, 'Finance'),
    (3, 'Operations');
```

**3. Add a location column**

```sql
ALTER TABLE dbo.Departments
ADD Location VARCHAR(100) NULL;
```

**4. Update one department**

```sql
UPDATE dbo.Departments
SET Location = 'Hyderabad'
WHERE DepartmentID = 1;
```

**5. Delete one department**

```sql
DELETE FROM dbo.Departments
WHERE DepartmentID = 3;
```

**6. Create Inventory**

```sql
CREATE TABLE dbo.Inventory
(
    InventoryID INT PRIMARY KEY,
    ProductName VARCHAR(100) NOT NULL,
    Quantity INT NOT NULL,
    UnitPrice DECIMAL(10,2) NOT NULL
);
```

**7. Add validation constraints**

```sql
ALTER TABLE dbo.Inventory
ADD CONSTRAINT CK_Inventory_Quantity
CHECK (Quantity >= 0);

ALTER TABLE dbo.Inventory
ADD CONSTRAINT CK_Inventory_UnitPrice
CHECK (UnitPrice >= 0);
```

**8. Insert multiple inventory records**

```sql
INSERT INTO dbo.Inventory
    (InventoryID, ProductName, Quantity, UnitPrice)
VALUES
    (1, 'Laptop', 10, 65000.00),
    (2, 'Mouse', 40, 750.00),
    (3, 'Keyboard', 4, 1500.00);
```

**9. Increase prices in a selected category**

The `Inventory` table does not contain a category column, so add one first.

```sql
ALTER TABLE dbo.Inventory
ADD Category VARCHAR(50) NULL;

UPDATE dbo.Inventory
SET Category = 'Accessories'
WHERE InventoryID IN (2, 3);

UPDATE dbo.Inventory
SET UnitPrice = UnitPrice * 1.08
WHERE Category = 'Accessories';
```

**10. Create a temporary table for low-stock products**

```sql
SELECT *
INTO #LowStockInventory
FROM dbo.Inventory
WHERE Quantity < 5;

SELECT * FROM #LowStockInventory;
```

**11. Copy inventory into a new permanent table**

```sql
SELECT *
INTO dbo.InventoryBackup
FROM dbo.Inventory;
```

This creates a new table containing the selected source columns and rows. It does not automatically copy every constraint or index.

**12. Explain the foreign key restriction**

A parent row cannot normally be deleted while dependent child rows reference it through a foreign key. The child rows must be handled first, or the relationship must have an intentionally configured deletion rule such as `ON DELETE CASCADE`.

---

## 13. Safe SQL Execution Checklist

Before executing any script, verify the following:

- [ ] Confirm the selected database with `SELECT DB_NAME();`.
- [ ] Inspect existing records before running `UPDATE` or `DELETE`.
- [ ] Include a suitable `WHERE` clause when changing or deleting selected records.
- [ ] Check foreign key relationships before deleting parent records.
- [ ] Avoid `DROP`, `TRUNCATE`, or unrestricted `DELETE` on tables containing important data.
- [ ] Use a disposable practice database for destructive exercises.
- [ ] Validate results with `SELECT` statements.
- [ ] Remember that `SELECT INTO` creates a new table and fails if the destination table already exists.
- [ ] Clean up temporary or practice objects when no longer needed.

### Optional transaction-based practice

For a simple update that you want to inspect before keeping, you can use a transaction:

```sql
BEGIN TRANSACTION;

UPDATE dbo.Products
SET Price = Price * 1.05
WHERE ProductID = 102;

SELECT *
FROM dbo.Products
WHERE ProductID = 102;

-- Choose one:
-- COMMIT TRANSACTION;
-- ROLLBACK TRANSACTION;
```

Execute either `COMMIT TRANSACTION` to keep the change or `ROLLBACK TRANSACTION` to undo it before the transaction is committed. Do not leave transactions open unnecessarily.

---

## 14. Module 1 Completion Checklist

By the end of this module, you should be able to:

- [ ] Create and select a database.
- [ ] Create tables with suitable data types.
- [ ] Define primary and foreign keys.
- [ ] Apply constraints and default values.
- [ ] Insert single and multiple records.
- [ ] Update and delete records safely.
- [ ] Add columns and constraints using `ALTER TABLE`.
- [ ] Explain `DELETE`, `TRUNCATE`, and `DROP`.
- [ ] Create local and global temporary tables.
- [ ] Copy table data using `SELECT INTO`.
- [ ] Complete the retail mini-project without copying the solutions.


