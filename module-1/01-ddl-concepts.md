# Module 1: SQL Fundamentals - DDL & DML Basics

## Purpose

This module introduces the core concepts of database design and data manipulation. You will learn how to create databases and tables, insert and modify data, and understand the lifecycle of database objects. This foundation is essential for all SQL work.

---

## 1. Database and Schema Basics

### What is a Database?
A database is an organized collection of structured data stored electronically on a computer system.

Examples:
- Student management database
- E-commerce database
- Bank account database
- Hospital patient database

### What is a Schema?
A schema is the blueprint or structure that defines:
- What tables exist
- What columns are in each table
- What data types each column holds
- What relationships exist between tables

### What is a Table?
A table is a structured collection of data organized in rows and columns, similar to a spreadsheet.

Example:

| customer_id | customer_name | city      | email               |
|-------------|---------------|-----------|---------------------|
| 1           | Rahul         | Hyderabad | rahul@email.com     |
| 2           | Neha          | Chennai   | neha@email.com      |
| 3           | Kiran         | Bangalore | kiran@email.com     |

- Each row represents one record
- Each column represents one attribute
- Rows and columns together form the table structure

---

## 2. Table Structure Concepts

### Components of a Table

#### Row
One complete record in a table.

Example:
```
(1, Rahul, Hyderabad, rahul@email.com)
```

#### Column
A property or attribute.

Example:
```
customer_name
```

#### Field
The value at the intersection of a row and column.

Example:
```
Rahul
```

#### Record
One complete row of data.

### Why Structure Matters

Good table structure ensures:
- Data consistency
- Data integrity
- Efficient querying
- Reliable reporting
- Easy maintenance

---

## 3. Data Types

Choosing the correct data type is crucial. It ensures data is stored correctly and queries run efficiently.

### Integer Types
Used for whole numbers.

- `INT` or `INTEGER`: standard integer
- `SMALLINT`: smaller range
- `BIGINT`: larger range

Example:
```sql
customer_id INT
product_id INT
age SMALLINT
```

### Decimal Types
Used for numbers with decimal places, especially money.

- `DECIMAL(p,s)` or `NUMERIC(p,s)`
  - p = total digits
  - s = digits after decimal point

Example:
```sql
price DECIMAL(10,2)        -- Up to 99,999,999.99
discount DECIMAL(5,2)      -- Up to 999.99
```

### String Types
Used for text data.

- `CHAR(n)`: fixed-length string
- `VARCHAR(n)`: variable-length string
- `TEXT`: long text (varies by system)

Example:
```sql
customer_name VARCHAR(100)
email VARCHAR(255)
description TEXT
```

### Date and Time Types
Used for dates and timestamps.

- `DATE`: only date (YYYY-MM-DD)
- `TIME`: only time (HH:MM:SS)
- `TIMESTAMP`: date and time together

Example:
```sql
birth_date DATE
order_date DATE
created_at TIMESTAMP
last_updated TIMESTAMP
```

### Boolean Type
Used for true/false values.

- `BOOLEAN` or `BOOL`

Example:
```sql
is_active BOOLEAN
is_premium BOOLEAN
```

### Choosing the Right Data Type

| Data | Suitable Type |
|------|---------------|
| Customer ID | INT |
| Product Price | DECIMAL(10,2) |
| Customer Name | VARCHAR(100) |
| Order Date | DATE |
| Is Active | BOOLEAN |
| Account Balance | DECIMAL(15,2) |
| Phone Number | VARCHAR(20) |
| Email | VARCHAR(255) |

---

## 4. CREATE DATABASE

A database is a container that holds multiple tables.

### Syntax
```sql
CREATE DATABASE database_name;
```

### Example
```sql
CREATE DATABASE ecommerce_db;
```

### Use the Database
Before creating tables, you must select the database:

```sql
USE ecommerce_db;
```

Some systems use:
```sql
SELECT ecommerce_db;
```

Or:
```sql
SET DATABASE ecommerce_db;
```

### Important Notes
- Database name should be meaningful
- Use lowercase and underscores for clarity
- Each database is separate
- Multiple databases can exist on one server

---

## 5. CREATE TABLE with Constraints

A table is created with columns and constraints that define the structure and rules.

### Basic Syntax
```sql
CREATE TABLE table_name (
    column1 data_type constraint,
    column2 data_type constraint,
    column3 data_type constraint
);
```

