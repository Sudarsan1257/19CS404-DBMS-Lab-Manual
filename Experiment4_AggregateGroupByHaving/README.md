# Experiment 4: Aggregate Functions, Group By and Having Clause

## AIM
To study and implement aggregate functions, GROUP BY, and HAVING clause with suitable examples.

## THEORY

### Aggregate Functions
These perform calculations on a set of values and return a single value.

- **MIN()** – Smallest value  
- **MAX()** – Largest value  
- **COUNT()** – Number of rows  
- **SUM()** – Total of values  
- **AVG()** – Average of values

**Syntax:**
```sql
SELECT AGG_FUNC(column_name) FROM table_name WHERE condition;
```
### GROUP BY
Groups records with the same values in specified columns.
**Syntax:**
```sql
SELECT column_name, AGG_FUNC(column_name)
FROM table_name
GROUP BY column_name;
```
### HAVING
Filters the grouped records based on aggregate conditions.
**Syntax:**
```sql
SELECT column_name, AGG_FUNC(column_name)
FROM table_name
GROUP BY column_name
HAVING condition;
```

**Question 1**

How many prescriptions were written for each medication?

Sample table:Prescriptions Table

<img width="1141" height="166" alt="image" src="https://github.com/user-attachments/assets/886173ac-c0ae-436b-a28c-5be728765507" />

```sql
select Medication,
count(*) as TotalPrescriptions
from Prescriptions
group by Medication;
```

**Output:**

<img width="777" height="793" alt="image" src="https://github.com/user-attachments/assets/da7336e9-20dc-4566-80f3-a95986b97cd2" />


**Question 2**

How many patients are covered by each insurance company?

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

<img width="736" height="724" alt="image" src="https://github.com/user-attachments/assets/f4366a0a-7c4d-4fa7-bb2a-de2e0f36dc12" />


**Question 3**

What is the total number of appointments scheduled for each day?

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

<img width="777" height="721" alt="image" src="https://github.com/user-attachments/assets/31e521c7-f987-49b4-a0e5-18ce70f6d501" />


**Question 4**

Write a SQL query to return the total number of rows in the 'customer' table where the city is not Noida.

Sample table: customer

<img width="802" height="167" alt="image" src="https://github.com/user-attachments/assets/58a8187c-c6d3-4429-a482-fcde6447bd18" />


```sql
select count(*) as COUNT
from customer
where city!='Noida';
```

**Output:**

<img width="535" height="401" alt="image" src="https://github.com/user-attachments/assets/ff1448f1-73c3-443b-98fd-9c36f8d023c8" />


**Question 5**

Write a SQL query to calculate the average purchase amount of all orders. Return average purchase amount.

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

<img width="542" height="412" alt="image" src="https://github.com/user-attachments/assets/97f8502c-2fe6-454d-9270-6607e6d30faf" />


**Question 6**

Write a SQL query to return the total number of rows in the 'customer' table where the city is Noida.

Sample table: customer

<img width="866" height="186" alt="image" src="https://github.com/user-attachments/assets/f1ae2862-7b1b-4fea-9f0d-cab6b9d46448" />


```sql
select count(*) as COUNT
from customer
where city='Noida';
```

**Output:**

<img width="519" height="421" alt="image" src="https://github.com/user-attachments/assets/ef7de220-4b15-4690-9c78-bdcba9fde38c" />


**Question 7**
Write a SQL query to find the number of employees who are having the same age removing the duplicate values.

Sample table: employee

```
id   name  age   address     salary
1    Paul  32   California   20000
4    Mark  25    Richtown    65000
5   David  27     Texas      85000
```

```sql
select count(distinct age) as COUNT
from employee;
```

**Output:**

<img width="576" height="416" alt="image" src="https://github.com/user-attachments/assets/4c17bc14-e507-447e-9193-da7771c3c4b7" />


**Question 8**

Write the SQL query to find how many patients have more than 3 medical records?.

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

<img width="781" height="425" alt="image" src="https://github.com/user-attachments/assets/a4308ee3-553e-4ea9-bc9a-293d87330e47" />


**Question 9**

Write the SQL query that accomplishes the grouping of data by joining date (jdate), calculates the total work hours for each date, and excludes dates where the total work hour sum is not greater than 40.

Sample table: employee1

<img width="802" height="163" alt="image" src="https://github.com/user-attachments/assets/d774a3e9-d90b-4d21-935b-374bda544cd3" />


```sql
select jdate,SUM(workhour) 
from employee1
group by jdate
having sum(workhour) >40;
```

**Output:**

<img width="730" height="473" alt="image" src="https://github.com/user-attachments/assets/4eacd53e-fb9a-46bd-b227-e3c51c478a73" />


**Question 10**

Write an SQL query that groups the customer data into 5-year age intervals, calculates the minimum salary for each group, and excludes groups where the minimum salary is not less than 2000.

Table: customer1

<img width="809" height="146" alt="image" src="https://github.com/user-attachments/assets/c8709201-33ca-42be-ba2c-3fab9dfb43ff" />


```sql
select age-(age%5) as age_group,
MIN(salary)
from customer1
group by age-(age%5)
having min(salary)<2000
```

**Output:**

<img width="691" height="438" alt="image" src="https://github.com/user-attachments/assets/5ebb1a08-bce8-462d-90ed-92e0639315ef" />

# RESULT
Thus, the SQL queries to implement aggregate functions, GROUP BY, and HAVING clause have been executed successfully.
