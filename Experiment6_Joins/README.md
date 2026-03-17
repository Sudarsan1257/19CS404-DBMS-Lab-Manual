# Experiment 6: Joins

## AIM
To study and implement different types of joins.

## THEORY

SQL Joins are used to combine records from two or more tables based on a related column.

### 1. INNER JOIN
Returns records with matching values in both tables.

**Syntax:**
```sql
SELECT columns
FROM table1
INNER JOIN table2
ON table1.column = table2.column;
```

### 2. LEFT JOIN
Returns all records from the left table, and matched records from the right.

**Syntax:**

```sql
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.column = table2.column;
```
### 3. RIGHT JOIN
Returns all records from the right table, and matched records from the left.

**Syntax:**

```sql
SELECT columns
FROM table1
RIGHT JOIN table2
ON table1.column = table2.column;
```
### 4. FULL OUTER JOIN
Returns all records when there is a match in either left or right table.

**Syntax:**

```sql
SELECT columns
FROM table1
FULL OUTER JOIN table2
ON table1.column = table2.column;
```

**Question 1**
```
From the following tables write a SQL query to find those customers with a grade less than 300. Return cust_name, customer city, grade, Salesman, salesmancity. The result should be ordered by ascending customer_id. 

Sample table: customer

 customer_id |   cust_name    |    city    | grade | salesman_id 
-------------+----------------+------------+-------+-------------
        3002 | Nick Rimando   | New York   |   100 |        5001
        3007 | Brad Davis     | New York   |   200 |        5001
        3005 | Graham Zusi    | California |   200 |        5002
        3008 | Julian Green   | London     |   300 |        5002
        3004 | Fabian Johnson | Paris      |   300 |        5006
        3009 | Geoff Cameron  | Berlin     |   100 |        5003
        3003 | Jozy Altidor   | Moscow     |   200 |        5007
        3001 | Brad Guzan     | London     |       |        5005
Sample table: salesman

 salesman_id |    name    |   city   | commission 
-------------+------------+----------+------------
        5001 | James Hoog | New York |       0.15
        5002 | Nail Knite | Paris    |       0.13
        5005 | Pit Alex   | London   |       0.11
        5006 | Mc Lyon    | Paris    |       0.14
        5007 | Paul Adam  | Rome     |       0.13
        5003 | Lauson Hen | San Jose |       0.12
```

```sql
select c.cust_name,c.city,c.grade,s.name as Salesman,s.city as city
from customer c
join salesman s
on s.salesman_id=c.salesman_id
where c.grade<300
order by customer_id;
```

**Output:**

<img width="1264" height="819" alt="image" src="https://github.com/user-attachments/assets/e643cae3-4541-45d3-9593-760e30366ebb" />


**Question 2**
```
Write an SQL query to select all columns from the 'customer' table (aliased as 'c') by performing a LEFT JOIN with the 'orders' table on the 'customer_id' column, including only those orders where the order date falls between '2012-08-01' and '2012-08-30'.

'customer' Table: (customer_id, cust_name, city, grade, salesman_id)

'orders' Table: (ord_no, purch_amt, ord_date, customer_id, salesman_id)
```

```sql
select c.*
from customer c
left join orders o
on c.customer_id=o.customer_id
where o.ord_date between '2012-08-01' and '2012-08-30'
;
```

**Output:**

<img width="1292" height="571" alt="image" src="https://github.com/user-attachments/assets/98a790c5-0525-4748-8653-2b61d1fb2227" />


**Question 3**
```
Write the SQL query that achieves the selection of the "name" column from the "salesman" table (aliased as "s"), with a left join on the "salesman_id" column and a condition filtering for customers in the city 'London'.

Customer Table: (customer_id, cust_name, city, grade, salesman_id)

Salesman Table: (salesman_id, name, city, commission)
```

```sql
select s.name 
from salesman s
left join customer c
on c.salesman_id=s.salesman_id
where c.city='London';
```

**Output:**

<img width="717" height="565" alt="image" src="https://github.com/user-attachments/assets/4e55173c-9041-4f72-8539-5d645d3ab175" />


**Question 4**
```
Write the SQL query that achieves the selection of all columns from the "patients" table (aliased as "p"), with an inner join on the "patient_id" column and a condition filtering for appointments with an appointment date between '2024-01-01' and '2024-01-31'.

PATIENTS TABLE:

ATTRIBUTES - patient_id, first_name, last_name, date_of_birth, admission_date, discharge_date, doctor_id
```
<img width="752" height="126" alt="image" src="https://github.com/user-attachments/assets/230e3945-ce2e-4991-9c08-1a25e0d34c55" />
```
APPOINTMENTS TABLE:

ATTRIBUTES - appointment_id, patient_id, doctor_id, appointment_date
```
<img width="754" height="125" alt="image" src="https://github.com/user-attachments/assets/2938638e-cc92-48db-98d9-00d3da2b2033" />

```sql
select p.*
from patients p
inner join appointments a
on p.patient_id=a.patient_id
where a.appointment_date between '2024-01-01' and '2024-01-31';
```

