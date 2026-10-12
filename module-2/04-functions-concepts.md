# Module 2 Part 2: SQL Functions

## Objective
Learn the most common SQL functions used for strings, dates, numbers, conversions, and conditional logic.

---

## 1. String Functions
These are used to manipulate text values.

### When to use
- Clean names or email values.
- Convert text to upper or lower case.
- Remove extra spaces.
- Extract part of a string.
- Combine values from multiple columns.

### Examples
```sql
SELECT CONCAT('SQL', ' ', 'Practice');
```

```sql
SELECT LOWER('HYDERABAD');
SELECT UPPER('hyderabad');
```

```sql
SELECT TRIM('  SQL  ');
SELECT LTRIM('   SQL');
SELECT RTRIM('SQL   ');
```

```sql
SELECT LENGTH('SQL');
SELECT CHAR_LENGTH('SQL');
```

```sql
SELECT SUBSTRING('SQL Practice', 1, 3);
```

```sql
SELECT REPLACE('Hello World', 'World', 'SQL');
```

```sql
SELECT LEFT('SQL', 2);
SELECT RIGHT('Practice', 4);
```

---

## 2. Numeric Functions
Used for calculations and math operations.

### When to use
- Compare prices and totals.
- Round values for reporting.
- Calculate discounts or percentages.
- Handle negative numbers safely.

### Examples
```sql
SELECT ABS(-25);
```

```sql
SELECT ROUND(125.678, 2);
```

```sql
SELECT CEIL(12.3);
SELECT FLOOR(12.9);
```

```sql
SELECT MOD(15, 4);
```

```sql
SELECT POWER(3, 2);
SELECT SQRT(81);
```

---

## 3. Date and Time Functions
Used for working with dates and timestamps.

### When to use
- Find orders from the last 30 days.
- Extract year or month from dates.
- Add or subtract days from a date.
- Compare dates in reports.

### Examples
```sql
SELECT CURRENT_DATE;
```

```sql
SELECT CURRENT_TIMESTAMP;
SELECT NOW();
```

```sql
SELECT YEAR('2025-01-15');
SELECT MONTH('2025-01-15');
SELECT DAY('2025-01-15');
```

```sql
SELECT DATE_ADD('2025-01-15', INTERVAL 10 DAY);
SELECT DATE_SUB('2025-01-15', INTERVAL 5 DAY);
```

```sql
SELECT DATEDIFF('2025-02-10', '2025-01-15');
```

```sql
SELECT EXTRACT(YEAR FROM '2025-01-15');
```

```sql
SELECT DATE_FORMAT('2025-01-15', '%d-%m-%Y');
```

```sql
SELECT TIMESTAMPDIFF(YEAR, '2000-01-01', '2025-01-01');
```

---

## 4. Conversion / Type Casting
These functions convert one type of data into another.

### When to use
- Convert text to number before comparison.
- Convert number to text for display.
- Convert date strings into proper database date values.

### Examples
```sql
SELECT CAST(123 AS VARCHAR(10));
```

```sql
SELECT CONVERT(VARCHAR, 123);
```

```sql
SELECT TO_CHAR(CURRENT_DATE, 'YYYY-MM-DD');
```

```sql
SELECT STR_TO_DATE('15-01-2025', '%d-%m-%Y');
```

---

## 5. NULL Handling Functions
These help manage missing values.

### When to use
- Replace missing phone numbers with a default value.
- Avoid errors when a value is null.
- Provide user-friendly output.

### Examples
```sql
SELECT COALESCE(phone, 'No Phone')
FROM customers;
```

```sql
SELECT NULLIF(10, 10);
```

```sql
SELECT IFNULL(phone, 'Missing');
```

---

## 6. Conditional Functions
Used to return different values depending on a condition.

### When to use
- Label data as high/low salary.
- Classify products as expensive or cheap.
- Apply business logic in reports.

### Example
```sql
SELECT customer_name,
       CASE
           WHEN city = 'Hyderabad' THEN 'Local'
           ELSE 'Other'
       END AS location_type
FROM customers;
```

### IF function
```sql
SELECT IF(price > 1000, 'Expensive', 'Cheap')
FROM products;
```

---

## 7. Use of Functions in Real Queries

### Example 1: Format customer names
```sql
SELECT UPPER(customer_name)
FROM customers;
```

### Example 2: Combine names
```sql
SELECT CONCAT(first_name, ' ', last_name) AS full_name
FROM employees;
```

### Example 3: Find orders in the last 30 days
```sql
SELECT *
FROM orders
WHERE order_date >= DATE_SUB(CURRENT_DATE, INTERVAL 30 DAY);
```

### Example 4: Replace missing values
```sql
SELECT COALESCE(phone, 'N/A')
FROM customers;
```

---

## 8. Important Rules
- Function syntax differs between databases.
- Data type conversion must be done carefully.
- Date functions vary across SQL databases.
- Use `COALESCE` when missing values must be handled.
- Use `CASE` for custom business rules.

---

## 9. Common Practice Questions
1. What is the difference between `CONCAT` and `CONCAT_WS`?
2. Why do we use `COALESCE`?
3. How do you convert text to date?
4. Which function extracts the month from a date?
5. How do you convert all names to uppercase?
6. What is the purpose of `CASE`?
7. Why is `CAST` useful?
8. What happens when `NULLIF` receives equal values?

---

## Summary
SQL functions help transform, clean, compare, and report data. They are essential for real-world databases because raw data is rarely ready to be displayed directly.