### Simple Example
```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL,
    email VARCHAR(255),
    city VARCHAR(50)
);
```

### Example with Multiple Constraints
```sql
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    category VARCHAR(50),
    price DECIMAL(10,2) NOT NULL,
    stock_quantity INT DEFAULT 0
);
```

### Structure Breakdown
- `product_id INT PRIMARY KEY` — unique identifier
- `product_name VARCHAR(100) NOT NULL` — required text field
- `category VARCHAR(50)` — optional text field
- `price DECIMAL(10,2) NOT NULL` — required money field
- `stock_quantity INT DEFAULT 0` — defaults to 0 if not provided

---

## 6. PRIMARY KEY and FOREIGN KEY

### Primary Key
A primary key uniquely identifies each row in a table. No two rows can have the same primary key value.

#### Rules for Primary Key
- Must be unique
- Cannot be NULL
- Only one primary key per table
- Usually numeric (INT, BIGINT)

#### Example
```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100),
    email VARCHAR(255)
);
```

Here, `customer_id` is the primary key. Each customer has a unique ID.

### Foreign Key
A foreign key links one table to another. It creates a relationship between tables.

#### Rules for Foreign Key
- References the primary key of another table
- Ensures referential integrity
- Prevents orphaned records
- Can be NULL if not required

#### Example
```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```

Here, `customer_id` in orders table references `customer_id` in customers table.

This means:
- Each order belongs to a valid customer
- You cannot create an order for a non-existent customer
- Data integrity is maintained

#### Complete Example with Both Keys
```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL,
    email VARCHAR(255)
);

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT NOT NULL,
    order_date DATE,
    total_amount DECIMAL(10,2),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```

---

## 7. Constraints - Types and Uses

Constraints are rules that control what data can be stored in a table.

### NOT NULL Constraint
Ensures a column must always have a value.

```sql
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(100) NOT NULL,
    email VARCHAR(255) NOT NULL
);
```

Here, `emp_name` and `email` are required. You cannot insert a record without these values.

### UNIQUE Constraint
Ensures all values in a column are different.

```sql
CREATE TABLE users (
    user_id INT PRIMARY KEY,
    username VARCHAR(50) UNIQUE,
    email VARCHAR(255) UNIQUE
);
```

Here, no two users can have the same username or email.

### DEFAULT Constraint
Provides a default value if no value is provided.

```sql
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    is_available BOOLEAN DEFAULT TRUE,
    discount_percent DECIMAL(5,2) DEFAULT 0.00
);
```

If you don't specify `is_available`, it defaults to TRUE.
If you don't specify `discount_percent`, it defaults to 0.00.

### CHECK Constraint
Restricts values to meet a condition.

```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    quantity INT CHECK (quantity > 0),
    price DECIMAL(10,2) CHECK (price > 0)
);
```

Here:
- `quantity` must be greater than 0
- `price` must be greater than 0

You cannot insert an order with negative quantity or price.

### PRIMARY KEY Constraint
Uniquely identifies each row.

```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100)
);
```

### FOREIGN KEY Constraint
Links to another table's primary key.

```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```

### Complete Example with All Constraints
```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    age INT CHECK (age >= 18),
    is_active BOOLEAN DEFAULT TRUE
);
```

---

## 8. INSERT Basics

INSERT adds new records (rows) to a table.

### Single Insert
Insert one record at a time.

#### Syntax
```sql
INSERT INTO table_name (column1, column2, column3)
VALUES (value1, value2, value3);
```

#### Example
```sql
INSERT INTO customers (customer_id, customer_name, email, city)
VALUES (1, 'Rahul Kumar', 'rahul@email.com', 'Hyderabad');
```

### Insert All Columns
If you provide values for all columns, you can omit the column list.

```sql
INSERT INTO customers
VALUES (2, 'Neha Singh', 'neha@email.com', 'Chennai');
```

### Insert Selected Columns
If some columns are optional or have defaults, you can insert only specific columns.

```sql
INSERT INTO customers (customer_id, customer_name, email)
VALUES (3, 'Kiran Patel', 'kiran@email.com');
```

Here, `city` is not provided. If it has no default, it will be NULL.

### Multiple Inserts
Insert multiple records in one statement.

