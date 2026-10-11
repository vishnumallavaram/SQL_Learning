# Module 1: Data Definition (DDL) & Table Lifecycle

## Purpose

This module introduces the foundation of relational databases: how data is structured, how tables are created, and how those structures evolve over time. By the end of this module, a learner should be able to understand the building blocks of a database and design a simple schema for a business use case.

---

## 1. Relational Database Fundamentals

A relational database stores data in tables. Each table contains rows and columns. Rows represent records, and columns represent attributes or fields.

### Key objects in a relational database

- Database: a container for related data.
- Schema: a logical grouping of database objects such as tables, views, and procedures.
- Table: a structured collection of records.
- Row: one record in a table.
- Column: a property or attribute of the record.
- Constraint: rules that control what data can be stored.

### Example

A simple customer table:

| customer_id | customer_name | city        | age |
|-------------|---------------|-------------|-----|
| 1           | Rahul         | Hyderabad   | 25  |
| 2           | Neha          | Chennai     | 30  |
| 3           | Kiran         | Bengaluru   | 28  |

Here:
- `customer_id` is a column
- `Rahul` is a row value
- `customer_name` is an attribute
- The table stores structured relational data

---

## 2. Why Structure Matters

Data is more useful when it is organized logically.

A database structure helps with:

- Data consistency
- Faster lookup and filtering
- Better reporting and analysis
- Easier updates and maintenance
- Cleaner data relationships between business entities

Example business entities:
- Customers
- Products
- Orders
- Employees
- Payments

These entities are usually represented as tables and connected using relationships.

---

## 3. Data Types

Choosing the correct data type is important. It ensures the database stores valid and meaningful values.

### Common SQL data types

#### Integer types
- `INT` or `INTEGER`: whole numbers
- `SMALLINT`: smaller whole numbers
- `BIGINT`: large whole numbers

Example:
```sql
customer_id INT
stock_count BIGINT
```

#### Decimal types
- `DECIMAL(p,s)` or `NUMERIC(p,s)`: values with fixed precision
- Useful for money and percentages

Example:
```sql
price DECIMAL(10,2)
```

#### String types
- `CHAR(n)`: fixed-length strings
- `VARCHAR(n)`: variable-length strings
- `TEXT`: long text data in some systems

Example:
```sql
customer_name VARCHAR(100)
email VARCHAR(255)
```

#### Date and time types
- `DATE`: only date
- `TIME`: only time
- `TIMESTAMP`: date and time together

Example:
```sql
order_date DATE
created_at TIMESTAMP
```

#### Boolean type
- `BOOLEAN` or `BOOL`: true/false values

Example:
```sql
is_active BOOLEAN
```

### Choosing the right type

- Use `INT` for IDs and counters
- Use `DECIMAL` for money and exact decimal values
- Use `VARCHAR` for names and text fields
- Use `DATE` for dates
- Use `TIMESTAMP` for audit or event tracking
- Use `BOOLEAN` for yes/no or enabled/disabled flags

---

## 4. CREATE Statement

The `CREATE` statement is used to create database objects such as databases, schemas, and tables.

### Create a database

In many systems:
```sql
CREATE DATABASE company_db;
```

Some systems require selecting a database first:
```sql
USE company_db;
```

### Create a table

```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL,
    city VARCHAR(50),
    email VARCHAR(255) UNIQUE,
    is_active BOOLEAN DEFAULT TRUE
);
```

### Explanation
- `customer_id` is the primary key
- `customer_name` is required (`NOT NULL`)
- `email` must be unique
- `is_active` defaults to `TRUE`

---

## 5. Table Design Basics

Designing a table is not just about adding columns. A good table should reflect the real business need.

### Important design concepts

#### Column names
Use clear, meaningful names.

Good:
- `customer_name`
- `order_date`
- `product_price`

Avoid:
- `x1`
- `data1`
- `temp_value`

#### Nullability
A column may allow null values or may require a value.

- `NOT NULL`: value is required
- nullable: value may be missing

Example:
```sql
customer_name VARCHAR(100) NOT NULL
middle_name VARCHAR(50) NULL
```

#### Primary key
A primary key uniquely identifies each row in a table.

Typical primary key choices:
- `customer_id`
- `product_id`
- `order_id`

Example:
```sql
customer_id INT PRIMARY KEY
```

#### Foreign key
A foreign key links one table to another.

Example:
```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```

This means each order belongs to an existing customer.

#### Constraints
Constraints enforce rules.

Examples:
- `PRIMARY KEY`
- `FOREIGN KEY`
- `NOT NULL`
- `UNIQUE`
- `CHECK`
- `DEFAULT`

---

## 6. Creating Tables from Results or Existing Structures

Some databases support creating a table using a query result or copying an existing table structure.

### Create table from a query result

```sql
CREATE TABLE active_customers AS
SELECT customer_id, customer_name, city
FROM customers
WHERE is_active = TRUE;
```

This creates a new table with the selected rows and columns.

### Create a table like an existing one

Some database platforms support a structure-copy pattern.

Example conceptually:
```sql
CREATE TABLE new_customers LIKE customers;
```

This copies the column definitions and structure, but behavior differs by database system.

Important note:
- Some systems support this directly
- Others require a different approach
- Syntax varies by database platform

---

## 7. ALTER TABLE

`ALTER TABLE` changes an existing table structure after creation.

Common operations:
- Add a column
- Drop a column
- Rename a column
- Modify column type or size
- Change constraints

### Add a column
```sql
ALTER TABLE customers
ADD COLUMN phone_number VARCHAR(20);
```

