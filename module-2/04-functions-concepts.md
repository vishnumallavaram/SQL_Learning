# Module 2 Part 1: SELECT, DISTINCT, and WHERE Filters

## Objective
Learn how to retrieve exact data using SELECT, remove duplicates with DISTINCT, and filter rows with WHERE conditions.

---

## Practice Data for This Module

Use the following sample tables while learning SELECT and WHERE filters.

```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100),
    city VARCHAR(50),
    status VARCHAR(20),
    email VARCHAR(100),
    phone VARCHAR(20)
);

CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10,2),
    stock INT,
    is_active VARCHAR(10)
);

INSERT INTO customers (customer_id, customer_name, city, status, email, phone) VALUES
(1, 'Anil', 'Hyderabad', 'Active', 'anil@gmail.com', '9876543210'),
(2, 'Bhavna', 'Pune', 'Inactive', 'bhavna@gmail.com', NULL),
(3, 'Chetan', 'Hyderabad', 'Active', 'chetan@gmail.com', '9988776655'),
(4, 'Divya', 'Bengaluru', 'Active', 'divya@gmail.com', '9123456789'),
(5, 'Esha', 'Delhi', 'Inactive', 'esha@gmail.com', NULL),
(6, 'Farhan', 'Hyderabad', 'Active', 'farhan@gmail.com', '9090909090');

INSERT INTO products (product_id, product_name, category, price, stock, is_active) VALUES
(1, 'Laptop', 'Electronics', 45000.00, 15, 'Yes'),
(2, 'Phone', 'Electronics', 25000.00, 30, 'Yes'),
(3, 'Chair', 'Furniture', 2500.00, 20, 'Yes'),
(4, 'Table', 'Furniture', 5000.00, 12, 'No'),
(5, 'Monitor', 'Electronics', 12000.00, 18, 'Yes'),
(6, 'Keyboard', 'Electronics', 1500.00, 35, 'Yes');
```

This dataset supports all examples in this file.

---

## 1. SELECT Statement
SELECT is used to read data from one or more columns in a table.

### When to use
- You want to view only the columns you need.
- You want to see all column values from a table.
- You want to calculate values from columns.

### Syntax
```sql
SELECT column1, column2
FROM table_name;
```

### Example
```sql
SELECT customer_name, email
FROM customers;
```

### Select all columns
```sql
SELECT *
FROM customers;
```

---

## 2. Aliases
Aliases give a temporary name to a column or table to make the result easier to read.

### When to use
- Results need a cleaner display name.
- A calculated result needs a business-friendly label.
- You want shorter names in queries.

### Example
```sql
SELECT customer_name AS name, email AS email_id
FROM customers;
```

```sql
SELECT price * quantity AS total_value
FROM products;
```

---

## 3. DISTINCT
DISTINCT removes duplicate rows from the result set.

### When to use
- You want to list unique values only.
- You want to avoid repeated records in reports.
- You want to count unique values.

### Example
```sql
SELECT DISTINCT city
FROM customers;
```

### Count unique values
```sql
SELECT COUNT(DISTINCT city)
FROM customers;
```

---

## 4. WHERE Clause
WHERE filters the rows before they are displayed.

### When to use
- You want only active customers.
- You want records for a particular city.
- You want to remove rows that do not match business rules.

### Example
```sql
SELECT *
FROM customers
WHERE city = 'Hyderabad';
```

---

## 5. Comparison Operators
These compare values and help filter records.

### Operators
- `=` equal to
- `!=` or `<>` not equal to
- `>` greater than
- `<` less than
- `>=` greater than or equal to
- `<=` less than or equal to

### When to use
- Compare a value to a literal number or text.
- Filter products with price more than 5000.
- Find customers who are not from Delhi.

### Examples
```sql
SELECT *
FROM products
WHERE price > 5000;
```

```sql
SELECT *
FROM customers
WHERE city != 'Delhi';
```

---

## 6. Logical Operators
Logical operators combine multiple conditions.

### AND
Use when all conditions must be true.

```sql
SELECT *
FROM customers
WHERE city = 'Hyderabad' AND status = 'Active';
```

### OR
Use when any one condition can be true.

```sql
SELECT *
FROM customers
WHERE city = 'Hyderabad' OR city = 'Bengaluru';
```

### NOT
Use to reverse a condition.

```sql
SELECT *
FROM customers
WHERE NOT status = 'Inactive';
```

### When to use
- Use AND when you need stricter filtering.
- Use OR when you want records matching one of several values.
- Use NOT when excluding rows from a result.

### Parentheses example
```sql
SELECT *
FROM products
WHERE (category = 'Electronics' OR category = 'Furniture')
AND price > 1000;
```