```sql
INSERT INTO customers (customer_id, customer_name, email, city)
VALUES
(1, 'Rahul Kumar', 'rahul@email.com', 'Hyderabad'),
(2, 'Neha Singh', 'neha@email.com', 'Chennai'),
(3, 'Kiran Patel', 'kiran@email.com', 'Bangalore'),
(4, 'Amit Sharma', 'amit@email.com', 'Delhi');
```

This is more efficient than single inserts.

### Multiple Inserts with Different Columns
You can insert different sets of columns in one statement (if supported).

```sql
INSERT INTO customers (customer_id, customer_name, email, city)
VALUES
(5, 'Priya Verma', 'priya@email.com', 'Pune'),
(6, 'Rajesh Gupta', 'rajesh@email.com', NULL),
(7, 'Meera Khan', 'meera@email.com', 'Lucknow');
```

Here, customer 6 has no city value (NULL).

### Insert Data for Few Columns Only
If columns are optional or have defaults, you don't need to provide all.

```sql
INSERT INTO products (product_id, product_name)
VALUES (101, 'Laptop');
```

Here, only `product_id` and `product_name` are provided. Other columns will be NULL or use their defaults.

Example with defaults:
```sql
CREATE TABLE inventory (
    item_id INT PRIMARY KEY,
    item_name VARCHAR(100) NOT NULL,
    quantity INT DEFAULT 0,
    is_available BOOLEAN DEFAULT TRUE
);

INSERT INTO inventory (item_id, item_name)
VALUES (1, 'Pen');
```

Here:
- `quantity` automatically becomes 0
- `is_available` automatically becomes TRUE

---

## 9. UPDATE Basics

UPDATE modifies existing records in a table.

### Single Update
Update one or more columns for matching rows.

#### Syntax
```sql
UPDATE table_name
SET column1 = value1, column2 = value2
WHERE condition;
```

#### Example
```sql
UPDATE customers
SET city = 'Pune'
WHERE customer_id = 1;
```

This changes the city of customer with ID 1 to Pune.

### Multiple Updates in One Column
Update a column for multiple rows.

```sql
UPDATE customers
SET is_active = FALSE
WHERE city = 'Delhi';
```

This marks all customers from Delhi as inactive.

### Multiple Column Updates
Update multiple columns for the same row.

```sql
UPDATE customers
SET email = 'neha.singh@email.com', city = 'Bangalore'
WHERE customer_id = 2;
```

This updates both email and city for customer 2.

### Multiple Updates with Different Conditions
Update different rows with different values.

```sql
UPDATE products
SET price = price * 0.90
WHERE category = 'Electronics';

UPDATE products
SET price = price * 0.95
WHERE category = 'Furniture';
```

The first query gives 10% discount to electronics.
The second query gives 5% discount to furniture.

### Update with Calculation
Use existing values in calculation.

```sql
UPDATE products
SET stock_quantity = stock_quantity - 1
WHERE product_id = 101;
```

This reduces the stock of product 101 by 1.

### Update Multiple Rows
Update multiple rows at once.

```sql
UPDATE orders
SET status = 'Shipped'
WHERE order_date < '2025-03-01';
```

This marks all orders before March 1st as Shipped.

### Important Safety Note
**Always use WHERE clause in UPDATE statements.**

Without WHERE, ALL rows will be updated.

❌ Dangerous:
```sql
UPDATE customers
SET email = 'new@email.com';
```

✅ Safe:
```sql
UPDATE customers
SET email = 'new@email.com'
WHERE customer_id = 1;
```

---

## 10. DELETE Statement

DELETE removes records from a table.

### Syntax
```sql
DELETE FROM table_name
WHERE condition;
```

### Single Delete
Delete one record.

```sql
DELETE FROM customers
WHERE customer_id = 4;
```

This removes customer with ID 4.

### Multiple Deletes
Delete multiple records based on a condition.

```sql
DELETE FROM orders
WHERE order_date < '2024-01-01';
```

This removes all orders from before 2024.

### Important Safety Note
**Always use WHERE clause in DELETE statements.**

Without WHERE, ALL rows will be deleted.

❌ Dangerous:
```sql
DELETE FROM customers;
```

✅ Safe:
```sql
DELETE FROM customers
WHERE customer_id = 5;
```

### Difference from TRUNCATE and DROP
- `DELETE` removes rows but keeps the table structure
- `TRUNCATE` removes all rows quickly
- `DROP` removes the entire table

---

## 11. ALTER TABLE

ALTER TABLE changes a table structure after creation.

