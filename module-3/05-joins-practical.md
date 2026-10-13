# Joins: Practical Exercises — Easy → Medium → Complex

## Objective
Three practical scenarios with table definitions and sample data. Each scenario includes practice questions arranged by difficulty (Easy → Medium → Complex) and answers with SQL and expected results. The goal is to teach join structure and thinking — start simple, then combine features and edge cases.

---

## Scenario 1 — E-Commerce Orders (Easy → Medium)

Description
A small online store tracks customers, products, and orders. Practice basic INNER and LEFT joins and a simple aggregation.

Schema and sample data

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

Practice questions (easy → medium)

Easy 1 — List all orders with customer name and product name
Answer (SQL):
```sql
SELECT o.order_id, c.customer_name, p.product_name, o.quantity
FROM orders o
JOIN customers c ON o.customer_id = c.customer_id
JOIN products p ON o.product_id = p.product_id
ORDER BY o.order_id;
```
Expected result:
| order_id | customer_name | product_name | quantity |
|--------:|---------------|--------------|---------:|
| 100     | Rahul         | Laptop       | 1        |
| 101     | Rahul         | Mobile       | 2        |
| 102     | Neha          | Chair        | 1        |

Easy 2 — Show customers who have not placed any order (anti-join)
Answer (SQL):
```sql
SELECT c.customer_id, c.customer_name
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```
Expected result:
| customer_id | customer_name |
|------------:|---------------|
| 3           | Kiran         |

Medium 1 — Count orders per customer (include customers with zero orders)
Answer (SQL):
```sql
SELECT c.customer_id, c.customer_name, COALESCE(cnt.num_orders, 0) AS orders_count
FROM customers c
LEFT JOIN (
  SELECT customer_id, COUNT(*) AS num_orders
  FROM orders
  GROUP BY customer_id
) cnt ON c.customer_id = cnt.customer_id
ORDER BY c.customer_id;
```
Expected result:
| customer_id | customer_name | orders_count |
|------------:|---------------|-------------:|
| 1           | Rahul         | 2            |
| 2           | Neha          | 1            |
| 3           | Kiran         | 0            |

Notes: This combines LEFT JOIN with an aggregate subquery and COALESCE to replace NULLs.

---

## Scenario 2 — Employees and Managers (Medium)

Description
An employee table contains hierarchical manager relationships (self-referential). Practice self-join, LEFT JOIN to include missing managers, and a semi-join to find managers who have reports.

Schema and sample data

```sql
CREATE TABLE employees (
  employee_id INT PRIMARY KEY,
  name VARCHAR(50),
  department VARCHAR(50),
  manager_id INT -- references employees.employee_id (nullable)
);

INSERT INTO employees (employee_id, name, department, manager_id) VALUES
(1, 'Bob', 'Engineering', NULL),
(2, 'Alice', 'Engineering', 1),
(3, 'John', 'Sales', 1),
(4, 'Mary', 'Engineering', 2),
(5, 'Steve', 'Sales', 3),
(6, 'Tom', 'HR', NULL);
```

Practice questions (medium)

Medium 1 — List each employee with their manager's name (show NULL if no manager)
Answer (SQL):
```sql
SELECT e.employee_id, e.name AS employee_name, m.employee_id AS manager_id, m.name AS manager_name
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.employee_id
ORDER BY e.employee_id;
```
Expected result:
| employee_id | employee_name | manager_id | manager_name |
|------------:|---------------|-----------:|--------------|
| 1           | Bob           | NULL       | NULL         |
| 2           | Alice         | 1          | Bob          |
| 3           | John          | 1          | Bob          |
| 4           | Mary          | 2          | Alice        |
| 5           | Steve         | 3          | John         |
| 6           | Tom           | NULL       | NULL         |

Medium 2 — Find employees who are managers (have at least one direct report) (semi-join)
Answer (SQL):
```sql
SELECT DISTINCT m.employee_id, m.name
FROM employees m
WHERE EXISTS (SELECT 1 FROM employees e WHERE e.manager_id = m.employee_id)
ORDER BY m.employee_id;
```
Expected result:
| employee_id | name |
|------------:|------|
| 1           | Bob  |
| 2           | Alice|
| 3           | John |

Medium 3 — Show department head count (how many employees per department)
Answer (SQL):
```sql
SELECT department, COUNT(*) AS head_count
FROM employees
GROUP BY department
ORDER BY department;
```
Expected result:
| department  | head_count |
|------------:|-----------:|
| Engineering | 3          |
| HR          | 1          |
| Sales       | 2          |

Notes: These queries teach self-join, EXISTS-based semi-join, and grouping.

---

## Scenario 3 — Sales, Products, Regions, Promotions (Complex)

Description
A sales system stores sales transactions, product catalog with category, and region information. Practice multi-way joins, left/right/full behavior, anti-join across many-to-many, and combining results with COALESCE and window functions for ranking.

Schema and sample data

