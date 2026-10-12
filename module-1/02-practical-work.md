# Module 1: SQL Fundamentals - DDL & DML Basics

## Purpose

This module introduces the core concepts of database design and data manipulation. You will learn how to create databases and tables, insert and modify data, and understand the lifecycle of database objects.

---

## Sample Practice Data for This Module

Before practicing any DDL or DML commands, use this small sample setup.

```sql
CREATE DATABASE ecommerce_db;
USE ecommerce_db;

CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE,
    city VARCHAR(50),
    phone VARCHAR(20),
    is_active BOOLEAN DEFAULT TRUE
);

CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    category VARCHAR(50),
    price DECIMAL(10,2) NOT NULL CHECK (price > 0),
    stock_quantity INT DEFAULT 0,
    is_available BOOLEAN DEFAULT TRUE
);

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT NOT NULL,
    order_date DATE,
    total_amount DECIMAL(10,2) CHECK (total_amount > 0),
    order_status VARCHAR(20) DEFAULT 'Pending',
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

INSERT INTO customers (customer_id, customer_name, email, city, phone, is_active) VALUES
(1, 'Rahul Kumar', 'rahul@email.com', 'Hyderabad', '9876543210', TRUE),
(2, 'Neha Singh', 'neha@email.com', 'Chennai', '9123456789', TRUE),
(3, 'Kiran Patel', 'kiran@email.com', 'Bengaluru', NULL, FALSE),
(4, 'Amit Sharma', 'amit@email.com', 'Delhi', '9988776655', TRUE);

INSERT INTO products (product_id, product_name, category, price, stock_quantity, is_available) VALUES
(101, 'Laptop', 'Electronics', 45000.00, 12, TRUE),
(102, 'Mobile', 'Electronics', 25000.00, 25, TRUE),
(103, 'Chair', 'Furniture', 2500.00, 30, TRUE),
(104, 'Desk', 'Furniture', 5000.00, 8, FALSE),
(105, 'Keyboard', 'Electronics', 1200.00, 40, TRUE);

INSERT INTO orders (order_id, customer_id, order_date, total_amount, order_status) VALUES
(1, 1, '2024-01-12', 45000.00, 'Delivered'),
(2, 2, '2024-01-20', 25000.00, 'Processing'),
(3, 1, '2024-02-02', 5000.00, 'Shipped'),
(4, 4, '2024-02-15', 1200.00, 'Pending');
```

This sample data will be used throughout the module to explain database design, constraints, inserts, updates, and deletions.

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
VALUES (1, 'USB Cable');
```

This inserts a row where quantity becomes 0 and is_available becomes TRUE.

---

## 9. UPDATE Basics

UPDATE modifies existing rows.

### Syntax
```sql
UPDATE table_name
SET column1 = value1, column2 = value2
WHERE condition;
```

### Example
```sql
UPDATE customers
SET city = 'Hyderabad'
WHERE customer_id = 2;
```

### Important
- Without `WHERE`, all rows in the table would be updated.
- Use conditions carefully.

---

## 10. DELETE Basics

DELETE removes rows from a table.

### Syntax
```sql
DELETE FROM table_name
WHERE condition;
```

### Example
```sql
DELETE FROM customers
WHERE customer_id = 3;
```

### Important
- Without `WHERE`, all rows would be deleted.
- Use backups before deleting records.

---

## 11. ALTER TABLE

ALTER TABLE is used to add, modify, or remove columns.

### Add a Column
```sql
ALTER TABLE customers
ADD COLUMN last_login DATE;
```

### Rename a Column
```sql
ALTER TABLE customers
RENAME COLUMN city TO residence_city;
```

### Change Data Type
```sql
ALTER TABLE customers
ALTER COLUMN phone VARCHAR(25);
```

### Drop a Column
```sql
ALTER TABLE products
DROP COLUMN discount_percent;
```

---

## 12. DROP TABLE and TRUNCATE TABLE

### TRUNCATE
Removes all rows from a table but keeps the table structure.

```sql
TRUNCATE TABLE orders;
```

### DROP
Removes the full table definition and data.

```sql
DROP TABLE orders;
```

### Difference
- `TRUNCATE`: empties the table, keeps structure.
- `DROP`: removes the table completely.

---

## 13. Temporary Tables and Clones

### Temporary Table
Useful for intermediate working data.

```sql
CREATE TEMPORARY TABLE temp_products AS
SELECT * FROM products WHERE price > 2000;
```

### Clone / Backup Table
```sql
CREATE TABLE products_backup AS
SELECT * FROM products;
```

This copies the table structure and data into a new table.

---

## Summary
This module lays the foundation of database design and SQL manipulation. You now understand how to create databases, design tables, define constraints, insert data, and modify existing records safely.

---

## Interview Questions
1. What is the difference between a database and a table?
2. Why is a primary key important?
3. When would you use a foreign key?
4. What does NOT NULL do?
5. Why is a CHECK constraint useful?
6. What is the difference between INSERT and UPDATE?
7. Why should you avoid DELETE without WHERE?
8. What is the difference between TRUNCATE and DROP?


