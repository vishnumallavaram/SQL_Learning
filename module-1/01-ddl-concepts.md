# Module 1: SQL Fundamentals — DDL & DML Basics

## Purpose

This module introduces the core concepts of database design and data manipulation. You will learn how to create databases and tables, insert and modify data, and understand the lifecycle of database objects. This foundation is essential for all SQL work.

---

## 1. Database and Schema Basics

### What is a Database?

A database is a collection of data stored in a logical and structured manner.

**Examples:**
- Student management database
- E-commerce database
- Bank account database
- Hospital patient database

### What is a Schema?

A schema defines the structure and organization of database objects, including:
- Tables available in the schema
- Columns within each table
- Data types assigned to columns
- Relationships and constraints between tables

In simple words, a schema defines how a database is organized.

> Note: The relationship between databases and schemas varies by database system. In some systems, a database contains multiple schemas; in others, the terminology and organization differ.

### What is a Table?

A table is a structured collection of data organized into rows and columns, similar to a spreadsheet.

**Example:**

| customer_id | customer_name | city | email |
|---:|---|---|---|
| 1 | Rahul | Hyderabad | rahul@email.com |
| 2 | Neha | Chennai | neha@email.com |
| 3 | Kiran | Bangalore | kiran@email.com |

- Each row, also called a record, represents one complete entry.
- Each column represents an attribute or property.
- The combination of rows and columns organizes the table's data.

---

## 2. Table Structure Concepts

### Components of a Table

#### Row / Record

A row, also called a record, represents one complete entry in a table.

Example:

```text
(1, Rahul, Hyderabad, rahul@email.com)
```

#### Column

A column represents a property or attribute of the data.

Example:

```text
customer_name
```

#### Field

A field is the value at the intersection of a particular row and column.

Example:

```text
Rahul
```

### Why Does Table Structure Matter?

A well-structured table helps ensure:
- Data consistency
- Data integrity
- Efficient querying
- Reliable reporting
- Easier maintenance

---

## 3. Data Types

Choosing the correct data type helps ensure that data is stored appropriately and queries operate efficiently.

### Integer Types

Integer types are used to store whole numbers without decimal places.

| Data type | Description | Example |
|---|---|---|
| `SMALLINT` | Stores whole numbers within a smaller range | `age SMALLINT` |
| `INT` / `INTEGER` | Stores whole numbers within a standard range | `product_id INT` |
| `BIGINT` | Stores whole numbers within a larger range | `transaction_id BIGINT` |

**Example:**

```sql
CREATE TABLE transactions (
    transaction_id BIGINT PRIMARY KEY,
    customer_id INT,
    transaction_amount DECIMAL(12,2)
);
```

Here, `transaction_id` uses `BIGINT` because a transaction system may generate a very large number of identifiers over time.

The appropriate type depends on the expected range of values and the database system.

### Decimal Types

Decimal types are used to store precise numeric values with decimal places, especially financial data such as salaries, prices, account balances, and transactions.

- `DECIMAL(p,s)` or `NUMERIC(p,s)`
- `p` = total number of digits allowed
- `s` = number of digits allowed after the decimal point

**Example:**

```sql
price DECIMAL(10,2)
discount DECIMAL(5,2)
salary DECIMAL(12,2)
```

For `DECIMAL(10,2)`, up to 10 digits are allowed in total, with 2 digits after the decimal point. The maximum positive value is `99,999,999.99`.

### String Types

String types store text data.

| Data type | Description | Example |
|---|---|---|
| `CHAR(n)` | Fixed-length character data | `country_code CHAR(2)` |
| `VARCHAR(n)` | Variable-length character data | `customer_name VARCHAR(100)` |
| `TEXT` | Longer text; availability and limits vary by system | `description TEXT` |

**Example:**

```sql
CREATE TABLE customer_details (
    customer_id INT,
    customer_name VARCHAR(100),
    email VARCHAR(255),
    description TEXT
);
```

### Date and Time Types

These data types store dates and time-related values.

| Data type | Purpose | Example |
|---|---|---|
| `DATE` | Stores a calendar date | `order_date DATE` |
| `TIME` | Stores a time of day | `start_time TIME` |
| `TIMESTAMP` | Stores a date and time | `created_at TIMESTAMP` |

The supported range, precision, and time-zone behavior depend on the database system.

### Boolean Type

Boolean types represent true/false values where supported.

**Example:**

```sql
CREATE TABLE products (
    product_id INT,
    is_active BOOLEAN,
    is_available BOOLEAN
);
```

Some database systems represent Boolean values differently or use equivalent types.

### Choosing the Right Data Type

| Data | Suitable type |
|---|---|
| Customer ID | `INT` or `BIGINT` |
| Transaction ID | `BIGINT`, if the expected range requires it |
| Product price | `DECIMAL(10,2)` |
| Customer name | `VARCHAR(100)` |
| Order date | `DATE` |
| Active status | `BOOLEAN`, where supported |
| Account balance | `DECIMAL(15,2)` |
| Phone number | `VARCHAR(20)` |
| Email address | `VARCHAR(255)` |

