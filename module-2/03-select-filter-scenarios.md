# Module 2 Part 1: Scenario-Based Questions

## Objective
Apply SELECT, DISTINCT, and WHERE logic to realistic business situations.

---

## Practice Data Setup
Use the following sample tables before solving the questions in this file.

### 1. Students table
```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(100),
    city VARCHAR(50),
    marks INT,
    status VARCHAR(20),
    phone_number VARCHAR(20)
);

INSERT INTO students (student_id, student_name, city, marks, status, phone_number) VALUES
(1, 'Anil', 'Hyderabad', 81, 'Active', '9876543210'),
(2, 'Bhavna', 'Pune', 72, 'Inactive', NULL),
(3, 'Chetan', 'Hyderabad', 90, 'Active', '9988776655'),
(4, 'Divya', 'Bengaluru', 68, 'Active', '9123456789'),
(5, 'Esha', 'Delhi', 85, 'Inactive', NULL),
(6, 'Farhan', 'Hyderabad', 76, 'Active', '9090909090'),
(7, 'Gita', 'Chennai', 60, 'Active', '9812345678'),
(8, 'Hemanth', 'Pune', 92, 'Active', NULL);
```

### 2. Customers table
```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100),
    city VARCHAR(50),
    status VARCHAR(20),
    email VARCHAR(100),
    phone VARCHAR(20)
);

INSERT INTO customers (customer_id, customer_name, city, status, email, phone) VALUES
(1, 'Aarav', 'Hyderabad', 'Active', 'aarav@gmail.com', '9000112233'),
(2, 'Bhavya', 'Bengaluru', 'Active', 'bhavya@gmail.com', '9000223344'),
(3, 'Chitra', 'Pune', 'Inactive', NULL, NULL),
(4, 'David', 'Hyderabad', 'Active', 'david@gmail.com', NULL),
(5, 'Esha', 'Delhi', 'Inactive', 'esha@gmail.com', '9000556677'),
(6, 'Farah', 'Bengaluru', 'Active', 'farah@gmail.com', '9000667788');
```

### 3. Products table
```sql
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    product_name VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10,2),
    stock INT,
    is_active VARCHAR(10)
);

INSERT INTO products (product_id, product_name, category, price, stock, is_active) VALUES
(1, 'Laptop', 'Electronics', 45000.00, 15, 'Yes'),
(2, 'Phone', 'Electronics', 25000.00, 30, 'Yes'),
(3, 'Chair', 'Furniture', 2500.00, 20, 'Yes'),
(4, 'Table', 'Furniture', 5000.00, 12, 'No'),
(5, 'Monitor', 'Electronics', 12000.00, 18, 'Yes'),
(6, 'Keyboard', 'Electronics', 1500.00, 35, 'Yes'),
(7, 'Desk', 'Furniture', 8000.00, 10, 'Yes'),
(8, 'Router', 'Electronics', 2200.00, 25, 'No');
```

### 4. Employees table
```sql
CREATE TABLE employees (
    employee_id INT PRIMARY KEY,
    employee_name VARCHAR(100),
    department VARCHAR(50),
    salary DECIMAL(10,2),
    city VARCHAR(50),
    manager_id INT,
    phone VARCHAR(20)
);

INSERT INTO employees (employee_id, employee_name, department, salary, city, manager_id, phone) VALUES
(1, 'Asha', 'Sales', 50000.00, 'Hyderabad', 10, '9000112233'),
(2, 'Bharat', 'Finance', 65000.00, 'Delhi', 11, NULL),
(3, 'Chandru', 'Sales', 48000.00, 'Bengaluru', 10, '9000445566'),
(4, 'Deepa', 'HR', 42000.00, 'Hyderabad', 12, '9000778899'),
(5, 'Ethan', 'Finance', 70000.00, 'Pune', 11, NULL),
(6, 'Fiza', 'Sales', 53000.00, 'Delhi', 10, '9000889977');
```

