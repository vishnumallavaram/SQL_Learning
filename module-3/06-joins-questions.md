# Joins Practice Questions (Join-only, No Answers)

## Objective
Provide 30 join-focused practice questions (no answers) using the same sample tables: Employees, Departments, Projects, Customers, and Orders. These questions are arranged from easy to complex and are to be solved using JOIN / LEFT JOIN / ON / WHERE / aliases / subqueries only. No GROUP BY, aggregates, or window functions.

---

## Sample Table Schemas and Sample Data (Run before attempting questions)

-- Departments
```sql
CREATE TABLE departments (
  department_id INT PRIMARY KEY,
  department_name VARCHAR(100),
  location VARCHAR(100)
);

INSERT INTO departments (department_id, department_name, location) VALUES
(10, 'HR', 'Head Office'),
(20, 'IT', 'Tech Park'),
(30, 'Sales', 'Regional Office'),
(40, 'Finance', 'Head Office');
```

-- Employees
```sql
CREATE TABLE employees (
  employee_id INT PRIMARY KEY,
  employee_name VARCHAR(100),
  department_id INT,      -- may be NULL or refer to departments.department_id
  manager_id INT,         -- references employees.employee_id (nullable)
  salary DECIMAL(10,2)
);

INSERT INTO employees (employee_id, employee_name, department_id, manager_id, salary) VALUES
(100, 'Asha', 20,  NULL, 70000.00),
(101, 'Vikram', 20, 100, 65000.00),
(102, 'Neha', 30, 100, 55000.00),
(103, 'Ravi', NULL, NULL, 50000.00),
(104, 'Sana', 20, 101, 60000.00),
(105, 'Imran', 40, NULL, 58000.00),
(106, 'Sara', 50, 100, 62000.00); -- note: department_id 50 does not exist (data quality)
```

-- Projects
```sql
CREATE TABLE projects (
  project_id INT PRIMARY KEY,
  project_name VARCHAR(100),
  lead_id INT             -- references employees.employee_id
);

INSERT INTO projects (project_id, project_name, lead_id) VALUES
(1000, 'Alpha', 101),
(1001, 'Beta', 104),
(1002, 'Data Pipeline', 100),
(1003, 'Gamma', 200); -- lead 200 does not exist
```

-- Customers
```sql
CREATE TABLE customers (
  customer_id INT PRIMARY KEY,
  customer_name VARCHAR(100),
  city VARCHAR(100)
);

INSERT INTO customers (customer_id, customer_name, city) VALUES
(500, 'Green Corp', 'Hyderabad'),
(501, 'Blue Ltd', 'Chennai'),
(502, 'Red LLC', 'Mumbai');
```

-- Orders
```sql
CREATE TABLE orders (
  order_id INT PRIMARY KEY,
  customer_id INT,
  order_amount DECIMAL(10,2),
  order_date DATE
);

INSERT INTO orders (order_id, customer_id, order_amount, order_date) VALUES
(2000, 500, 4500.00, '2024-01-10'),
(2001, 500, 1500.00, '2024-01-12'),
(2002, 501, 3200.00, '2024-02-01'),
(2003, 999, 5000.00, '2024-02-05'); -- customer_id 999 missing (data quality)
```

Notes about the sample data:
- employees.employee_id 106 references department 50 which does not exist to simulate dirty data.
- projects.project_id 1003 has lead_id 200 (missing employee) to simulate mismatches.
- orders.order_id 2003 refers to a non-existent customer to practice reconciliation.

---

## Practice Questions — 30 JOIN-only Problems

Level 1: Beginner — Basic JOINs (1–6)

1. Display employee names along with their department names.
2. Display employee names and their project names (show only employees who are assigned to a project).
3. Display customer names with their order IDs (only matching rows).
4. Display employee names, salary and department name for all employees who have a department.
5. Display order IDs, customer names and order amounts for all existing orders with matching customers.
6. Display employee names and department names for employees working in the IT department (department_name = 'IT').

Level 2: Intermediate — LEFT JOIN and unmatched records (7–12)

7. Display all employees, including those who are not assigned to a department (show NULLs for department fields).
8. Display all departments, including departments that have no employees assigned.
9. Find employees who are not assigned to any department (anti-join pattern).
10. Find departments that do not have any employees assigned (anti-join on departments).
11. Display all customers and their orders, including customers who have never placed an order.
12. Find orders whose customer IDs do not exist in the customers table (payments/orders reconciliation style).

Level 3: Multiple JOINs and self-JOINs (13–18)

13. Display employee names, department names and project names; include only rows where all three exist (use INNER JOINs appropriately).
14. Display each employee’s name along with their manager’s name (self-join). Include employees without a manager.
15. Display employee names, department names and salaries for employees earning more than ₹60,000 (use JOIN to get department name).
16. Display customer names, order IDs and order amounts for orders greater than ₹4,000 (only matching customers).
17. Display all employees and their department and project details, including employees without a matching project (use LEFT JOIN where appropriate).
18. Display all departments and the names of employees working in each department, including departments with no employees (LEFT JOIN departments → employees).

Level 4: Advanced JOIN conditions (19–24)

19. Find employees who belong to the IT department and have at least one project assigned (combine JOINs and appropriate filters).
20. Find employees who have no matching project using a LEFT JOIN anti-join pattern.
21. Find customers who have never placed an order using a LEFT JOIN anti-join.
22. Display employees whose salary is greater than their manager’s salary (self-join with a condition on salary).
23. Find employees whose department exists in the departments table but who have no assigned project (detect dept exists but project missing).
24. Display every employee and their manager (including employees who do not have a manager). Explain in your own words why a LEFT self-join is appropriate here.

Level 5: Complex and interview scenarios (25–30)

25. Find employees working in departments listed in the Departments table with department IDs greater than 20.
26. Find employees assigned to projects whose project_name contains the word 'Data'.
27. Display all customers and matching orders, but only include orders placed for amounts above ₹3,000 while retaining customers with no qualifying orders (LEFT JOIN + filter on order_amount applied correctly so customers without qualifying orders still appear).
28. Identify employees whose department ID does not match any department record, excluding employees whose department ID is NULL (data quality check).
29. Find employees who work in the same department as employee 101, excluding employee 101.
30. Create a detailed employee report showing employee name, department name, manager name and project name. Retain employees even if they have no department, manager or project (use LEFT JOINs appropriately).

---

## Constraints and Rules for Practice
- Use only JOIN / LEFT JOIN / ON / WHERE / aliases / subqueries where necessary.
- Do NOT use GROUP BY, aggregates, HAVING, or window functions for these exercises.
- For anti-joins use LEFT JOIN ... WHERE right_side_key IS NULL or NOT EXISTS as preferred.
- Pay attention to NULLs and data-quality issues in sample data.

---

If you want these questions saved into a repository file (module-3/06-joins-questions.md) I can commit it. Do you want me to add the file to the repo now?