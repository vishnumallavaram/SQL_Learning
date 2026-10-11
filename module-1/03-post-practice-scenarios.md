# Module 1: Post-Practice Scenarios

## Objective
Apply the concepts learned in this module to real-world business situations. Each scenario focuses on designing tables, using constraints, and making safe schema changes.

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
These scenarios and interview questions prepare you for real-world database work and technical interviews. Understanding not just the syntax, but the reasoning behind design decisions, is what makes a professional database developer. The ability to explain your choices and consider the impact of your changes is as important as knowing the SQL commands themselves.
