# Module 1: Post-Practice Scenarios

## Objective
Apply the concepts learned in this module to real-world business situations. Each scenario focuses on designing tables, using constraints, and making safe schema changes.

---

## Sample Data Setup

Before solving the scenarios, create and populate these tables:

### Students table
```sql
CREATE TABLE students (
    student_id INT PRIMARY KEY,
    student_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    city VARCHAR(50),
    phone_number VARCHAR(20),
    join_date DATE DEFAULT CURRENT_DATE,
    status VARCHAR(20) DEFAULT 'Active'
);

INSERT INTO students (student_id, student_name, email, city, phone_number, join_date, status) VALUES
(1, 'Anil Kumar', 'anil@gmail.com', 'Hyderabad', '9876543210', '2024-01-15', 'Active'),
(2, 'Bhavna Sharma', 'bhavna@gmail.com', 'Pune', NULL, '2024-02-20', 'Active'),
(3, 'Chetan Singh', 'chetan@gmail.com', 'Delhi', '9988776655', '2024-03-10', 'Inactive'),
(4, 'Divya Patel', 'divya@gmail.com', 'Bengaluru', '9123456789', '2024-01-05', 'Active'),
(5, 'Esha Verma', 'esha@gmail.com', 'Hyderabad', NULL, '2024-04-12', 'Active');
```

### Enrollments table
```sql
CREATE TABLE enrollments (
    enrollment_id INT PRIMARY KEY,
    student_id INT,
    course_name VARCHAR(100),
    enrollment_date DATE,
    FOREIGN KEY (student_id) REFERENCES students(student_id)
);

INSERT INTO enrollments (enrollment_id, student_id, course_name, enrollment_date) VALUES
(1, 1, 'SQL Basics', '2024-01-20'),
(2, 1, 'Python Programming', '2024-02-05'),
(3, 2, 'SQL Advanced', '2024-02-25'),
(4, 3, 'Database Design', '2024-03-15'),
(5, 4, 'SQL Basics', '2024-01-10'),
(6, 5, 'Python Programming', '2024-04-18');
```

### Customers table
```sql
CREATE TABLE customers (
    customer_id INT PRIMARY KEY,
    customer_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone VARCHAR(20),
    city VARCHAR(50),
    registration_date DATE DEFAULT CURRENT_DATE,
    customer_status VARCHAR(20) DEFAULT 'Active'
);

INSERT INTO customers (customer_id, customer_name, email, phone, city, registration_date, customer_status) VALUES
(1, 'Aarav Singh', 'aarav@gmail.com', '9000112233', 'Hyderabad', '2024-01-01', 'Active'),
(2, 'Bhavya Kapoor', 'bhavya@gmail.com', '9000223344', 'Pune', '2024-01-15', 'Active'),
(3, 'Chitra Nair', 'chitra@gmail.com', NULL, 'Bengaluru', '2024-02-10', 'Inactive'),
(4, 'David Khan', 'david@gmail.com', '9000445566', 'Delhi', '2024-02-20', 'Active'),
(5, 'Esha Gupta', 'esha@gmail.com', '9000556677', 'Hyderabad', '2024-03-05', 'Active');
```

### Orders table
```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY,
    customer_id INT NOT NULL,
    order_date DATE NOT NULL,
    total_amount DECIMAL(10,2) CHECK (total_amount > 0),
    order_status VARCHAR(20),
    expected_delivery_date DATE,
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

INSERT INTO orders (order_id, customer_id, order_date, total_amount, order_status, expected_delivery_date) VALUES
(101, 1, '2024-01-05', 5500.00, 'Delivered', '2024-01-12'),
(102, 2, '2024-01-20', 3200.00, 'Processing', '2024-02-02'),
(103, 1, '2024-02-15', 8900.00, 'Shipped', '2024-02-25'),
(104, 4, '2024-02-28', 2100.00, 'Delivered', '2024-03-07'),
(105, 5, '2024-03-10', 6700.00, 'Processing', '2024-03-20');
```

### Members table
```sql
CREATE TABLE members (
    member_id INT PRIMARY KEY,
    member_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    phone VARCHAR(20),
    city VARCHAR(50),
    membership_date DATE DEFAULT CURRENT_DATE
);

INSERT INTO members (member_id, member_name, email, phone, city, membership_date) VALUES
(1, 'Arjun Reddy', 'arjun@gmail.com', '9100112233', 'Hyderabad', '2023-06-01'),
(2, 'Bhavna Desai', 'bhavna@gmail.com', '9100223344', 'Pune', '2023-07-15'),
(3, 'Chandru Pillai', 'chandru@gmail.com', NULL, 'Kochi', '2023-08-20'),
(4, 'Divya Sharma', 'divya@gmail.com', '9100445566', 'Delhi', '2023-09-10'),
(5, 'Farah Ali', 'farah@gmail.com', '9100556677', 'Mumbai', '2023-10-05');
```

### Books table
```sql
CREATE TABLE books (
    book_id INT PRIMARY KEY,
    book_title VARCHAR(100) NOT NULL,
    author VARCHAR(100),
    price DECIMAL(10,2) CHECK (price > 0),
    category VARCHAR(50),
    quantity_available INT DEFAULT 0
);

INSERT INTO books (book_id, book_title, author, price, category, quantity_available) VALUES
(1, 'Introduction to SQL', 'Chris Post', 500.00, 'Technology', 5),
(2, 'The Great Gatsby', 'F. Scott Fitzgerald', 450.00, 'Fiction', 3),
(3, 'A Brief History of Time', 'Stephen Hawking', 650.00, 'Science', 2),
(4, 'Database Design Patterns', 'Edward Jones', 800.00, 'Technology', 4),
(5, 'The God Delusion', 'Richard Dawkins', 550.00, 'Philosophy', 1);
```