**Important:** Phone numbers should generally be stored as text because they may contain leading zeros, country codes, or other formatting characters.

---

## 4. CREATE DATABASE

### What is CREATE DATABASE?

`CREATE DATABASE` is a SQL command used to create a new database. A database can contain tables and other database objects.

### Syntax

```sql
CREATE DATABASE database_name;
```

### Example

```sql
CREATE DATABASE ecommerce_db;
```

This creates a database named `ecommerce_db`, provided the database system supports this operation and the user has sufficient permissions.

### Using the Database

The method used to select or connect to a database depends on the database system.

For example, in MySQL:

```sql
USE ecommerce_db;
```

In PostgreSQL, you typically connect to the target database using your SQL client or connection settings. The `USE` command is not supported in PostgreSQL.

### Important Notes

- Choose meaningful database names.
- Use consistent naming conventions.
- Multiple databases can be hosted on a single server, depending on the database platform.
- Creating a database may require appropriate permissions.
- Database creation and database selection are separate operations.

---

## 5. CREATE TABLE with Constraints

### What is CREATE TABLE?

`CREATE TABLE` is a SQL command used to create a new table by defining its columns, data types, and applicable constraints.

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

This creates a `customers` table with four columns.

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

- `product_id INT PRIMARY KEY` — Uniquely identifies each product.
- `product_name VARCHAR(100) NOT NULL` — A required text field that cannot contain `NULL`.
- `category VARCHAR(50)` — A text field for the product category; `NULL` is allowed unless restricted separately.
- `price DECIMAL(10,2) NOT NULL` — A required numeric field suitable for prices with two decimal places.
- `stock_quantity INT DEFAULT 0` — Uses `0` when a value is omitted and the default applies.

A `NOT NULL` constraint requires a value, but it does not validate whether that value is meaningful or correct.

---

## 6. PRIMARY KEY and FOREIGN KEY

### Primary Key

A primary key uniquely identifies each row in a table.

#### Rules for a Primary Key

- Each primary key value must be unique.
- A primary key cannot contain `NULL`.
- A table can have only one primary key constraint, although that key can consist of multiple columns.
- The key can use a suitable data type, such as `INT`, `BIGINT`, or a character type.

#### Example

```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100),
    email VARCHAR(255)
);
```

Here, `customer_id` uniquely identifies each customer.

### Foreign Key

A foreign key establishes a relationship between tables and helps maintain referential integrity.

#### Key Points

- A foreign key references a primary key or another eligible unique key.
- It prevents references to nonexistent parent records when the constraint is enforced.
- It can contain `NULL` if the column allows it.
- The database may reject changes that would violate the relationship, depending on the constraint and its configured actions.

#### Example

Assume the `customers` table already exists.

```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    FOREIGN KEY (customer_id)
        REFERENCES customers(customer_id)
);
```

Here, `customer_id` in the `orders` table references `customer_id` in the `customers` table.

This means an order cannot reference a customer that does not exist, provided the foreign key is enforced and the reference is not `NULL`.

### Complete Example with Both Keys

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
    FOREIGN KEY (customer_id)
        REFERENCES customers(customer_id)
);
```

Create the parent table (`customers`) before creating the referencing table (`orders`).

---

## 7. Constraints — Types and Uses

Constraints are rules that control the data allowed in a table.

### NOT NULL Constraint

Ensures a column cannot contain `NULL`.

```sql
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(100) NOT NULL,
    email VARCHAR(255) NOT NULL
);
```

Here, `emp_name` and `email` must have values that are not `NULL`.

### UNIQUE Constraint

Ensures that values in a column must be unique, according to the database system's rules for unique constraints.

```sql
CREATE TABLE users (
    user_id INT PRIMARY KEY,
    username VARCHAR(50) UNIQUE,
    email VARCHAR(255) UNIQUE
);
```

This prevents duplicate non-`NULL` usernames and email addresses under the usual unique-constraint rules. Handling of `NULL` values in unique constraints varies by database system.

### DEFAULT Constraint

Provides a default value when an insert omits the column or explicitly requests the default.

```sql
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    is_available BOOLEAN DEFAULT TRUE,
    discount_percent DECIMAL(5,2) DEFAULT 0.00
);
```

If `is_available` is omitted, the default is `TRUE` where supported. If `discount_percent` is omitted, the default is `0.00`.

A default does not automatically replace an explicitly supplied `NULL`.

### CHECK Constraint

Restricts values according to a condition.

```sql
CREATE TABLE order_items (
    item_id INT PRIMARY KEY,
    quantity INT CHECK (quantity > 0),
    price DECIMAL(10,2) CHECK (price > 0)
);
```

Here:

- `quantity` must be greater than zero.
- `price` must be greater than zero.

A `CHECK` constraint does not necessarily reject `NULL`; combine it with `NOT NULL` when a value is mandatory.

### PRIMARY KEY Constraint

Uniquely identifies each row and does not allow `NULL`.

```sql
CREATE TABLE departments (
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100)
);
```

### FOREIGN KEY Constraint

Links a column or group of columns to an eligible key in another table.

```sql
CREATE TABLE employees (
    emp_id INT PRIMARY KEY,
    emp_name VARCHAR(100) NOT NULL,
    department_id INT,
    FOREIGN KEY (department_id)
        REFERENCES departments(department_id)
);
```

### Complete Example with Multiple Constraints

```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    age INT CHECK (age >= 18),
    is_active BOOLEAN DEFAULT TRUE
);
```

This table uses a primary key, required columns, a unique email, a check condition, and a default value.

---

## 8. INSERT Basics

### What is INSERT?

`INSERT` is a SQL command used to add new rows of data into a table.

### Single Insert

Insert one record at a time.

#### Syntax

```sql
INSERT INTO table_name (column1, column2, column3)
VALUES (value1, value2, value3);
```

#### Example

```sql
INSERT INTO customers
    (customer_id, customer_name, email, city)