**Output:**


<img width="1250" height="494" alt="image" src="https://github.com/user-attachments/assets/4638c825-d10a-4517-8e35-7fc0d4e858f1" />


**Question 5**
```
From the following tables write a SQL query to locate those salespeople who do not live in the same city where their customers live and have received a commission of more than 12% from the company. Return Customer Name, customer city, Salesman, salesman city, commission.  

Sample table: customer

 customer_id |   cust_name    |    city    | grade | salesman_id 
-------------+----------------+------------+-------+-------------
        3002 | Nick Rimando   | New York   |   100 |        5001
        3007 | Brad Davis     | New York   |   200 |        5001
        3005 | Graham Zusi    | California |   200 |        5002
        3008 | Julian Green   | London     |   300 |        5002
        3004 | Fabian Johnson | Paris      |   300 |        5006
        3009 | Geoff Cameron  | Berlin     |   100 |        5003
        3003 | Jozy Altidor   | Moscow     |   200 |        5007
        3001 | Brad Guzan     | London     |       |        5005
Sample table: salesman

 salesman_id |    name    |   city   | commission 
-------------+------------+----------+------------
        5001 | James Hoog | New York |       0.15
        5002 | Nail Knite | Paris    |       0.13
        5005 | Pit Alex   | London   |       0.11
        5006 | Mc Lyon    | Paris    |       0.14
        5007 | Paul Adam  | Rome     |       0.13
        5003 | Lauson Hen | San Jose |       0.12
```

```sql
select c.cust_name as 'Customer Name',c.city,s.name as Salesman,s.city,s.commission
from customer c
left join salesman s
on c.salesman_id=s.salesman_id
where s.city<>c.city and s.commission>0.12;
```

**Output:**


<img width="1303" height="709" alt="image" src="https://github.com/user-attachments/assets/f4b734fd-12e3-446c-891f-542bebf6118c" />


**Question 6**
```
Write the SQL query that achieves the selection of all columns from the "patients" table (aliased as "p"), with an inner join on the "patient_id" column and a condition filtering for appointments with an appointment date between '2024-02-01' and '2024-02-28'.

PATIENTS TABLE:

ATTRIBUTES - patient_id, first_name, last_name, date_of_birth, admission_date, discharge_date, doctor_id
```
<img width="757" height="125" alt="image" src="https://github.com/user-attachments/assets/66683b00-eb58-4be3-a1d4-5f6b374fbe3d" />
```
APPOINTMENTS TABLE:

ATTRIBUTES - appointment_id, patient_id, doctor_id, appointment_date
```

<img width="771" height="134" alt="image" src="https://github.com/user-attachments/assets/1148692c-e624-4c28-bf68-b84e1f113310" />

```sql
SELECT p.*
FROM patients p
INNER JOIN appointments a
    ON p.patient_id = a.patient_id
WHERE a.appointment_date 
      BETWEEN '2024-02-01' AND '2024-02-28';
```

**Output:**


<img width="1284" height="503" alt="image" src="https://github.com/user-attachments/assets/8ce92295-88b1-4760-aa04-c37daaee4466" />


**Question 7**
```
Write a SQL statement to join the tables salesman, customer and orders so that the same column of each table appears once and only the relational rows are returned. 

Sample table: orders

ord_no      purch_amt   ord_date    customer_id  salesman_id
----------  ----------  ----------  -----------  -----------
70001       150.5       2012-10-05  3005         5002
70009       270.65      2012-09-10  3001         5005
70002       65.26       2012-10-05  3002         5001
70004       110.5       2012-08-17  3009         5003
70007       948.5       2012-09-10  3005         5002
70005       2400.6      2012-07-27  3007         5001
70008       5760        2012-09-10  3002         5001
70010       1983.43     2012-10-10  3004         5006
70003       2480.4      2012-10-10  3009         5003
70012       250.45      2012-06-27  3008         5002
70011       75.29       2012-08-17  3003         5007
70013       3045.6      2012-04-25  3002         5001
Sample table: customer

 customer_id |   cust_name    |    city    | grade | salesman_id 
-------------+----------------+------------+-------+-------------
        3002 | Nick Rimando   | New York   |   100 |        5001
        3007 | Brad Davis     | New York   |   200 |        5001
        3005 | Graham Zusi    | California |   200 |        5002
        3008 | Julian Green   | London     |   300 |        5002
        3004 | Fabian Johnson | Paris      |   300 |        5006
        3009 | Geoff Cameron  | Berlin     |   100 |        5003
        3003 | Jozy Altidor   | Moscow     |   200 |        5007
        3001 | Brad Guzan     | London     |       |        5005
Sample table : salesman

 salesman_id |    name    |   city   | commission 
-------------+------------+----------+------------
        5001 | James Hoog | New York |       0.15
        5002 | Nail Knite | Paris    |       0.13
        5005 | Pit Alex   | London   |       0.11
        5006 | Mc Lyon    | Paris    |       0.14
        5007 | Paul Adam  | Rome     |       0.13
        5003 | Lauson Hen | San Jose |       0.12
```