### Issued_books table
```sql
CREATE TABLE issued_books (
    issue_id INT PRIMARY KEY,
    member_id INT NOT NULL,
    book_id INT NOT NULL,
    issue_date DATE,
    return_date DATE,
    FOREIGN KEY (member_id) REFERENCES members(member_id),
    FOREIGN KEY (book_id) REFERENCES books(book_id)
);

INSERT INTO issued_books (issue_id, member_id, book_id, issue_date, return_date) VALUES
(1, 1, 1, '2024-01-10', '2024-01-25'),
(2, 2, 2, '2024-01-15', NULL),
(3, 1, 3, '2024-02-01', '2024-02-18'),
(4, 4, 4, '2024-02-05', NULL),
(5, 3, 5, '2024-02-20', '2024-03-05');
```

---

## Scenario 1: Student Management System

### Business Need
A college wants to manage students and their course enrollments.

### Task
Create a database design for the following:
- Students table
- Enrollments table

### Requirements
- Every student must have a unique student ID
- Student name should be required
- Email should be unique
- Every enrollment should be linked to a valid student
- Add a common field such as city, phone number, or join date
- Add a new column after the initial design to track student status
- Explain how the relationship works between students and enrollments

### Questions to Answer
- Why is student ID important?
- Why is email unique?
- Why is the foreign key required between students and enrollments?
- What happens if a student record is deleted?
- How would you add a new field to the students table safely?

---

## Scenario 2: E-Commerce Store

### Business Need
An online store needs to manage customers and their orders.

### Task
Create a schema for:
- Customers table
- Orders table

### Requirements
- Each customer must have a unique ID
- Customer email must be unique
- Customer name should be required
- Each order must be linked to an existing customer
- Add a field for order amount
- Add a field for order status
- Add a new column to track order delivery date
- Create a backup copy of the table before making schema changes

### Questions to Answer
- Why is a foreign key important in orders?
- What is the role of the total amount field?
- What happens if an order references a customer that does not exist?
- Why is it useful to create a backup table before modifying the structure?
- Which field should be checked to ensure a valid order value?

---

## Scenario 3: Library Management System

### Business Need
A library wants to track members, books, and issued books.

### Task
Create a schema for:
- Members table
- Books table
- Issued books table

### Requirements
- Each member must have a unique ID
- Each book must have a unique ID
- Issue records must be linked to valid members and books
- Add a field for book price
- Add a field for issue date
- Add a field for return date
- Add a new column for member phone number
- Practice a table change such as renaming or dropping a column

### Questions to Answer
- Why do we need both member and book IDs?
- What is the value of a foreign key in issued_books?
- How does the library know which books are issued to which member?
- Why is it useful to add new columns over time?
- What is the difference between deleting one issue record and deleting a full table?

---

## Final Reflection - Interview-Style Questions

After completing all three scenarios, prepare answers to the following questions as if in a job interview:

### Database Design Questions

1. **"Explain why a primary key is important. Can a table function without one?"**

2. **"A customer table has 100,000 rows. How would you safely add a new column without risking data loss?"**

3. **"What is the difference between a primary key and a unique constraint? When would you use each?"**

4. **"You need to update the phone number for one customer. Write the approach you would take and explain why WHERE clause is critical."**

### Real-World Scenarios

5. **"If you accidentally ran `DELETE FROM orders;` without a WHERE clause, what would happen? How would you prevent this mistake?"**

6. **"When would you use TRUNCATE instead of DELETE? Explain with an example."**

7. **"A business wants to track a new property for all products. How would you modify the table structure and ensure no data is lost?"**

### Data Integrity Questions

8. **"Why is a foreign key important? What happens if you try to insert an order for a customer that does not exist?"**

9. **"A table has a column with CHECK constraint (price > 0). Can you insert a negative price? Why or why not?"**

10. **"Explain the difference between NOT NULL and DEFAULT constraints. When would you use each?"**

### Testing and Maintenance

11. **"Before making major changes to a production table, what steps would you take to ensure safety?"**

12. **"How would you test a schema change without affecting the live database?"**

13. **"You need to remove a column that might be used by an application. What would you check first?"**

### Practical Problem-Solving

14. **"Design a simple table for a video rental store. What columns would you include? What constraints would you add?"**

15. **"An email column currently has VARCHAR(100), but new email addresses are longer. How would you handle this change?"**

16. **"Describe a situation where you would need to update multiple rows at once. How would you ensure only the intended rows are modified?"**

### Decision-Making

17. **"When deciding between VARCHAR and CHAR data types, what factors would you consider?"**

18. **"A business process requires temporarily storing calculated results before applying them to permanent tables. What would you use and why?"**

19. **"You have a backup table from last week. How would you use it if something goes wrong with the current table?"**

20. **"A colleague suggests dropping a table to 'clean up' old data. What would you recommend instead?"**

---

## Summary
These scenarios and interview questions prepare you for real-world database work and technical interviews. Understanding not just the syntax, but the reasoning behind design decisions, is what makes a great SQL developer.
