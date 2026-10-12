# Module 2 Part 1: Scenario-Based Questions

## Objective
Apply SELECT, DISTINCT, and WHERE logic to realistic business situations.

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

### Example design
```sql
students (
    student_id,
    student_name,
    city,
    marks,
    status,
    phone_number
)
```

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


