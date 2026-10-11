# Module 1: Practical Work

## Objective
Apply all major SQL concepts from this module by creating a realistic business database and performing the required operations carefully.

---

## Practice Scenario: Online Store Database

### Business Need
You are creating a database for an online store that manages:
- Products
- Customers
- Orders
- Temporary data for testing
- Backup tables for safe changes

---

## Task 1: Create a Database
- Create a new database for the online store.
- Choose a meaningful database name.
- Switch to the new database before creating tables.

**What you should understand:**
- Databases are containers for related data.
- All tables in the system should be created inside the selected database.

---

## Task 2: Create a Products Table
Create a products table with the following requirements:

**Add the following columns:**
- Unique product identifier
- Product name
- Category
- Price
- Quantity in stock
- Product active status
- Discount percentage
- Date the product was added

**Apply the following constraints:**
- Primary key on the product ID
- Required values where necessary
- Default value for stock quantity
- Default value for active status
- Price must be positive
- Quantity must not be negative

**What you should understand:**
- Tables should reflect real business data
- Constraints protect the quality of the data
- Data types must match the kind of information being stored

---

## Task 3: Create Customers Table
Create a customers table with these requirements:

**Columns:**
- Customer ID
- Full name
- Email
- Phone number
- City
- Registration date
- Active status

**Constraints:**
- Primary key
- Required fields
- Unique email
- Default value for active status
- Default value for registration date

**What you should understand:**
- Why email should be unique
- Why some fields must be required
- How defaults reduce manual work

---

## Task 4: Create Orders Table
Create an orders table that links customers and purchases.

**Columns:**
- Order ID
- Customer ID
- Order date
- Total amount
- Order status
- Expected delivery date

**Constraints:**
- Primary key
- Foreign key to customers
- Required values where needed
- Positive total amount

**What you should understand:**
- Foreign keys connect business entities
- Orders should belong to valid customers
- Relationships improve data integrity

---

## Task 5: Insert a Single Product
Insert one product record into the products table.

**Use a realistic product such as:**
- Laptop
- Phone
- Headphones
- Chair
- Monitor

**Requirements:**
- Provide values for all required fields
- Use realistic price, category, and stock

**What you should understand:**
- How single row inserts work
- The importance of required columns
- The difference between required and optional fields

---

## Task 6: Insert Multiple Products
Insert several products at once.

**Requirements:**
- Add at least 5 products
- Include at least 3 different categories
- Some rows may omit optional fields
- Some rows should use default values
- Include one product with a discount value

**What you should understand:**
- Multiple insert statements save time
- Optional columns may be left empty
- Defaults are used automatically when values are missing

---

## Task 7: Insert Customers
Insert at least 4 customer records.

**Requirements:**
- Provide realistic names and email values
- At least one customer should have a missing phone number
- Ensure email values are unique
- Use different cities

**What you should understand:**
- Unique constraints prevent duplicate email values
- Optional fields may be left blank
- Real business systems often have incomplete or missing optional data

---

## Task 8: Insert Orders
Insert at least 3 order records.

**Requirements:**
- Each order must reference an existing customer
- Include realistic dates
- Include realistic order totals and status
- Each order should satisfy all constraints

**What you should understand:**
- Foreign keys ensure valid relationships
- Table linking is essential in relational databases

---

## Task 9: Update a Single Product
Update one product record.

**Scenario:**
You need to increase or decrease the price of a product due to a sale.

**Tasks:**
- Choose one product
- Change only the relevant field
- Use a condition to target the correct row
- Verify that only that product changed

**What you should understand:**
- UPDATE works on rows that match a condition
- WHERE is necessary to avoid changing every row
- Single row updates are common in real systems

---

## Task 10: Update Multiple Products
Update several products at once.

**Scenario:**
All electronics products need a product price adjustment.

**Tasks:**
- Identify products in a specific category
- Update the price or discount for all matching products
- Verify the number of rows affected

**What you should understand:**
- UPDATE can affect many rows at once
- Conditions determine which records change
- Data updates should be carefully reviewed

---

## Task 11: Update Multiple Columns
Update one customer record with multiple values.

**Scenario:**
A customer changed their phone number and moved to another city.

**Tasks:**
- Modify the customer record
- Update both phone number and city
- Verify both values changed

**What you should understand:**
- Multiple columns can be updated in one statement
- Several values can change for the same row

---

## Task 12: Delete One Record
Delete a specific product or customer.

**Scenario:**
A product is discontinued, or a customer record is being removed from the system.

