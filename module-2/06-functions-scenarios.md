# Module 2 Part 2: Scenario-Based Questions

## Objective
Apply SQL functions to realistic business problems.

---

## Sample Data Setup

Before solving the scenarios, create and populate these tables:

### Customers table
```sql
CREATE TABLE customers_data (
    customer_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    phone VARCHAR(20),
    city VARCHAR(50)
);

INSERT INTO customers_data (customer_id, first_name, last_name, email, phone, city) VALUES
(1, '  anil  ', 'sharma', 'anil@gmail.com', '9876543210', 'HYDERABAD'),
(2, 'bhavna', 'KAPOOR', 'bhavna.kapoor@gmail.com', NULL, 'Pune'),
(3, 'chetan', 'verma', 'chetan.mail@gmail.com', '9988776655', 'DELHI'),
(4, 'divya', 'nair', 'divya.nair@yahoo.com', '9123456789', 'Bengaluru'),
(5, 'esha', 'GUPTA', NULL, NULL, 'Mumbai');
```

### Products table
```sql
CREATE TABLE products_data (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    price DECIMAL(10,2),
    stock INT,
    discount DECIMAL(5,2)
);

INSERT INTO products_data (product_id, product_name, price, stock, discount) VALUES
(1, 'Laptop', 45000.50, 15, 10.5),
(2, 'Phone', 25000.00, -5, 5.0),
(3, 'Monitor', 12000.75, 18, 0.0),
(4, 'Keyboard', 1500.25, 35, 15.75),
(5, 'Mouse', 850.00, 50, -2.5);
```

### Orders table
```sql
CREATE TABLE orders_data (
    order_id INT PRIMARY KEY,
    customer_id INT,
    order_date DATE,
    total_amount DECIMAL(10,2),
    order_status VARCHAR(50)
);

INSERT INTO orders_data (order_id, customer_id, order_date, total_amount, order_status) VALUES
(101, 1, '2024-01-05', 45000.00, 'Delivered'),
(102, 2, '2024-01-20', 32500.00, 'Processing'),
(103, 3, '2024-02-10', 15000.00, 'Shipped'),
(104, 1, '2024-02-25', 5600.00, 'Delivered'),
(105, 4, '2024-03-15', 2500.00, 'Pending');
```

### Employees table
```sql
CREATE TABLE employees_data (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    salary DECIMAL(10,2),
    joining_date DATE,
    phone VARCHAR(20),
    city VARCHAR(50)
);

INSERT INTO employees_data (employee_id, first_name, last_name, salary, joining_date, phone, city) VALUES
(1, 'anil', 'sharma', 50000.00, '2020-01-15', '9876543210', 'Hyderabad'),
(2, 'bhavna', 'kapoor', 65000.00, '2021-06-11', NULL, 'Pune'),
(3, 'chetan', 'verma', 42000.00, '2019-12-01', '9988776655', 'Delhi'),
(4, 'divya', 'nair', 80000.00, '2023-02-20', '9123456789', 'Bengaluru'),
(5, 'esha', 'gupta', 55000.00, '2022-08-10', NULL, 'Mumbai');
```

### Students table
```sql
CREATE TABLE students_data (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(100),
    marks INT,
    grade VARCHAR(5),
    exam_date DATE
);

INSERT INTO students_data (student_id, student_name, marks, grade, exam_date) VALUES
(1, '  anil  ', 85, 'A', '2024-01-15'),
(2, 'bhavna', 72, 'B', '2024-01-20'),
(3, 'chetan', 45, NULL, '2024-01-22'),
(4, 'divya', 92, 'A', '2024-02-10'),
(5, 'esha', 58, 'C', '2024-02-12');
```

---

## Scenario 1: Customer Data Cleanup
A company stores customer names and contact details.

### Questions
1. Display customer names in uppercase.
2. Remove extra spaces from customer names.
3. Combine first_name and last_name into a full name.
4. Replace missing phone numbers with `N/A`.
5. Find customers whose email contains `gmail`.
6. Show customer name and city in lowercase.

---

## Scenario 2: Product Pricing
A retail company stores product prices and stock data.

### Questions
1. Round product prices to two decimals.
2. Check which products have negative quantity or invalid discount.
3. Display the absolute value of discount differences.
4. Find products with price above 5000.
5. Use `CASE` to classify products as `Expensive` or `Affordable` (expensive > 20000).
6. Convert product IDs to text for reports.

---

## Scenario 3: Order Report
A business wants to analyze order frequency and dates.

### Questions
1. Show order dates in `DD-MM-YYYY` format.
2. Find orders placed in the last 30 days.
3. Calculate the difference between order_date and today's date.
4. Extract the month from each order date.
5. Show the year of all orders.
6. Use `DATE_ADD` to calculate delivery dates (add 10 days to order date).

---

## Scenario 4: Employee Salary Report
An HR team needs a salary summary.

### Questions
1. Show employee names in uppercase.
2. Display full employee names (first + last).
3. Replace null phone values with `Not Available`.
4. Classify employees as `High Salary` or `Normal Salary` using `CASE` (high > 60000).
5. Display joining date in a readable format.
6. Find employees whose joining year is 2020 or later.

---

## Scenario 5: Student Performance Analysis
A trainer wants to analyze student records.

### Questions
1. Add a column label such as `Pass` or `Fail` using `CASE` (pass >= 50).
2. Convert marks to text if needed for display.
3. Show student names in lowercase for reports.
4. Replace missing grades with `Absent`.
5. Extract the month from exam dates.
6. Calculate the difference between exam_date and today's date.

---

## Scenario 6: Interview Questions
Prepare answers to the following:

1. Why are string functions important in SQL?
2. When would you use `COALESCE`?
3. What is the purpose of `CASE`?
4. Why do we need date functions in reports?
5. What is the difference between `CAST` and `CONVERT`?
6. How do you check for missing values before applying functions?
7. What is the benefit of formatting dates?
8. Which functions help improve report readability?

---

## Final Practice Tip
A good SQL developer does not just fetch data; they also transform it, clean it, and present it in a meaningful way using SQL functions.
