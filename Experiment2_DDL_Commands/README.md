# Experiment 2: DDL Commands

## AIM
To study and implement DDL commands and different types of constraints.

## THEORY

### 1. CREATE
Used to create a new relation (table).

**Syntax:**
```sql
CREATE TABLE (
  field_1 data_type(size),
  field_2 data_type(size),
  ...
);
```
### 2. ALTER
Used to add, modify, drop, or rename fields in an existing relation.
(a) ADD
```sql
ALTER TABLE std ADD (Address CHAR(10));
```
(b) MODIFY
```sql
ALTER TABLE relation_name MODIFY (field_1 new_data_type(size));
```
(c) DROP
```sql
ALTER TABLE relation_name DROP COLUMN field_name;
```
(d) RENAME
```sql
ALTER TABLE relation_name RENAME COLUMN old_field_name TO new_field_name;
```
### 3. DROP TABLE
Used to permanently delete the structure and data of a table.
```sql
DROP TABLE relation_name;
```
### 4. RENAME
Used to rename an existing database object.
```sql
RENAME TABLE old_relation_name TO new_relation_name;
```
### CONSTRAINTS
Constraints are used to specify rules for the data in a table. If there is any violation between the constraint and the data action, the action is aborted by the constraint. It can be specified when the table is created (using CREATE TABLE) or after it is created (using ALTER TABLE).
### 1. NOT NULL
When a column is defined as NOT NULL, it becomes mandatory to enter a value in that column.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) NOT NULL
);
```
### 2. UNIQUE
Ensures that values in a column are unique.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) UNIQUE
);
```
### 3. CHECK
Specifies a condition that each row must satisfy.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) CHECK (logical_expression)
);
```
### 4. PRIMARY KEY
Used to uniquely identify each record in a table.
Properties:
Must contain unique values.
Cannot be null.
Should contain minimal fields.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size) PRIMARY KEY
);
```
### 5. FOREIGN KEY
Used to reference the primary key of another table.
Syntax:
```sql
CREATE TABLE Table_Name (
  column_name data_type(size),
  FOREIGN KEY (column_name) REFERENCES other_table(column)
);
```
### 6. DEFAULT
Used to insert a default value into a column if no value is specified.

Syntax:
```sql
CREATE TABLE Table_Name (
  col_name1 data_type,
  col_name2 data_type,
  col_name3 data_type DEFAULT 'default_value'
);
```

**Question 1**
Create a new table named products with the following specifications:
```
product_id as INTEGER and primary key.
product_name as TEXT and not NULL.
list_price as DECIMAL (10, 2) and not NULL.
discount as DECIMAL (10, 2) with a default value of 0 and not NULL.
A CHECK constraint at the table level to ensure:
list_price is greater than or equal to discount
discount is greater than or equal to 0
list_price is greater than or equal to 0
```

```
CREATE TABLE products(
product_id INT PRIMARY KEY, 
product_name TEXT NOT NULL, 
list_price DECIMAL(10,2) NOT NULL,
discount DECIMAL(10,2) 
DEFAULT 0,
CONSTRAINT products
CHECK(
list_price>=discount AND
discount>=0 AND
list_price>=0
)
);
```

**Output:**
<img width="1328" height="219" alt="image" src="https://github.com/user-attachments/assets/68dc0a97-21d1-486f-b3fb-7efc0400221d" />


**Question 2**
Insert a student with RollNo 201, Name David Lee, Gender M, Subject Physics, and MARKS 92 into the Student_details table.

```
INSERT INTO Student_details (RollNo,Name,Gender,Subject,MARKS)
VALUES (201,'David Lee','M','Physics',92);
```

**Output:**
<img width="1017" height="181" alt="image" src="https://github.com/user-attachments/assets/6560a1f4-6918-4b7a-b010-8f02b08d28c1" />


**Question 3**
Write a SQL Query to add an attribute designation in the employee table with the data type VARCHAR(50).
```
ALTER TABLE employee
ADD COLUMN designation varchar(50);
```

**Output:**
<img width="1837" height="371" alt="image" src="https://github.com/user-attachments/assets/79d5f7f4-ca54-4d88-af6d-b4e7f59f754f" />

