# Module 3: Joins Practice File (Join-focused only)

## Objective
Practice SQL joins from beginner to advanced join-only scenarios. This file excludes aggregation, GROUP BY, ORDER BY, and window functions — those will be covered later. The goal here is to teach join structure, null handling, anti-/semi-joins, and multi-table matching.

---

## 1. Easy Level: Basic Join Practice

### Table Setup
Use the following tables for the first set of exercises.

```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(50),
    city VARCHAR(50)
);

CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(50),
    category VARCHAR(50)
);

CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    product_id INT,
    quantity INT,
    order_date DATE
);

INSERT INTO customers (customer_id, customer_name, city) VALUES
(1, 'Rahul', 'Hyderabad'),
(2, 'Neha', 'Chennai'),
(3, 'Kiran', 'Bengaluru');

INSERT INTO products (product_id, product_name, category) VALUES
(10, 'Laptop', 'Electronics'),
(11, 'Mobile', 'Electronics'),
(12, 'Chair', 'Furniture');

INSERT INTO orders (order_id, customer_id, product_id, quantity, order_date) VALUES
(100, 1, 10, 1, '2024-01-10'),
(101, 1, 11, 2, '2024-01-20'),
(102, 2, 12, 1, '2024-02-02');
```

---

## Exercise 1: INNER JOIN
Find all orders with customer name and product name.

```sql
SELECT o.order_id, c.customer_name, p.product_name, o.quantity
FROM orders o
INNER JOIN customers c ON o.customer_id = c.customer_id
INNER JOIN products p ON o.product_id = p.product_id;
```

### Expected Output
| order_id | customer_name | product_name | quantity |
|---------:|---------------|--------------|---------:|
| 100      | Rahul         | Laptop       | 1        |
| 101      | Rahul         | Mobile       | 2        |
| 102      | Neha          | Chair        | 1        |

---

## Exercise 2: LEFT JOIN
List all customers and any orders they have placed.

```sql
SELECT c.customer_id, c.customer_name, o.order_id, o.order_date
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id;
```

### Expected Output
| customer_id | customer_name | order_id | order_date |
|------------:|---------------|---------:|------------|
| 1           | Rahul         | 100      | 2024-01-10 |
| 1           | Rahul         | 101      | 2024-01-20 |
| 2           | Neha          | 102      | 2024-02-02 |
| 3           | Kiran         | NULL     | NULL       |

---

## Exercise 3: ANTI-JOIN (customers without orders)

```sql
SELECT c.customer_id, c.customer_name
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```

### Expected Output
| customer_id | customer_name |
|------------:|---------------|
| 3           | Kiran         |

---

## Exercise 4: PRODUCTS LEFT JOIN ORDERS (find products with/without orders)

```sql
SELECT p.product_id, p.product_name, o.order_id, o.quantity
FROM products p
LEFT JOIN orders o ON p.product_id = o.product_id;
```

### Expected Output
| product_id | product_name | order_id | quantity |
|-----------:|--------------|---------:|---------:|
| 10         | Laptop       | 100      | 1        |
| 11         | Mobile       | 101      | 2        |
| 12         | Chair        | 102      | 1        |

---

## 2. Medium Level: Self-joins, USING/ON, and Semi-joins

### Table Setup
```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(50),
    department VARCHAR(50),
    manager_id INT
);

INSERT INTO employees (employee_id, employee_name, department, manager_id) VALUES
(1, 'Bob', 'Engineering', NULL),
(2, 'Alice', 'Engineering', 1),
(3, 'John', 'Sales', 1),
(4, 'Mary', 'Engineering', 2),
(5, 'Steve', 'Sales', 3);
```

---

## Exercise 5: SELF JOIN (employee → manager)

```sql
SELECT e.employee_id,
       e.employee_name AS employee,
       m.employee_name AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.employee_id;
```

### Expected Output
| employee_id | employee | manager |
|------------:|----------|---------|
| 1           | Bob      | NULL    |
| 2           | Alice    | Bob     |
| 3           | John     | Bob     |
| 4           | Mary     | Alice   |
| 5           | Steve    | John    |

---

## Exercise 6: Find managers (semi-join using EXISTS)

```sql
SELECT DISTINCT m.employee_id, m.employee_name
FROM employees m
WHERE EXISTS (SELECT 1 FROM employees e WHERE e.manager_id = m.employee_id);
```

### Expected Output
| employee_id | employee_name |
|------------:|---------------|
| 1           | Bob           |
| 2           | Alice         |
| 3           | John          |

---

## Exercise 7: USING vs ON example (explicit vs shorthand)

Assume orders.product_id and products.product_id are same-named keys.

```sql
-- USING (shorthand)
SELECT o.order_id, o.customer_id, p.product_name
FROM orders o
JOIN products p USING (product_id);

-- Equivalent explicit ON version (recommended for clarity)
SELECT o.order_id, o.customer_id, p.product_name
FROM orders o
JOIN products p ON o.product_id = p.product_id;
```