VALUES
    (1, 'Rahul Kumar', 'rahul@email.com', 'Hyderabad');
```

This inserts one customer into the table.

### Insert Data Without Specifying Column Names

If you provide values for all columns, you can omit the column list.

```sql
INSERT INTO customers
VALUES (2, 'Neha Singh', 'neha@email.com', 'Chennai');
```

**Important considerations:**

- The values must match the table's column order.
- The number of values must match the number of columns expected by the statement.
- All required columns must receive valid values.
- This approach is more sensitive to schema changes than explicitly listing columns.

Specifying column names is generally recommended for readability and maintainability.

### Insert Selected Columns

Specify only the columns you want to insert data into.

```sql
INSERT INTO customers
    (customer_id, customer_name, email)
VALUES
    (3, 'Kiran Patel', 'kiran@email.com');
```

Here, `city` is omitted. If no default value is defined and the column allows `NULL`, its value will be `NULL`.

Omitted columns must allow `NULL`, have a default value, or be populated automatically, as applicable. Otherwise, the insert may fail.

### Multiple Inserts

Insert multiple records in a single statement.

```sql
INSERT INTO customers
    (customer_id, customer_name, email, city)
VALUES
    (4, 'Amit Sharma', 'amit@email.com', 'Delhi'),
    (5, 'Priya Verma', 'priya@email.com', 'Pune'),
    (6, 'Rajesh Gupta', 'rajesh@email.com', 'Mumbai');
```

This inserts three records in one statement. Multi-row `VALUES` syntax is supported by common SQL database systems, although details may vary.

### Multiple Inserts with a NULL Value

A single multi-row `INSERT` statement can insert records containing `NULL` values into columns that allow them.

```sql
INSERT INTO customers
    (customer_id, customer_name, email, city)
VALUES
    (7, 'Meera Khan', 'meera@email.com', 'Lucknow'),
    (8, 'Arjun Rao', 'arjun@email.com', NULL),
    (9, 'Divya Reddy', 'divya@email.com', 'Nellore');
```

Here, `NULL` indicates that the city value for customer 8 is missing, unknown, or not applicable.

`NULL` is not the same as an empty string or the text `'NULL'`.

### Insert Data for Few Columns Only

If columns are optional or have default values, you do not need to provide values for every column when inserting a new record.

```sql
INSERT INTO products (product_id, product_name)
VALUES (101, 'Laptop');
```

Here, only `product_id` and `product_name` are provided. The remaining columns will use their defaults, become `NULL` if permitted, or be populated automatically, as applicable. If a required column has no applicable default, the insert may fail.

#### Example with Defaults

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

The resulting row uses:

- `quantity` = `0`
- `is_available` = `TRUE`, where supported

---

## 9. UPDATE Basics

### What is UPDATE?

`UPDATE` is a SQL command used to modify existing data in one or more rows of a table.

### Update One Record

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

This changes the city of customer 1 to Pune.

### Update a Column for Multiple Rows

```sql
UPDATE customers
SET is_active = FALSE
WHERE city = 'Delhi';
```

This marks all customers from Delhi as inactive, assuming the table contains the relevant column.

### Update Multiple Columns

```sql
UPDATE customers
SET email = 'neha.singh@email.com',
    city = 'Bangalore'
WHERE customer_id = 2;
```

This updates both the email and city for customer 2.

### Update Different Groups with Different Conditions

```sql
UPDATE products
SET price = price * 0.90
WHERE category = 'Electronics';

UPDATE products
SET price = price * 0.95
WHERE category = 'Furniture';
```

The first statement reduces the price of electronics by 10%. The second reduces the price of furniture by 5%.

### Update with a Calculation

```sql
UPDATE products
SET stock_quantity = stock_quantity - 1
WHERE product_id = 101;
```

This reduces the stock quantity of product 101 by one.

### Update Multiple Rows

```sql
UPDATE orders
SET status = 'Shipped'
WHERE order_date < '2025-03-01';
```

This changes the status of every order before March 1, 2025, to `Shipped
