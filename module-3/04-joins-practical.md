# Module 3: Joins Practice File

## Objective
Practice SQL joins from beginner to intermediate level using simple business tables. The goal is to understand how records match across tables and how to write readable queries step by step.

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

### Explanation
- Only rows with matching customer_id and product_id are shown.
- This is the most common join type.

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

### Explanation
- All customers are shown.
- Customers with no order still appear with NULL values on the order side.

---

## Exercise 3: Find customers without orders
Use LEFT JOIN with a filter to find customers who never placed an order.

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

### Explanation
- This is called an anti-join.
- It shows rows in the left table that do not have a match in the right table.

---

## Exercise 4: Show all products and matching order information
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

### Explanation
- All products are listed.
- Products without an order would also appear with NULL values.

---

## 2. Medium Level: Multiple Tables + Aggregation

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

## Exercise 5: SELF JOIN
Display each employee with their manager name.

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

### Explanation
- The same table is joined to itself.
- This is used for hierarchy queries like manager-subordinate relationships.

---

## Exercise 6: Show managers who have direct reports
```sql
SELECT DISTINCT m.employee_id, m.employee_name
FROM employees m
INNER JOIN employees e ON m.employee_id = e.manager_id;
```

### Expected Output
| employee_id | employee_name |
|------------:|---------------|
| 1           | Bob           |
| 2           | Alice         |
| 3           | John          |

### Explanation
- Only employees who manage others are returned.
- DISTINCT avoids duplicates when an employee has multiple reports.

---

## Exercise 7: Count employees in each department
```sql
SELECT department, COUNT(*) AS total_employees
FROM employees
GROUP BY department;
```

### Expected Output
| department | total_employees |
|------------|----------------:|
| Engineering| 3               |
| Sales      | 2               |

### Explanation
- This uses GROUP BY with COUNT.
- It summarizes the number of employees per department.

---

## Exercise 8: Department-wise employee report with manager details
```sql
SELECT e.department,
       e.employee_name,
       m.employee_name AS manager_name
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.employee_id
ORDER BY e.department, e.employee_id;
```

### Expected Output
| department | employee_name | manager_name |
|------------|--------------|--------------|
| Engineering| Bob          | NULL         |
| Engineering| Alice        | Bob          |
| Engineering| Mary         | Alice        |
| Sales      | John         | Bob          |
| Sales      | Steve        | John         |

### Explanation
- This query combines employee hierarchy and department grouping.
- It is useful for reporting teams to their manager.

---

## 3. Complex Level: Real-World Multi-Table Joins

### Table Setup
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
(1003, 10, 3, '2024-02-01', 44000.00);

INSERT INTO promotions (promo_id, product_id, promo_name) VALUES
(900, 10, 'New Year Sale'),
(901, 13, 'Clearance Sale');
```

---

## Exercise 9: Join sales with region and product
```sql
SELECT s.sale_id, p.product_name, r.region_name, s.amount
FROM sales s
JOIN products p ON s.product_id = p.product_id
JOIN regions r ON s.region_id = r.region_id;
```

### Expected Output
| sale_id | product_name | region_name | amount    |
|--------:|--------------|-------------|----------:|
| 1000    | Laptop       | North       | 45000.00 |
| 1001    | Mobile       | North       | 25000.00 |
| 1002    | Chair        | South       | 5000.00  |
| 1003    | Laptop       | West        | 44000.00 |

### Explanation
- This is a real business report combining product, region, and sales information.

---

## Exercise 10: Find products that have promotions but no sales
```sql
SELECT p.product_id, p.product_name
FROM products p
JOIN promotions pr ON p.product_id = pr.product_id
LEFT JOIN sales s ON p.product_id = s.product_id
WHERE s.sale_id IS NULL;
```

### Expected Output
| product_id | product_name |
|-----------:|--------------|
| 13         | Desk         |

### Explanation
- Product 13 has a promotion but no sales in the sales table.
- This is a useful reconciliation check.

---

## Exercise 11: Total sales by region and product
```sql
SELECT r.region_name, p.product_name, SUM(s.amount) AS total_sales
FROM sales s
JOIN products p ON s.product_id = p.product_id
JOIN regions r ON s.region_id = r.region_id
GROUP BY r.region_name, p.product_name
ORDER BY r.region_name, total_sales DESC;
```

### Expected Output
| region_name | product_name | total_sales |
|-------------|--------------|-----------:|
| North       | Laptop       | 45000.00   |
| North       | Mobile       | 25000.00   |
| South       | Chair        | 5000.00    |
| West        | Laptop       | 44000.00   |

### Explanation
- Aggregation is combined with joins to produce a sales summary.

---

## Exercise 12: Show products with no sales and no promotion
```sql
SELECT p.product_id, p.product_name
FROM products p
LEFT JOIN sales s ON p.product_id = s.product_id
LEFT JOIN promotions pr ON p.product_id = pr.product_id
WHERE s.sale_id IS NULL AND pr.promo_id IS NULL;
```

### Expected Output
| product_id | product_name |
|-----------:|--------------|
| 12         | Chair        |

### Explanation
- Product 12 has sales, so this query would not return it if sales exist.
- In the sample data, Chair is in sales so actual result may be none.
- Use this as a check for products that are absent from both sales and promotions.

---

## Exercise 13: Show all products, their promotion, and sales total
```sql
SELECT p.product_id,
       p.product_name,
       COALESCE(SUM(s.amount), 0) AS total_sales,
       pr.promo_name
FROM products p
LEFT JOIN sales s ON p.product_id = s.product_id
LEFT JOIN promotions pr ON p.product_id = pr.product_id
GROUP BY p.product_id, p.product_name, pr.promo_name
ORDER BY p.product_id;
```

### Expected Output
| product_id | product_name | total_sales | promo_name     |
|-----------:|--------------|-----------:|----------------|
| 10         | Laptop       | 89000.00   | New Year Sale  |
| 11         | Mobile       | 51000.00   | NULL           |
| 12         | Chair        | 5000.00    | NULL           |
| 13         | Desk         | 0.00       | Clearance Sale |

### Explanation
- `COALESCE` replaces missing sales totals with 0.
- This is commonly used in reporting.

---

## Practice Tips
- Start with simple join logic before trying multiple tables.
- Always check the join key carefully.
- Use LEFT JOIN when you want to keep all rows from one table.
- Use anti-join patterns when you need unmatched rows.
- Use COALESCE for clean reports.

---

## Final Challenge
Write a query to answer this business question:

"Which region has the highest total sales, and which product contributed most in that region?"

Try writing it using joins and grouping first, then compare with a model answer.

### Model Answer
```sql
SELECT r.region_name,
       p.product_name,
       SUM(s.amount) AS total_sales
FROM sales s
JOIN products p ON s.product_id = p.product_id
JOIN regions r ON s.region_id = r.region_id
GROUP BY r.region_name, p.product_name
ORDER BY total_sales DESC;
```

### Expected Output
| region_name | product_name | total_sales |
|-------------|--------------|-----------:|
| North       | Laptop       | 45000.00   |
| North       | Mobile       | 25000.00   |
| West        | Laptop       | 44000.00   |
| South       | Chair        | 5000.00    |

---

## Summary
These exercises progressively teach join logic:
- Easy: basic match and unmatched rows
- Medium: self-join and aggregation
- Complex: multi-table reporting and business analysis

This structure makes joins easier to understand and easier to apply in real SQL work.
