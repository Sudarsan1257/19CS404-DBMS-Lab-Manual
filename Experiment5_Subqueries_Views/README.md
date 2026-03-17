# Experiment 5: Subqueries and Views

## AIM
To study and implement subqueries and views.

## THEORY

### Subqueries
A subquery is a query inside another SQL query and is embedded in:
- WHERE clause
- HAVING clause
- FROM clause

**Types:**
- **Single-row subquery**:
  Sub queries can also return more than one value. Such results should be made use along with the operators in and any.
- **Multiple-row subquery**:
  Here more than one subquery is used. These multiple sub queries are combined by means of ‘and’ & ‘or’ keywords.
- **Correlated subquery**:
  A subquery is evaluated once for the entire parent statement whereas a correlated Sub query is evaluated once per row processed by the parent statement.

**Example:**
```sql
SELECT * FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```
### Views
A view is a virtual table based on the result of an SQL SELECT query.
**Create View:**
```sql
CREATE VIEW view_name AS
SELECT column1, column2 FROM table_name WHERE condition;
```
**Drop View:**
```sql
DROP VIEW view_name;
```

**Question 1**
```
Write a SQL query that retrieves the names of students and their corresponding grades, where the grade is equal to the maximum grade achieved in each subject.

Sample table: GRADES (attributes: student_id, student_name, subject, grade)
```
<img width="847" height="354" alt="image" src="https://github.com/user-attachments/assets/87b372c5-5626-4878-9bf5-32f1121a6a96" />

```sql
select student_name,grade
from grades g1
where grade=(select max(grade)
from grades g2
where g1.subject=g2.subject);
```

**Output:**

<img width="919" height="626" alt="image" src="https://github.com/user-attachments/assets/d9f5bf74-8cc6-4939-8d5f-d25cc1d9d49f" />

**Question 2**
```
Write a SQL query to retrieve all columns from the CUSTOMERS table for customers whose salary is greater than $4500.

Sample table: CUSTOMERS

ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------

1          Ramesh     32              Ahmedabad     2000
2          Khilan        25              Delhi      1500
3          Kaushik      23              Kota         2000
4          Chaitali       25             Mumbai       6500
5          Hardik        27              Bhopal        8500
6          Komal         22              Hyderabad     4500
7           Muffy          24              Indore      10000


```

```sql
SELECT *
FROM CUSTOMERS
WHERE SALARY > 4500;
```

**Output:**

<img width="1205" height="640" alt="image" src="https://github.com/user-attachments/assets/d1479fc7-2287-4359-82d9-288c98ce4030" />


**Question 3**
```
From the following tables write a SQL query to find salespeople who had more than one customer. Return salesman_id and name.
salesman table

name                 type
---------------   ---------------
salesman_id       numeric(5)
name                  varchar(30)
city                     varchar(15)
commission       decimal(5,2)

customer table

name              type
-----------       ----------
customer_id   int
cust_name     text
city                text
grade            int
salesman_id  int


```

```sql
select s.salesman_id,s.name 
from salesman s
join customer c
on s.salesman_id=c.salesman_id
group by s.salesman_id,s.name
having count(s.salesman_id)>1;
```

**Output:**

<img width="804" height="686" alt="image" src="https://github.com/user-attachments/assets/bbb6d76f-5353-44b4-bec9-1d74dd70260e" />


**Question 4**
```
Write a SQL query to retrieve all columns from the CUSTOMERS table for customers whose Address as Delhi and age below 30

Sample table: CUSTOMERS

ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------

1          Ramesh     32              Ahmedabad     2000
2          Khilan        25              Delhi                 1500
3          Kaushik      23              Kota                  2000
4          Chaitali       25             Mumbai            6500
5          Hardik        27              Bhopal              8500
6          Komal         22              Hyderabad       4500

7           Muffy          24              Indore            10000
```

```sql
SELECT *
FROM CUSTOMERS
WHERE ADDRESS = 'Delhi'
AND AGE < 30
order by id;
```

**Output:**

<img width="1224" height="572" alt="image" src="https://github.com/user-attachments/assets/2a1a2566-89e0-431d-a5ee-54638b87e87e" />