```sql
SELECT 
    o.ord_no,
    o.purch_amt,
    o.ord_date,
    c.cust_name,
    c.city AS customer_city,
    c.grade,
    s.name AS salesman_name,
    s.city AS salesman_city,
    s.commission
FROM orders o
INNER JOIN customer c
        ON o.customer_id = c.customer_id
INNER JOIN salesman s
        ON o.salesman_id = s.salesman_id;
```

**Output:**


<img width="1381" height="781" alt="image" src="https://github.com/user-attachments/assets/a9597116-e7f2-4919-a21b-26eb37dcb029" />


**Question 8**
```
 From the following tables write a SQL query to find the salesperson(s) and the customer(s) he represents. Return Customer Name, city, Salesman, commission.

Sample table: customer

 customer_id |   cust_name    |    city    | grade | salesman_id 
-------------+----------------+------------+-------+-------------
        3002 | Nick Rimando   | New York   |   100 |        5001
        3007 | Brad Davis     | New York   |   200 |        5001
        3005 | Graham Zusi    | California |   200 |        5002
        3008 | Julian Green   | London     |   300 |        5002
        3004 | Fabian Johnson | Paris      |   300 |        5006
        3009 | Geoff Cameron  | Berlin     |   100 |        5003
        3003 | Jozy Altidor   | Moscow     |   200 |        5007
        3001 | Brad Guzan     | London     |       |        5005
Sample table: salesman

 salesman_id |    name    |   city   | commission 
-------------+------------+----------+------------
        5001 | James Hoog | New York |       0.15
        5002 | Nail Knite | Paris    |       0.13
        5005 | Pit Alex   | London   |       0.11
        5006 | Mc Lyon    | Paris    |       0.14
        5007 | Paul Adam  | Rome     |       0.13
        5003 | Lauson Hen | San Jose |       0.12
```

```sql
SELECT 
    c.cust_name AS "Customer Name",
    c.city,
    s.name AS "Salesman",
    s.commission
FROM customer c
INNER JOIN salesman s
        ON c.salesman_id = s.salesman_id;
```

**Output:**


<img width="1272" height="832" alt="image" src="https://github.com/user-attachments/assets/c4761196-d371-435d-bbdd-740e7bf28e05" />


**Question 9**
```
Write the SQL query that achieves the selection of all columns from the "nurses" table (aliased as "n"), with an inner join on the "department_id" column and a condition filtering for nurses in the 'Pediatrics' department.

NURSES TABLE:

ATTRIBUTES - nurse_id, first_name, last_name, department_id
```

<img width="700" height="119" alt="image" src="https://github.com/user-attachments/assets/7a479188-9bd4-4437-ae33-d3d78165b74d" />
```
DEPARTMENTS TABLE:

ATTRIBUTES - department_id, department_name
```

<img width="702" height="125" alt="image" src="https://github.com/user-attachments/assets/bdb52b0f-fdcd-4a06-8d3a-836af468cf41" />

```sql
SELECT n.*
FROM nurses n
INNER JOIN departments d
    ON n.department_id = d.department_id
WHERE d.department_name = 'Pediatrics';
```

**Output:**

<img width="1278" height="458" alt="image" src="https://github.com/user-attachments/assets/ce388153-7eed-4d92-98ef-4b882b90c797" />


**Question 10**
```
 From the following tables write a SQL query to find salespeople who received commissions of more than 12 percent from the company. Return Customer Name, customer city, Salesman, commission.  

Sample table: customer

 customer_id |   cust_name    |    city    | grade | salesman_id 
-------------+----------------+------------+-------+-------------
        3002 | Nick Rimando   | New York   |   100 |        5001
        3007 | Brad Davis     | New York   |   200 |        5001
        3005 | Graham Zusi    | California |   200 |        5002
        3008 | Julian Green   | London     |   300 |        5002
        3004 | Fabian Johnson | Paris      |   300 |        5006
        3009 | Geoff Cameron  | Berlin     |   100 |        5003
        3003 | Jozy Altidor   | Moscow     |   200 |        5007
        3001 | Brad Guzan     | London     |       |        5005
Sample table: salesman

 salesman_id |    name    |   city   | commission 
-------------+------------+----------+------------
        5001 | James Hoog | New York |       0.15
        5002 | Nail Knite | Paris    |       0.13
        5005 | Pit Alex   | London   |       0.11
        5006 | Mc Lyon    | Paris    |       0.14
        5007 | Paul Adam  | Rome     |       0.13
        5003 | Lauson Hen | San Jose |       0.12
```

```sql
SELECT 
    c.cust_name AS "Customer Name",
    c.city,
    s.name AS "Salesman",
    s.commission
FROM customer c
INNER JOIN salesman s
        ON c.salesman_id = s.salesman_id
WHERE s.commission > 0.12;
```

**Output:**


<img width="1293" height="789" alt="image" src="https://github.com/user-attachments/assets/10443a4f-ee07-4347-b44d-fb0a1ab4637b" />



## RESULT
Thus, the SQL queries to implement different types of joins have been executed successfully.