**Tasks:**
- Decide which row to delete
- Use a condition to target only the selected record
- Verify the record is gone

**What you should understand:**
- DELETE removes rows from a table
- WHERE is critical for safety
- Always confirm what is being removed before executing

---

## Task 13: Delete Multiple Records
Delete multiple rows using one condition.

**Scenario:**
Remove inactive or obsolete records based on a condition.

**Tasks:**
- Select rows matching a common condition
- Delete all matching records
- Verify how many rows were removed

**What you should understand:**
- DELETE can remove more than one row
- Conditional deletion is common in maintenance work

---

## Task 14: Alter Table - Add a Column
Add a new column to the products table.

**Scenario:**
The store wants to track supplier information.

**Tasks:**
- Add a supplier name column
- Choose an appropriate data type
- Decide whether it should allow NULL values
- Verify the new column exists

**What you should understand:**
- ALTER TABLE adjusts an existing structure
- New columns are added without deleting table data
- Good design often changes over time

---

## Task 15: Alter Table - Remove a Column
Remove a column that is no longer needed.

**Scenario:**
A temporary or unused field should be removed.

**Tasks:**
- Drop an unnecessary column
- Verify the table still works after the change
- Check whether any data risk is created

**What you should understand:**
- Dropping columns is a structural change
- Structural changes should be planned carefully

---

## Task 16: Alter Table - Rename a Column
Rename a column in one of the tables.

**Scenario:**
A column name should be clearer for business users.

**Tasks:**
- Rename a column to a clearer name
- Ensure the new name is meaningful
- Verify the table still works correctly

**What you should understand:**
- Column names affect readability and maintenance
- Renaming does not change the data itself

---

## Task 17: Alter Table - Change Data Type
Modify the data type of an existing column.

**Scenario:**
A field needs to store a longer value than before.

**Tasks:**
- Change the data type of a relevant column
- Choose a larger or more suitable type
- Verify the change was applied correctly

**What you should understand:**
- Data type selection matters from the beginning
- Some schema changes require updates to existing design

---

## Task 18: Understand DELETE vs TRUNCATE
Create a backup/test table and compare operations.

**Tasks:**
- Create a copy of one table for testing
- Remove selected records using DELETE
- Create another copy and clear all records using TRUNCATE
- Compare the effects on the table and the data

**What you should understand:**
- DELETE is row-based and conditional
- TRUNCATE removes all rows quickly
- The table structure remains in both cases

---

## Task 19: Understand DROP
Remove an entire table that is no longer needed.

**Tasks:**
- Create a test table
- Insert sample data
- Drop the table
- Try to access it later and note the result

**What you should understand:**
- DROP removes the entire table object
- A dropped table is no longer available
- Always make backups before dropping tables

---

## Task 20: Create a Temporary Table
Use a temporary table for testing or intermediate work.

**Scenario:**
You need to create a temporary list of products for review before finalizing changes.

**Tasks:**
- Create a temporary table
- Insert a few records
- Query the table
- Note how it behaves compared to normal tables

**What you should understand:**
- Temporary tables are useful for intermediate processing
- They help avoid changing permanent tables during testing

---

## Task 21: Clone a Table
Create a backup or clone of a table before making changes.

**Tasks:**
- Create a copy of one of your tables
- Use a meaningful backup table name
- Verify both tables contain the same data
- Make schema changes on one table while keeping the other unchanged

**What you should understand:**
- Cloning helps test changes safely
- Backups reduce the risk of accidental loss
- Testing should happen before production changes

---

## Task 22: Final Practice Review
After completing all tasks, write a short summary explaining:

- Which tables you created
- Which constraints you used
- Which records were inserted or changed
- Which columns were modified
- Which operations were destructive
- Which backup/testing methods you used
- What you learned about SQL safety and design

---

## Reflection Questions
Answer the following in your own words:

1. Why is a primary key important?
2. What is the purpose of a foreign key?
3. Why should you use NOT NULL where required?
4. What is the difference between DELETE and TRUNCATE?
5. What is the difference between DELETE and DROP?
6. Why do we use constraints?
7. How do defaults reduce manual errors?
8. Why should you test schema changes on a clone first?
9. Why is UPDATE dangerous if you forget WHERE?
10. Why are temporary tables useful in practice?

---

## Summary
This practical work helps you understand how databases are created, structured, updated, and maintained in real-world systems. It covers all key topics in this module: creating tables, using constraints, inserting records, updating data, deleting records, altering structures, and managing table lifecycle safely.