### Expected Output (both queries return same rows):
| order_id | customer_id | product_name |
|---------:|------------:|--------------|
| 100      | 1           | Laptop       |
| 101      | 1           | Mobile       |
| 102      | 2           | Chair        |

---

## 3. Complex Level: Multi-table joins and reconciliation (join-only)

### Table Setup (orders/payments/products/customers)
```sql
CREATE TABLE payments (
    payment_id INT PRIMARY KEY,
    order_id INT,
    amount DECIMAL(10,2),
    payment_date DATE
);

INSERT INTO payments (payment_id, order_id, amount, payment_date) VALUES
(1000, 100, 45000.00, '2024-01-07'),
(1001, 101, 50000.00, '2024-01-08'),
(1002, 200, 3000.00, '2024-01-09'); -- payment with no matching order

-- reuse products/customers/orders from earlier sections
```

---

## Exercise 8: Multi-join order report (orders → customers → products → payments)

```sql
SELECT o.order_id,
       c.customer_name,
       p.product_name,
       o.quantity,
       pay.payment_id,
       pay.amount
FROM orders o
LEFT JOIN customers c ON o.customer_id = c.customer_id
LEFT JOIN products p ON o.product_id = p.product_id
LEFT JOIN payments pay ON o.order_id = pay.order_id;
```

### Expected Output
| order_id | customer_name | product_name | quantity | payment_id | amount    |
|--------:|---------------|--------------|---------:|-----------:|----------:|
| 100     | Rahul         | Laptop       | 1        | 1000       | 45000.00 |
| 101     | Rahul         | Mobile       | 2        | 1001       | 50000.00 |
| 102     | Neha          | Chair        | 1        | NULL       | NULL     |

### Explanation
- LEFT JOINs keep all orders even if payments are missing.
- Payment 1002 (for order 200) is not shown — reconciliation below will reveal it.

---

## Exercise 9: Find orders without payments (anti-join)

```sql
SELECT o.order_id, o.customer_id
FROM orders o
LEFT JOIN payments p ON o.order_id = p.order_id
WHERE p.payment_id IS NULL;
```

### Expected Output
| order_id | customer_id |
|--------:|------------:|
| 102     | 2           |

---

## Exercise 10: Find payments without matching orders (anti-join other side)

```sql
SELECT p.payment_id, p.order_id, p.amount
FROM payments p
LEFT JOIN orders o ON p.order_id = o.order_id
WHERE o.order_id IS NULL;
```

### Expected Output
| payment_id | order_id | amount   |
|-----------:|---------:|---------:|
| 1002       | 200      | 3000.00  |

---

## Exercise 11: Full reconciliation (emulate FULL OUTER JOIN across orders and payments)

Portable approach using LEFT and RIGHT joins with UNION ALL:

```sql
-- left side: all orders with payments where available
SELECT o.order_id, o.customer_id, p.payment_id, p.amount
FROM orders o
LEFT JOIN payments p ON o.order_id = p.order_id

UNION ALL

-- right side: payments that had no matching orders (exclude rows already returned by left join)
SELECT o.order_id, o.customer_id, p.payment_id, p.amount
FROM orders o
RIGHT JOIN payments p ON o.order_id = p.order_id
WHERE o.order_id IS NULL;
```

### Expected Output (conceptual)
| order_id | customer_id | payment_id | amount   |
|--------:|------------:|-----------:|---------:|
| 100     | 1           | 1000       | 45000.00|
| 101     | 1           | 1001       | 50000.00|
| 102     | 2           | NULL       | NULL    |
| NULL    | NULL        | 1002       | 3000.00 |

---

## Exercise 12: CROSS JOIN small demo (combine small sets)

```sql
-- small cross join of two short lists
SELECT c.customer_name, p.product_name
FROM (SELECT * FROM customers WHERE customer_id <= 2) c
CROSS JOIN (SELECT * FROM products WHERE product_id <= 11) p;
```

### Expected Output
| customer_name | product_name |
|--------------:|--------------|
| Rahul         | Laptop       |
| Rahul         | Mobile       |
| Neha          | Laptop       |
| Neha          | Mobile       |

---

## Exercise 13: USING vs ON practical check

```sql
-- USING will not repeat the shared column in output in some DBs
SELECT o.order_id, o.customer_id, p.product_name
FROM orders o
JOIN products p USING (product_id);

-- ON version returns same data but is explicit
SELECT o.order_id, o.customer_id, p.product_name
FROM orders o
JOIN products p ON o.product_id = p.product_id;
```

### Expected Output (both):
| order_id | customer_id | product_name |
|---------:|------------:|--------------|
| 100      | 1           | Laptop       |
| 101      | 1           | Mobile       |
| 102      | 2           | Chair        |

---

## Final Notes
- This file focuses strictly on joins, anti-/semi-joins, and reconciliation patterns.
- Aggregation, grouping, ordering, and window functions will be covered in a separate file to avoid mixing concepts.

If you want, I will now update the repository with this join-only practice file (replacing the previous practice file). Confirm and I will commit the change.