### Add a Column
Add a new column to an existing table.

#### Syntax
```sql
ALTER TABLE table_name
ADD COLUMN column_name data_type;
```

#### Example
```sql
ALTER TABLE customers
ADD COLUMN phone_number VARCHAR(20);
```

This adds a phone_number column to the customers table.

### Add Column with Constraint
```sql
ALTER TABLE customers
ADD COLUMN phone_number VARCHAR(20) NOT NULL DEFAULT '0000000000';
```

### Delete a Column
Remove a column from a table.

#### Syntax
```sql
ALTER TABLE table_name
DROP COLUMN column_name;
```

#### Example
```sql
ALTER TABLE customers
DROP COLUMN phone_number;
```

This removes the phone_number column.

### Rename a Column
Change the name of a column.

#### Syntax (varies by database)
```sql
ALTER TABLE table_name
RENAME COLUMN old_name TO new_name;
```

#### Example
```sql
ALTER TABLE customers
RENAME COLUMN customer_name TO full_name;
```

Some systems use:
```sql
ALTER TABLE customers
CHANGE COLUMN customer_name full_name VARCHAR(100);
```

### Modify Column Data Type
Change the data type of a column.

#### Syntax (varies by database)
```sql
ALTER TABLE table_name
ALTER COLUMN column_name TYPE new_data_type;
```

#### Example
```sql
ALTER TABLE customers
ALTER COLUMN email TYPE VARCHAR(500);
```

This increases email column size to 500 characters.

Some systems use:
```sql
ALTER TABLE customers
MODIFY COLUMN email VARCHAR(500);
```

### Add Constraint to Existing Column
```sql
ALTER TABLE customers
ADD CONSTRAINT unique_email UNIQUE (email);
```

### Complete ALTER TABLE Examples
```sql
-- Add a column
ALTER TABLE customers
ADD COLUMN city VARCHAR(50) DEFAULT 'Unknown';

-- Modify column size
ALTER TABLE products
ALTER COLUMN product_name TYPE VARCHAR(200);

-- Drop a column
ALTER TABLE customers
DROP COLUMN phone_number;

-- Rename a column
ALTER TABLE orders
RENAME COLUMN total_amount TO order_total;
```

### Important Considerations
- ALTER TABLE changes can affect dependent queries and applications
- Test changes on a copy first
- Ensure backups exist before making changes
- Some changes may lock the table temporarily
- Large tables may take time to alter

---

## 12. DROP vs TRUNCATE vs DELETE

These three operations remove data, but they work differently.

### DELETE
Removes specific rows based on a condition.

```sql
DELETE FROM customers
WHERE city = 'Delhi';
```

**Characteristics:**
- Removes rows conditionally
- Table structure remains
- Can be rolled back in transactions
- Slower for large deletions
- Logs each row deletion
- WHERE clause is required for safety

### TRUNCATE
Removes all rows quickly without deleting the table.

```sql
TRUNCATE TABLE customers;
```

**Characteristics:**
- Removes ALL rows at once
- Table structure remains
- Very fast operation
- Cannot be filtered by WHERE clause
- Cannot be rolled back in some systems
- Resets identity/sequence values

### DROP
Removes the entire table including structure.

```sql
DROP TABLE customers;
```

**Characteristics:**
- Removes table completely
- Structure is deleted
- Very fast operation
- Cannot undo without backup
- Frees up storage space
- No WHERE clause needed

### Comparison Table

| Aspect | DELETE | TRUNCATE | DROP |
|--------|--------|----------|------|
| Removes rows | Yes | Yes | Yes |
| Removes structure | No | No | Yes |
| WHERE clause | Yes | No | No |
| Speed | Slow | Fast | Fast |
| Rollback possible | Yes | Varies | Only with backup |
| Frees space | Partially | Yes | Yes |
| Use case | Remove specific rows | Clear all data | Remove table |

### Examples

#### DELETE - Remove specific orders
```sql
DELETE FROM orders
WHERE order_date < '2024-01-01';
```

#### TRUNCATE - Clear all temporary data
```sql
TRUNCATE TABLE temp_orders;
```

#### DROP - Remove entire table
```sql
DROP TABLE archived_orders;
```

---

## 13. Temporary Tables

Temporary tables are tables that exist temporarily for processing and are automatically removed.

### Purpose
- Store intermediate results
- Test schema without affecting production
- Process data during development
- Reduce clutter in the database