**Question 4**
Create a table named Invoices with the following constraints:
```
InvoiceID as INTEGER should be the primary key.
InvoiceDate as DATE.
DueDate as DATE should be greater than the InvoiceDate.
Amount as REAL should be greater than 0.
```

```
CREATE TABLE Invoices(
InvoiceID int PRIMARY KEY,
InvoiceDate DATE,
DueDate DATE,
Amount REAL CHECK (Amount>0),
CHECK(DueDate>InvoiceDate)
);

```

**Output:**
<img width="1882" height="368" alt="image" src="https://github.com/user-attachments/assets/9d16f465-2436-4cce-94ca-f40c9f8da0fe" />


**Question 5**
Write a SQL query for adding a new column named "email" with the datatype VARCHAR(100) to the  table "customer" 
```
Sample table: customer

 customer_id |   cust_name    |    city    | grade | salesman_id 
-------------+----------------+------------+-------+-------------
        3002 | Nick Rimando   | New York   |   100 |        5001
        3007 | Brad Davis     | New York   |   200 |        5001
        3005 | Graham Zusi    | California |   200 |        5002
```
```
ALTER TABLE customer
ADD COLUMN email VARCHAR(100);
```
**Output:**
<img width="1470" height="340" alt="image" src="https://github.com/user-attachments/assets/890355a7-815d-4e9d-96d6-3a4e8b12fc55" />


**Question 6**
Insert all customers from Old_customers into Customers

Table attributes are CustomerID, Name, Address, Email

```
INSERT INTO Customers(CustomerID, Name, Address, Email)
SELECT CustomerID, Name, Address, Email FROM Old_customers;
```

**Output:**
<img width="1641" height="386" alt="image" src="https://github.com/user-attachments/assets/ec484436-cdc0-4b42-b0b8-8d6b9c59743e" />


**Question 7**
In the Products table, insert a record where some fields are NULL, another record where all fields are filled without any NULL values, and a third record where some fields are filled, and others are left as NULL.
```
ProductID   Name              Category    Price       Stock
----------  ---------------   ----------  ----------  ----------
106         Fitness Tracker   Wearables
107         Laptop            Electronics  999.99      50
108         Wireless Earbuds  Accessories              100
```

```
INSERT INTO Products(ProductID,Name,Category,Price,Stock)
VALUES(106,'Fitness Tracker','Wearables',NULL,NULL),
(107,'Laptop','Electronic',999.99,50),
(108,'Wireless Earbud','Accessorie',NULL,100);
```

**Output:**


**Question 8**
Create a table named Employees with the following constraints:
```
EmployeeID should be the primary key.
FirstName and LastName should be NOT NULL.
Email should be unique.
Salary should be greater than 0.
DepartmentID should be a foreign key referencing the Departments table.
```

```
CREATE TABLE Employees(
EmployeeID INT PRIMARY KEY,
FirstName TEXT NOT NULL, 
LastName TEXT NOT NULL,
Email TEXT UNIQUE,
Salary INT CHECK(Salary>0),
DepartmentID INT,
FOREIGN KEY(DepartmentID) REFERENCES Departments);
```

**Output:**
<img width="1607" height="394" alt="image" src="https://github.com/user-attachments/assets/cb3422d4-814a-4e1b-8938-eeb842ba6f05" />

**Question 9**
Create a table named Departments with the following columns:
```
DepartmentID as INTEGER
DepartmentName as TEXT
```
```
CREATE TABLE Departments(
DepartmentID  INTEGER,
DepartmentName  TEXT);
```

**Output:**
![Uploading image.png…]()


**Question 10**
Create a new table named item with the following specifications and constraints:
```
item_id as TEXT and as primary key.
item_desc as TEXT.
rate as INTEGER.
icom_id as TEXT with a length of 4.
icom_id is a foreign key referencing com_id in the company table.
The foreign key should cascade updates and deletes.
item_desc and rate should not accept NULL.
```

```
CREATE TABLE item(
item_id TEXT PRIMARY KEY,
item_desc TEXT NOT NULL,
rate INTEGER NOT NULL,
icom_id TEXT CHECK (length(icom_id)=4),
FOREIGN KEY(icom_id) REFERENCES company(com_id) ON UPDATE CASCADE
ON DELETE CASCADE);
```

**Output:**
![Uploading image.png…]()




## RESULT
Thus, the SQL queries to implement different types of constraints and DDL commands have been executed successfully.