---

## 7. BETWEEN and NOT BETWEEN
BETWEEN checks if a value is within a range.

### When to use
- Find amounts between two values.
- Check date ranges.
- Filter numeric values inside a range.

### Example
```sql
SELECT *
FROM products
WHERE price BETWEEN 500 AND 2000;
```

### Not between
```sql
SELECT *
FROM products
WHERE price NOT BETWEEN 500 AND 2000;
```

---

## 8. IN and NOT IN
IN checks whether a value matches any value in a list.

### When to use
- Filter by several known values.
- Replace long OR conditions.
- Compare city, status, category, department, etc.

### Example
```sql
SELECT *
FROM customers
WHERE city IN ('Hyderabad', 'Pune', 'Bengaluru');
```

```sql
SELECT *
FROM customers
WHERE city NOT IN ('Delhi', 'Mumbai');
```

---

## 9. LIKE and Pattern Matching
LIKE is used for text matching with wildcards.

### Wildcards
- `%` = any number of characters
- `_` = one single character

### When to use
- Find names beginning with a letter.
- Search text like email, product codes, names.
- Match partial text values.

### Examples
```sql
SELECT *
FROM customers
WHERE customer_name LIKE 'A%';
```

This returns names starting with A.

```sql
SELECT *
FROM customers
WHERE customer_name LIKE '%sh%';
```

This returns names containing "sh".

```sql
SELECT *
FROM customers
WHERE customer_name LIKE 'A_';
```

This returns names of exactly 2 characters starting with A.

### NOT LIKE
```sql
SELECT *
FROM customers
WHERE customer_name NOT LIKE 'A%';
```

---

## 10. NULL Filtering
NULL means the value is missing or unknown.

### Important
Do not use `= NULL`.
Use `IS NULL` or `IS NOT NULL`.

### When to use
- Check if phone number is missing.
- Find rows where a field is not entered.
- Handle incomplete records.

### Examples
```sql
SELECT *
FROM customers
WHERE phone IS NULL;
```

```sql
SELECT *
FROM customers
WHERE phone IS NOT NULL;
```

---

## 11. Main WHERE Filter Types Summary
The most common filters used in WHERE are:

- Comparison filters: `=`, `>`, `<`, `>=`, `<=`, `!=`
- Logical filters: `AND`, `OR`, `NOT`
- Range filters: `BETWEEN`, `NOT BETWEEN`
- List filters: `IN`, `NOT IN`
- Pattern filters: `LIKE`, `NOT LIKE`
- Null checks: `IS NULL`, `IS NOT NULL`

---

## 12. When to Use Which Filter?

### Use `=` when
- You know the exact value.
```sql
WHERE city = 'Hyderabad';
```

### Use `BETWEEN` when
- You need a range of values.
```sql
WHERE price BETWEEN 1000 AND 5000;
```

### Use `IN` when
- You need a list of allowed values.
```sql
WHERE city IN ('Hyderabad', 'Pune', 'Bengaluru');
```

### Use `LIKE` when
- You need partial matching or pattern matching.
```sql
WHERE customer_name LIKE 'A%';
```

### Use `IS NULL` when
- You want to find missing values.
```sql
WHERE phone IS NULL;
```

### Use `AND` when
- Multiple conditions must all be true.
```sql
WHERE city = 'Hyderabad' AND status = 'Active';
```

### Use `OR` when
- Any one of multiple conditions is acceptable.
```sql
WHERE city = 'Hyderabad' OR city = 'Pune';
```

---

## 13. Best Practices
- Use `WHERE` to reduce the number of rows returned.
- Use parentheses in complex conditions for clarity.
- Never use `= NULL`; always use `IS NULL`.
- Use `IN` instead of many OR conditions when possible.
- Use `LIKE` only when pattern matching is required.

---

## 14. Example Combined Query
```sql
SELECT customer_name, city, status
FROM customers
WHERE city IN ('Hyderabad', 'Bengaluru')
AND status = 'Active'
AND phone IS NOT NULL;
```

This query finds active customers from Hyderabad or Bengaluru who have a phone number.

---

## Summary
SELECT reads data, DISTINCT removes duplicates, and WHERE filters the rows you need. Understanding the different filter types is essential for writing accurate SQL queries.

---

## Interview Questions
1. What is the purpose of WHERE?
2. Why is DISTINCT important?
3. Why is `IS NULL` used instead of `= NULL`?
4. When would you use `IN` instead of OR?
5. What is the difference between `BETWEEN` and `IN`?
6. How is `LIKE` different from `=`?
7. When is `AND` preferred over `OR`?
8. Why do we use parentheses in complex conditions?