### Rename a column
Some databases support:
```sql
ALTER TABLE customers
RENAME COLUMN customer_name TO full_name;
```

### Modify a column
```sql
ALTER TABLE customers
ALTER COLUMN email TYPE VARCHAR(300);
```

### Drop a column
```sql
ALTER TABLE customers
DROP COLUMN phone_number;
```

### Important safety note
Changing table structure can affect application logic, reports, and dependent queries. Always understand the impact before altering production tables.

---

## 8. Temporary and Transient Objects

Some database systems support temporary tables or transient objects. These are useful for intermediate processing and may not persist after a session or a certain time window.

### Why use temporary objects?
- Store intermediate results
- Avoid modifying permanent tables while testing
- Reduce data cleanup complexity during development

### Example conceptual use
```sql
CREATE TEMPORARY TABLE temp_order_summary AS
SELECT customer_id, COUNT(*) AS total_orders
FROM orders
GROUP BY customer_id;
```

### Scope and lifetime
Temporary objects can behave differently depending on the platform:
- Available only in the current session
- Automatically removed at session end
- Not visible to other users

This is useful in testing and development but may behave differently in production environments.

---

## 9. DROP vs TRUNCATE vs DELETE

These commands may appear similar but have very different meanings.

### DELETE
Deletes rows from a table but keeps the table structure.

```sql
DELETE FROM customers WHERE customer_id = 3;
```

Features:
- removes rows
- table still exists
- can be filtered
- logs row deletions depending on system

### TRUNCATE
Removes all rows from a table quickly without deleting the table itself.

```sql
TRUNCATE TABLE customers;
```

Features:
- removes all records
- keeps table definition
- usually faster than deleting row-by-row
- may not be supported in all systems

### DROP
Removes the entire object from the database.

```sql
DROP TABLE customers;
```

Features:
- removes the table structure and data
- object is no longer available
- permanent unless restored from backup

### Comparison

| Operation | Removes rows | Removes table structure | Typical use |
|-----------|--------------|-------------------------|-------------|
| DELETE    | Yes          | No                      | Remove selected rows |
| TRUNCATE  | Yes          | No                      | Clear all data quickly |
| DROP      | Yes          | Yes                     | Remove the object completely |

---

## 10. Cloning and Table Lifecycle

A table lifecycle includes creation, testing, usage, modification, and cleanup.

### Cloning a table
Some systems support copying tables or creating a backup version for testing.

Example conceptually:
```sql
CREATE TABLE customers_backup AS
SELECT * FROM customers;
```

### Why clone tables?
- Test schema changes safely
- Create backup copies for review
- Validate migration logic before update

### Table retention and cleanup
Teams should consider:
- Whether a table is business-critical
- Whether backup versions should be kept temporarily
- Whether temporary objects need cleanup
- Whether data retention policies apply

### Lifecycle phases
1. Design schema
2. Create table
3. Insert data
4. Alter when needed
5. Test changes
6. Keep or archive valuable objects
7. Drop stale or unnecessary tables

---

## 11. Common Design Example: Customers, Products, Orders

A simple business scenario can be modeled with a few tables.

### Customer table
```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL,
    city VARCHAR(50),
    email VARCHAR(255) UNIQUE
);
```

### Product table
```sql
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100) NOT NULL,
    price DECIMAL(10,2) NOT NULL
);
```

### Orders table
```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT NOT NULL,
    order_date DATE,
    total_amount DECIMAL(10,2),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);
```

This schema supports a basic e-commerce flow.

---

## 12. Practical Design Tips for Freshers

When designing a table, ask:

- What is the unique identifier?
- What values must always be present?
- What values should be unique?
- What fields should be linked across tables?
- What data type is most suitable?
- Will this table change often?

### Good habits
- Use singular table names
- Use consistent naming conventions
- Keep IDs numeric and unique
- Avoid storing redundant data
- Document what each table represents

---

## 13. Dialect Differences

The exact syntax for database creation, transient tables, cloning, and some ALTER operations differs across database systems.

Examples of popular SQL dialects:
- PostgreSQL
- MySQL
- SQL Server
- Oracle
- Snowflake
- BigQuery

For example:
- Some systems support `CREATE TEMP TABLE ...`
- Others use `CREATE TEMPORARY TABLE ...`
- Some support `ALTER TABLE ... RENAME COLUMN ...`
- Others require a different syntax

This means learners should focus on the concepts first, then adapt the syntax to the database they are using.

---

## 14. Typical Interview / Learning Questions

1. What is the difference between a database and a schema?
2. What is a primary key and why is it important?
3. What is a foreign key?
4. Why is `NOT NULL` used?
5. When would you use `ALTER TABLE`?
6. What is the difference between `DROP`, `TRUNCATE`, and `DELETE`?
7. Why do we need temporary tables?
8. What happens if you choose the wrong data type?

---

## 15. Summary

In this module, you learned the building blocks of relational data design:

- What databases and tables are
- How columns and rows are organized
- How to choose appropriate data types
- How to create and alter tables
- How constraints help protect data quality
- The difference between `DELETE`, `TRUNCATE`, and `DROP`
- The purpose of temporary objects and lifecycle planning

These ideas are the foundation for working with SQL in real projects and database systems.

---

## 16. Next Step

The next file in this module will focus on practical SQL work:
- creating a basic customer/products/orders schema
- adding and modifying columns
- testing `DROP`, `TRUNCATE`, and `DELETE`
- documenting the behavior in a practice environment

This will help you move from theory to applied database work.
