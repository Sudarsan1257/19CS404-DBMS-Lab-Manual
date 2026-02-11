# Experiment 3: DML Commands

## AIM
To study and implement DML (Data Manipulation Language) commands.

## THEORY

### 1. INSERT INTO
Used to add records into a relation.
These are three type of INSERT INTO queries which are as
A)Inserting a single record
**Syntax (Single Row):**
```sql
INSERT INTO table_name (field_1, field_2, ...) VALUES (value_1, value_2, ...);
```
**Syntax (Multiple Rows):**
```sql
INSERT INTO table_name (field_1, field_2, ...) VALUES
(value_1, value_2, ...),
(value_3, value_4, ...);
```
**Syntax (Insert from another table):**
```sql
INSERT INTO table_name SELECT * FROM other_table WHERE condition;
```
### 2. UPDATE
Used to modify records in a relation.
Syntax:
```sql
UPDATE table_name SET column1 = value1, column2 = value2 WHERE condition;
```
### 3. DELETE
Used to delete records from a relation.
**Syntax (All rows):**
```sql
DELETE FROM table_name;
```
**Syntax (Specific condition):**
```sql
DELETE FROM table_name WHERE condition;
```
### 4. SELECT
Used to retrieve records from a table.
**Syntax:**
```sql
SELECT column1, column2 FROM table_name WHERE condition;
```
**Question 1**

### How many prescriptions were written for each medication?

<img width="1143" height="169" alt="image" src="https://github.com/user-attachments/assets/a6131c96-6b89-45fa-8b04-bd4452353273" />

```sql
select Medication,
count(*) as TotalPrescriptions
from Prescriptions
group by Medication;
```

**Output:**

<img width="919" height="813" alt="image" src="https://github.com/user-attachments/assets/c346d22a-31f2-41ea-8054-39e913a3914e" />


**Question 2**
### How many patients are covered by each insurance company?

Sample table:Insurance Table
```
name               type
-----------------  ----------
InsuranceID        INTEGER
PatientID          INTEGER
InsuranceCompany   TEXT
PolicyNumber       TEXT
PolicyHolder       TEXT
ValidityPeriod     TEXT
```
```sql
select InsuranceCompany,
count(*) as TotalPatients
from Insurance 
group by InsuranceCompany;
```

**Output:**

<img width="987" height="716" alt="image" src="https://github.com/user-attachments/assets/abe17926-1846-448a-940f-13803b746453" />


**Question 3**
### What is the total number of appointments scheduled for each day?


Table: Appointments
```
name                 type
-------------------  ----------
AppointmentID        INTEGER
PatientID            INTEGER
DoctorID             INTEGER
AppointmentDateTime  DATETIME
Purpose              TEXT
Status               TEXT
```
```sql
select date(AppointmentDateTime) as AppointmentDate,
count(*) as TotalAppointments
from Appointments
group by AppointmentDateTime;
```

**Output:**

<img width="889" height="779" alt="image" src="https://github.com/user-attachments/assets/f7a418d2-debb-4a66-8ee5-52b6d82684b0" />


**Question 4**
### Write a SQL query to return the total number of rows in the 'customer' table where the city is not Noida.
Sample table: customer
<img width="931" height="197" alt="image" src="https://github.com/user-attachments/assets/774256ff-da07-485f-b62a-7b07bcfcd6c9" />

```sql
select count(*) as COUNT
from customer
where city!='Noida';
```

**Output:**

<img width="596" height="428" alt="image" src="https://github.com/user-attachments/assets/dd83c34b-856c-40e8-87fe-c93b35489e9d" />


**Question 5**
### Write a SQL query to calculate the average purchase amount of all orders. Return average purchase amount.

Sample table: orders
```
ord_no      purch_amt   ord_date    customer_id  salesman_id

----------  ----------  ----------  -----------  -----------

70001       150.5       2012-10-05  3005         5002

70009       270.65      2012-09-10  3001         5005

70002       65.26       2012-10-05  3002         5001
```
```sql
select avg(purch_amt) as AVERAGE
from orders;
```

**Output:**

<img width="524" height="446" alt="image" src="https://github.com/user-attachments/assets/e6a3847d-5969-4eb3-a7ea-2b7781033bd6" />


**Question 6**
### Write a SQL query to return the total number of rows in the 'customer' table where the city is Noida.
Sample table: customer
<img width="905" height="200" alt="image" src="https://github.com/user-attachments/assets/e5453930-7453-4cb6-8a79-eecf7b32eade" />

```sql
select count(*) as COUNT
from customer
where city='Noida';
```

**Output:**

<img width="539" height="425" alt="image" src="https://github.com/user-attachments/assets/9a0ee281-fa51-4b82-90cd-8be7222a2b97" />


**Question 7**
### Write a SQL query to find the number of employees who are having the same age removing the duplicate values.

Sample table: employee
```

id name age address      salary
1 Paul  32  California   20000
4 Mark  25  Richtown     65000
5 David 27  Texas        85000
```
```sql
select count(distinct age) as COUNT
from employee;
```

**Output:**

<img width="539" height="420" alt="image" src="https://github.com/user-attachments/assets/83da7e6b-6e1f-46cc-8350-cefda15bb340" />


**Question 8**
### Write the SQL query to find how many patients have more than 3 medical records?.

Sample table: MedicalRecords
```
name        type
----------  ----------
RecordID    INTEGER
PatientID   INTEGER
DoctorID    INTEGER
Date        DATE
Diagnosis   TEXT
Treatment   TEXT
Medication  TEXT
```
```sql
select PatientID,count(*) as TotalRecords
from MedicalRecords
group by PatientID
having count(*) >3;
```

**Output:**

<img width="791" height="448" alt="image" src="https://github.com/user-attachments/assets/c74e1d9c-72a7-428c-b59b-16b28e90a22d" />


**Question 9**
### Write the SQL query that accomplishes the grouping of data by joining date (jdate), calculates the total work hours for each date, and excludes dates where the total work hour sum is not greater than 40.

Sample table: employee1
<img width="846" height="180" alt="image" src="https://github.com/user-attachments/assets/bfc56ced-7cc0-479c-91a9-5f39738acd58" />

```sql
select jdate,SUM(workhour) 
from employee1
group by jdate
having sum(workhour) >40;
```

**Output:**

<img width="745" height="499" alt="image" src="https://github.com/user-attachments/assets/b4fe611b-0fd1-43f3-97f4-19753e3c0ab4" />


**Question 10**
### Write an SQL query that groups the customer data into 5-year age intervals, calculates the minimum salary for each group, and excludes groups where the minimum salary is not less than 2000.

Table: customer1

<img width="837" height="148" alt="image" src="https://github.com/user-attachments/assets/99731af9-29f6-4d05-87fd-7ff405287d6b" />


```sql
select age-(age%5) as age_group,
MIN(salary)
from customer1
group by age-(age%5)
having min(salary)<2000
```

**Output:**

<img width="709" height="454" alt="image" src="https://github.com/user-attachments/assets/f0240cc6-c11b-4679-9f42-e596f9dfc24c" />


## RESULT
Thus, the SQL queries to implement DML commands have been executed successfully.