**Question 5**
```
Write a SQL query to retrieve all columns from the CUSTOMERS table for customers whose salary is LESS than $2500.

Sample table: CUSTOMERS

ID          NAME        AGE         ADDRESS     SALARY
----------  ----------  ----------  ----------  ----------

1          Ramesh     32              Ahmedabad     2000
2          Khilan        25              Delhi                 1500
3          Kaushik      23              Kota                  2000
4          Chaitali       25             Mumbai            6500
5          Hardik        27              Bhopal              8500
6          Komal         22              Hyderabad       4500

7           Muffy          24              Indore            10000
```

```sql
select *
from customers 
where salary<2500;
```

**Output:**

<img width="1228" height="674" alt="image" src="https://github.com/user-attachments/assets/9fabac74-9a31-4f8c-a2bc-119346bb331c" />


**Question 6**
```
From the following tables write a SQL query to count the number of customers with grades above the average in New York City. Return grade and count.

customer table

name         type
-----------  ----------
customer_id  int
cust_name    text
city         text
grade        int
salesman_id  int
```

```sql
SELECT grade, COUNT(*)
FROM customer
WHERE grade > (
    SELECT AVG(grade)
    FROM customer
    WHERE city = 'New York'
)
GROUP BY grade;
```

**Output:**

<img width="1080" height="511" alt="image" src="https://github.com/user-attachments/assets/31d5ae65-0825-4174-9f8e-9ac8f815e10c" />


**Question 7**
```
Write a SQL query to Retrieve the medications with dosages equal to the highest dosage

Table Name: Medications (attributes: medication_id, medication_name, dosage)
```
<img width="1101" height="271" alt="image" src="https://github.com/user-attachments/assets/4e81571e-3d93-4664-af8c-315dedbf7e32" />


```sql
SELECT medication_id, medication_name, dosage
FROM Medications
WHERE dosage = (
    SELECT MAX(dosage)
    FROM Medications
);
```

**Output:**

<img width="1109" height="585" alt="image" src="https://github.com/user-attachments/assets/3842e12f-2fb5-493d-9d7f-108d9e54c906" />


**Question 8**
```
Write a SQL query that retrieves the names of students and their corresponding grades, where the grade is equal to the minimum grade achieved in each subject.

Sample table: GRADES (attributes: student_id, student_name, subject, grade)

```
<img width="908" height="353" alt="image" src="https://github.com/user-attachments/assets/201e2048-d639-4b05-bf2f-cb7eee2bc48e" />


```sql
SELECT student_name, grade
FROM GRADES
WHERE (subject, grade) IN (
    SELECT subject, MIN(grade)
    FROM GRADES
    GROUP BY subject
);
```

**Output:**

<img width="958" height="632" alt="image" src="https://github.com/user-attachments/assets/4b5538d1-0467-41cb-ab3f-2e8c950ae7cf" />


**Question 9**
```
Write a SQL query to Retrieve the names and cities of customers who have the same city as customers with IDs 3 and 7

SAMPLE TABLE: customer

name             type
---------------  ---------------
id               INTEGER
name             TEXT
city             TEXT
email            TEXT
phone            INTEGER
```
```sql
SELECT name, city
FROM customer
WHERE city IN (
    SELECT city
    FROM customer
    WHERE id IN (3, 7)
);
```

**Output:**

<img width="935" height="660" alt="image" src="https://github.com/user-attachments/assets/7e5aa644-c865-43e7-b36f-5717a8025c4c" />


**Question 10**
```
Write a SQL query to Identify customers whose city is different from the city of the customer with the highest ID

SAMPLE TABLE: customer

name             type
---------------  ---------------
id               INTEGER
name             TEXT
city             TEXT
email            TEXT
phone            INTEGER
```

```sql
SELECT *
FROM customer
WHERE city != (
    SELECT city
    FROM customer
    ORDER BY id DESC
    LIMIT 1
);
```

**Output:**

<img width="1262" height="715" alt="image" src="https://github.com/user-attachments/assets/67af79f3-fdc6-42ab-9f66-5c8d0e39253f" />


## RESULT
Thus, the SQL queries to implement subqueries and views have been executed successfully.