### 5. Books table
```sql
CREATE TABLE books (
    book_id INT PRIMARY KEY,
    title VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10,2),
    member_city VARCHAR(50),
    member_phone VARCHAR(20)
);

INSERT INTO books (book_id, title, category, price, member_city, member_phone) VALUES
(1, 'Database Design', 'Computer Science', 350.00, 'Hyderabad', '9000112233'),
(2, 'Data Structures', 'Computer Science', 280.00, 'Pune', NULL),
(3, 'Dreams of India', 'History', 220.00, 'Delhi', '9000556677'),
(4, 'Digital Marketing', 'Business', 500.00, 'Hyderabad', '9000667788'),
(5, 'The Lost City', 'Fiction', 180.00, 'Bengaluru', NULL),
(6, 'Algorithms', 'Computer Science', 420.00, 'Pune', '9000998877');
```

### 6. Bookings table
```sql
CREATE TABLE bookings (
    booking_id INT PRIMARY KEY,
    guest_name VARCHAR(100),
    room_type VARCHAR(50),
    city VARCHAR(50),
    status VARCHAR(20),
    guest_phone VARCHAR(20),
    total_amount DECIMAL(10,2)
);

INSERT INTO bookings (booking_id, guest_name, room_type, city, status, guest_phone, total_amount) VALUES
(1, 'Aditi', 'Deluxe', 'Delhi', 'Active', '9000112233', 4200.00),
(2, 'Bharat', 'Standard', 'Hyderabad', 'Inactive', NULL, 1800.00),
(3, 'Chetan', 'Deluxe', 'Delhi', 'Active', '9000223344', 5200.00),
(4, 'Divya', 'Suite', 'Bengaluru', 'Active', '9000334455', 6500.00),
(5, 'Esha', 'Deluxe', 'Pune', 'Active', NULL, 3100.00),
(6, 'Farah', 'Standard', 'Delhi', 'Inactive', '9000445566', 2400.00);
```

---

## Scenario 1: Student Records
A college stores student information.

### Task
Write queries for the following:

1. Display all student names and cities.
2. Find students from Hyderabad.
3. Find students whose marks are greater than 75.
4. Show unique cities from student records.
5. Find students from Hyderabad and whose status is Active.
6. Find students whose phone number is missing.

---

## Scenario 2: E-Commerce Store
An online store manages customer and product data.

### Task
Write queries for the following:

1. Display all customers with their city and status.
2. Find products priced above 5000.
3. Find customers who live in Hyderabad or Bengaluru.
4. Find products in Electronics or Furniture category.
5. List unique product categories.
6. Find customers whose email is not available.

---

## Scenario 3: Employee Directory
A company records employee details.

### Task
Write queries using the following table:

```sql
employees (
    employee_id,
    employee_name,
    department,
    salary,
    city,
    manager_id,
    phone
)
```

Questions:
1. Show all employees in the Sales department.
2. Show employees with salary above 50000.
3. List unique cities from employee records.
4. Find employees who are not in Delhi.
5. Find employees with missing phone numbers.
6. Find employees in Finance or HR department with salary below 40000.

---

## Scenario 4: Library System
A library stores books, members, and issue records.

### Questions
1. Show all books with price above 300.
2. Find books issued to members from Hyderabad.
3. Show unique member cities.
4. Find books with title starting with 'D'.
5. Find members whose phone is NULL.
6. Find books in the Computer Science category.

---

## Scenario 5: Hotel Booking System
A hotel keeps records of rooms and bookings.

### Questions
1. Display all bookings from Delhi.
2. Find guests whose room type is Deluxe.
3. Find all active bookings.
4. Show unique room types.
5. Find bookings with missing guest phone numbers.
6. Find bookings where total_amount is between 2000 and 5000.

---

## Scenario 6: Business Interview Questions
Prepare answers for the following:

1. Why is filtering important in SQL?
2. When would you use `DISTINCT`?
3. How is `WHERE` different from `SELECT`?
4. Why is `IS NULL` used instead of `= NULL`?
5. Which filter is better for a list of values: `IN` or multiple `OR`s?
6. When would you use `LIKE`?
7. Why should you avoid writing too many complex conditions without parentheses?
8. What is the difference between `BETWEEN` and `IN`?

---

## Final Practice Note
A good SQL developer should know not just how to write queries, but also when to use the correct filter type for the correct business requirement.