```sql
CREATE TABLE regions (
  region_id INT PRIMARY KEY,
  region_name VARCHAR(50)
);

CREATE TABLE products (
  product_id INT PRIMARY KEY,
  product_name VARCHAR(50),
  category VARCHAR(50)
);

CREATE TABLE sales (
  sale_id INT PRIMARY KEY,
  product_id INT,
  region_id INT,
  sale_date DATE,
  amount DECIMAL(10,2)
);

CREATE TABLE promotions (
  promo_id INT PRIMARY KEY,
  product_id INT,
  promo_name VARCHAR(50)
);

INSERT INTO regions (region_id, region_name) VALUES
(1, 'North'),
(2, 'South'),
(3, 'West');

INSERT INTO products (product_id, product_name, category) VALUES
(10, 'Laptop', 'Electronics'),
(11, 'Mobile', 'Electronics'),
(12, 'Chair', 'Furniture'),
(13, 'Desk', 'Furniture');

INSERT INTO sales (sale_id, product_id, region_id, sale_date, amount) VALUES
(1000, 10, 1, '2024-01-05', 45000.00),
(1001, 11, 1, '2024-01-08', 25000.00),
(1002, 12, 2, '2024-01-09', 5000.00),
(1003, 10, 3, '2024-02-01', 44000.00),
(1004, 11, 3, '2024-02-10', 26000.00);

INSERT INTO promotions (promo_id, product_id, promo_name) VALUES
(900, 10, 'New Year Sale'),
(901, 13, 'Clearance'); -- promo on a product that has no sales in sample data
```

Practice questions (complex)

Complex 1 — Multi-join: For each sale show sale_id, product_name, category, region_name, amount
Answer (SQL):
```sql
SELECT s.sale_id, p.product_name, p.category, r.region_name, s.amount
FROM sales s
LEFT JOIN products p ON s.product_id = p.product_id
LEFT JOIN regions r ON s.region_id = r.region_id
ORDER BY s.sale_id;
```
Expected result:
| sale_id | product_name | category    | region_name | amount    |
|-------:|--------------|-------------|-------------|----------:|
| 1000   | Laptop       | Electronics | North       | 45000.00  |
| 1001   | Mobile       | Electronics | North       | 25000.00  |
| 1002   | Chair        | Furniture   | South       | 5000.00   |
| 1003   | Laptop       | Electronics | West        | 44000.00  |
| 1004   | Mobile       | Electronics | West        | 26000.00  |

Complex 2 — Promotions anti-join: List products that have promotions but no sales (hint: left join promos -> sales)
Answer (SQL):
```sql
SELECT pr.product_id, pr.product_name
FROM promotions promo
JOIN products pr ON promo.product_id = pr.product_id
LEFT JOIN sales s ON pr.product_id = s.product_id
WHERE s.sale_id IS NULL;
```
Expected result:
| product_id | product_name |
|-----------:|--------------|
| 13         | Desk         |

Explanation: promo exists for product 13 (Desk) but there are no sales for product_id 13 in sales table.

Complex 3 — Reconciliation (full outer style): show sales and promotions side-by-side per product to identify products with sales but no promo, promo but no sales, or both. Use UNION emulation for FULL OUTER join (portable SQL):
Answer (SQL):
```sql
-- left side: products with sales (and promos when available)
SELECT p.product_id, p.product_name, 'sales' AS source, s.sale_id AS ref_id, promo.promo_id
FROM products p
LEFT JOIN sales s ON p.product_id = s.product_id
LEFT JOIN promotions promo ON p.product_id = promo.product_id

UNION ALL

-- right side: promotions for products that have no sales (avoid duplicating rows already included)
SELECT p.product_id, p.product_name, 'promo_only' AS source, NULL AS ref_id, promo.promo_id
FROM products p
JOIN promotions promo ON p.product_id = promo.product_id
WHERE NOT EXISTS (SELECT 1 FROM sales s WHERE s.product_id = p.product_id)
ORDER BY product_id, source;
```
Expected conceptual result (rows combined; order may vary):
| product_id | product_name | source     | ref_id | promo_id |
|-----------:|--------------|------------|-------:|---------:|
| 10         | Laptop       | sales      | 1000   | 900      |
| 10         | Laptop       | sales      | 1003   | 900      |
| 11         | Mobile       | sales      | 1001   | NULL     |
| 11         | Mobile       | sales      | 1004   | NULL     |
| 12         | Chair        | sales      | 1002   | NULL     |
| 13         | Desk         | promo_only | NULL   | 901      |

Notes: This pattern helps reconcile events and flags items on either side only.

Complex 4 — Sales leader per region (window function) — show top-selling product by total amount in each region
Answer (SQL):
```sql
-- total sales per product per region
WITH region_product_sales AS (
  SELECT region_id, product_id, SUM(amount) AS total_amount
  FROM sales
  GROUP BY region_id, product_id
)
SELECT r.region_name, p.product_name, rps.total_amount
FROM (
  SELECT region_id, product_id, total_amount,
         ROW_NUMBER() OVER (PARTITION BY region_id ORDER BY total_amount DESC) AS rn
  FROM region_product_sales
) rps
JOIN regions r ON rps.region_id = r.region_id
JOIN products p ON rps.product_id = p.product_id
WHERE rps.rn = 1
ORDER BY r.region_id;
```
Expected result:
| region_name | product_name | total_amount |
|------------:|-------------:|-------------:|
| North       | Laptop       | 45000.00     |
| South       | Chair        | 5000.00      |
| West        | Laptop       | 44000.00     |

Notes: This uses aggregation + window functions to rank and select the top product per region. If your DB doesn't support window functions, achieve similar results with correlated subqueries.

---

## Teaching tips and progression
- Start with Scenario 1 to build confidence: focus on JOIN ... ON and the difference between INNER and LEFT.
- Use Scenario 2 to teach self-joins and EXISTS (semi-joins).
- Finish with Scenario 3 for composition: multi-joins, anti-joins, full-outer emulation, and window functions.
- Encourage students to run queries and compare the results with expected tables provided.

---

If you want, I can:
- add these exercises as a new file in the repository (module-3/05-joins-practical.md),
- include runnable SQL scripts for automated tests, or
- generate step-by-step solutions (explain why each JOIN type was used).

Tell me which of these you'd like me to do next.