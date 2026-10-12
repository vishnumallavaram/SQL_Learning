# Module 2 Part 1: Practical Work - SELECT, DISTINCT, WHERE

## Objective
Practice selecting columns, removing duplicates, and filtering data using WHERE conditions.

---

## Sample Tables

### Customers
```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100),
    email VARCHAR(100),
    city VARCHAR(50),
    status VARCHAR(20),
    phone VARCHAR(20)
);
```

### Products
```sql
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10,2),
    stock INT,
    is_active VARCHAR(10)
);
```

### Sample data
```sql
INSERT INTO customers (customer_id, customer_name, email, city, status, phone) VALUES
(1, 'Anil', 'anil@gmail.com', 'Hyderabad', 'Active', '9876543210'),
(2, 'Bhavna', 'bhavna@gmail.com', 'Pune', 'Inactive', NULL),
(3, 'Chetan', 'chetan@gmail.com', 'Hyderabad', 'Active', '9988776655'),
(4, 'Divya', 'divya@gmail.com', 'Bengaluru', 'Active', '9123456789'),
(5, 'Esha', 'esha@gmail.com', 'Delhi', 'Inactive', NULL),
(6, 'Farhan', 'farhan@gmail.com', 'Hyderabad', 'Active', '9090909090');

INSERT INTO products (product_id, product_name, category, price, stock, is_active) VALUES
(1, 'Laptop', 'Electronics', 45000.00, 15, 'Yes'),
(2, 'Phone', 'Electronics', 25000.00, 30, 'Yes'),
(3, 'Chair', 'Furniture', 2500.00, 20, 'Yes'),
(4, 'Table', 'Furniture', 5000.00, 12, 'No'),
(5, 'Monitor', 'Electronics', 12000.00, 18, 'Yes'),
(6, 'Keyboard', 'Electronics', 1500.00, 35, 'Yes');
```

---

## Exercise 1: Select all rows
```sql
SELECT *
FROM customers;
```

---

## Exercise 2: Select specific columns
```sql
SELECT customer_name, email, city
FROM customers;
```

---

## Exercise 3: Use aliases
```sql
SELECT customer_name AS name, email AS email_id
FROM customers;
```

```sql
SELECT product_name AS product, price * 2 AS doubled_price
FROM products;
```

---

## Exercise 4: DISTINCT for unique city names
```sql
SELECT DISTINCT city
FROM customers;
```

---

## Exercise 5: DISTINCT on multiple columns
```sql
SELECT DISTINCT city, status
FROM customers;
```

---

## Exercise 6: Filter with equality
```sql
SELECT *
FROM customers
WHERE city = 'Hyderabad';
```

---

## Exercise 7: Filter with greater than
```sql
SELECT *
FROM products
WHERE price > 10000;
```

---

## Exercise 8: Filter with AND
```sql
SELECT *
FROM customers
WHERE city = 'Hyderabad' AND status = 'Active';
```

---

## Exercise 9: Filter with OR
```sql
SELECT *
FROM customers
WHERE city = 'Hyderabad' OR city = 'Pune';
```

---

## Exercise 10: Filter with NOT
```sql
SELECT *
FROM customers
WHERE NOT status = 'Inactive';
```

---

## Exercise 11: Filter with BETWEEN
```sql
SELECT *
FROM products
WHERE price BETWEEN 1000 AND 20000;
```

---

## Exercise 12: Filter with IN
```sql
SELECT *
FROM customers
WHERE city IN ('Hyderabad', 'Bengaluru', 'Pune');
```

---

## Exercise 13: Filter with LIKE
```sql
SELECT *
FROM customers
WHERE customer_name LIKE 'A%';
```

```sql
SELECT *
FROM customers
WHERE customer_name LIKE '%a%';
```

---

## Exercise 14: Filter with IS NULL
```sql
SELECT *
FROM customers
WHERE phone IS NULL;
```

---

## Exercise 15: Filter with IS NOT NULL
```sql
SELECT *
FROM customers
WHERE phone IS NOT NULL;
```

---

## Exercise 16: Complex query with multiple filters
```sql
SELECT customer_name, city, status
FROM customers
WHERE city IN ('Hyderabad', 'Bengaluru')
AND status = 'Active'
AND phone IS NOT NULL;
```

---

## Exercise 17: Use parentheses
```sql
SELECT *
FROM products
WHERE (category = 'Electronics' OR category = 'Furniture')
AND price > 1000;
```

---

## Exercise 18: Find all inactive customers
```sql
SELECT *
FROM customers
WHERE status = 'Inactive';
```

---

## Exercise 19: Find all products under 5000
```sql
SELECT *
FROM products
WHERE price < 5000;
```

---

## Exercise 20: Find customers whose name starts with D
```sql
SELECT *
FROM customers
WHERE customer_name LIKE 'D%';
```

---

## Reflection Questions
1. Why is WHERE important in SQL?
2. What happens if you forget WHERE in UPDATE or DELETE?
3. Why is `IS NULL` different from `= NULL`?
4. How is `IN` better than writing many OR conditions?
5. When would you use `DISTINCT`?
6. Why do we use aliases?