### Create Temporary Table

#### Syntax
```sql
CREATE TEMPORARY TABLE temp_table_name (
    column1 data_type,
    column2 data_type
);
```

#### Example
```sql
CREATE TEMPORARY TABLE temp_order_summary (
    customer_id INT,
    total_orders INT,
    total_amount DECIMAL(10,2)
);
```

### Insert Data into Temporary Table
```sql
INSERT INTO temp_order_summary
VALUES (1, 5, 15000);
```

### Query Temporary Table
```sql
SELECT * FROM temp_order_summary;
```

### Scope and Lifetime
- Available only in the current session
- Automatically deleted when session ends
- Not visible to other users
- Behavior varies by database system

### Difference Between Temporary and Regular Tables
- Temporary tables are temporary
- Regular tables are permanent
- Temporary tables don't consume permanent storage
- Temporary tables are useful for testing

---

## 14. Cloning Tables

Cloning creates a copy of a table structure and data for testing or backup purposes.

### Clone Table Structure Only
Copy only the structure without data.

```sql
CREATE TABLE customers_empty LIKE customers;
```

Or:
```sql
CREATE TABLE customers_empty AS
SELECT * FROM customers
WHERE 1=0;
```

The second method creates the structure without any rows.

### Clone Table with Data
Copy both structure and data.

```sql
CREATE TABLE customers_backup AS
SELECT * FROM customers;
```

This creates a backup copy with all existing data.

### Clone Specific Rows
Copy only certain records.

```sql
CREATE TABLE active_customers AS
SELECT * FROM customers
WHERE is_active = TRUE;
```

This creates a new table with only active customers.

### Clone Specific Columns
Copy only certain columns.

```sql
CREATE TABLE customer_names AS
SELECT customer_id, customer_name FROM customers;
```

This creates a new table with only ID and name.

### Use Cases
- **Testing:** Test schema changes on a clone before applying to production
- **Backup:** Create backup copies of important tables
- **Archiving:** Move old data to archived tables
- **Reporting:** Create tables with specific data subsets

### Example Workflow
```sql
-- 1. Create a backup before making changes
CREATE TABLE products_backup AS
SELECT * FROM products;

-- 2. Modify the original table structure
ALTER TABLE products
ADD COLUMN expiry_date DATE;

-- 3. Test the changes
INSERT INTO products (product_id, product_name, expiry_date)
VALUES (999, 'Test Product', '2025-12-31');

-- 4. If satisfied, continue; if not, restore from backup
-- RESTORE from products_backup
```

---

## 15. Summary and Learning Outcomes

### Key Concepts Learned

#### Database and Table Structure
- Databases organize data logically
- Tables store data in rows and columns
- Schema defines the structure

#### Data Types
- Use INT for numbers
- Use DECIMAL for money
- Use VARCHAR for text
- Use DATE for dates
- Use BOOLEAN for true/false

#### DDL Operations
- `CREATE DATABASE` creates a database
- `CREATE TABLE` creates a table with structure
- `ALTER TABLE` modifies table structure
- `DROP` removes entire table
- `TRUNCATE` removes all rows

#### DML Operations
- `INSERT` adds rows
- `UPDATE` modifies rows
- `DELETE` removes rows

#### Constraints
- PRIMARY KEY uniquely identifies rows
- FOREIGN KEY links tables
- NOT NULL requires a value
- UNIQUE ensures no duplicates
- DEFAULT provides default values
- CHECK enforces conditions

#### Safety Practices
- Always use WHERE in UPDATE and DELETE
- Test changes on clones first
- Backup important tables
- Use TRUNCATE for large deletions
- Use DELETE for selective removals

### Expected Capabilities
- Create databases and tables
- Insert and update records
- Delete unwanted data
- Alter table structures safely
- Clone tables for testing
- Understand constraints and their purpose

### Common Interview Questions
1. What is the difference between DELETE and TRUNCATE?
2. Why is a primary key important?
3. What does a foreign key do?
4. When should you use NOT NULL?
5. How do you add a new column to an existing table?
6. What is the purpose of temporary tables?
7. Why should you clone a table before testing schema changes?

---

## 16. Next Steps

The next files in this module will provide:
- **Practical Work:** Step-by-step tasks to practice these concepts
- **Post-Practice Scenarios:** Real-world business cases to apply learning

This module gives you the foundation for all SQL work.
