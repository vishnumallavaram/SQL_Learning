# Module 2 Part 2: Practical Work - SQL Functions

## Objective
Practice common SQL functions for string, date, numeric, conversion, and conditional logic.

---

## Setup
Create a sample table:

```sql
CREATE TABLE employee_data (
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100),
    salary DECIMAL(10,2),
    joining_date DATE,
    phone VARCHAR(20),
    city VARCHAR(50)
);
```

Insert sample data:

```sql
INSERT INTO employee_data (employee_id, first_name, last_name, email, salary, joining_date, phone, city) VALUES
(1, 'anil', 'sharma', 'anil@gmail.com', 50000.00, '2020-01-15', '9876543210', 'Hyderabad'),
(2, 'bhavna', 'kapoor', 'bhavna@gmail.com', 65000.00, '2021-06-11', NULL, 'Pune'),
(3, 'chetan', 'verma', 'chetan@gmail.com', 42000.00, '2019-12-01', '9988776655', 'Delhi'),
(4, 'divya', 'nair', 'divya@gmail.com', 80000.00, '2023-02-20', '9123456789', 'Bengaluru');
```

---

## Exercise 1: Convert to uppercase
```sql
SELECT UPPER(first_name) AS first_name_upper
FROM employee_data;
```

---

## Exercise 2: Convert to lowercase
```sql
SELECT LOWER(city) AS city_lower
FROM employee_data;
```

---

## Exercise 3: Combine first and last name
```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM employee_data;
```

---

## Exercise 4: Trim spaces
```sql
SELECT TRIM('   SQL   ');
```

---

## Exercise 5: Get string length
```sql
SELECT first_name, LENGTH(first_name) AS name_length
FROM employee_data;
```

---

## Exercise 6: Replace characters
```sql
SELECT REPLACE(email, '@gmail.com', '@mail.com')
FROM employee_data;
```

---

## Exercise 7: Get substring
```sql
SELECT SUBSTRING(first_name, 1, 3)
FROM employee_data;
```

---

## Exercise 8: Round salary
```sql
SELECT ROUND(salary, 2)
FROM employee_data;
```

---

## Exercise 9: Find absolute value
```sql
SELECT ABS(-15);
```

---

## Exercise 10: Get current date
```sql
SELECT CURRENT_DATE;
```

---

## Exercise 11: Get current timestamp
```sql
SELECT CURRENT_TIMESTAMP;
```

---

## Exercise 12: Extract year from date
```sql
SELECT YEAR(joining_date)
FROM employee_data;
```

---

## Exercise 13: Extract month and day
```sql
SELECT MONTH(joining_date), DAY(joining_date)
FROM employee_data;
```

---

## Exercise 14: Add days to a date
```sql
SELECT DATE_ADD('2025-01-10', INTERVAL 10 DAY);
```

---

## Exercise 15: Difference between dates
```sql
SELECT DATEDIFF('2025-02-10', '2025-01-15');
```

---

## Exercise 16: Format date
```sql
SELECT DATE_FORMAT(joining_date, '%d-%m-%Y')
FROM employee_data;
```

---

## Exercise 17: Use COALESCE
```sql
SELECT first_name, COALESCE(phone, 'N/A') AS phone_number
FROM employee_data;
```

---

## Exercise 18: Use CASE
```sql
SELECT first_name,
       CASE
           WHEN salary > 60000 THEN 'High Salary'
           ELSE 'Normal Salary'
       END AS salary_status
FROM employee_data;
```

---

## Exercise 19: Use IF
```sql
SELECT first_name,
       IF(salary > 50000, 'Eligible', 'Not Eligible')
FROM employee_data;
```

---

## Exercise 20: Convert text to a number
```sql
SELECT CAST('123' AS INT);
```

---

## Exercise 21: Convert number to text
```sql
SELECT CAST(123 AS VARCHAR(10));
```

---

## Exercise 22: Query with multiple functions
```sql
SELECT CONCAT(UPPER(first_name), ' ', UPPER(last_name)) AS full_name,
       DATE_FORMAT(joining_date, '%d-%m-%Y') AS join_date
FROM employee_data;
```

---

## Reflection Questions
1. Why do we use string functions?
2. When would you use `COALESCE`?
3. What is the difference between `CAST` and `CONVERT`?
4. Which function helps you compare date ranges?
5. Why do we use `CASE` in queries?
6. How do `ROUND` and `ABS` help in data analysis?
7. Why are date functions important in business reports?
8. What is the difference between `DATE_ADD` and `DATE_SUB`?


