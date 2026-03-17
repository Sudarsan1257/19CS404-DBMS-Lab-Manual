# Experiment 10: PL/SQL – Triggers

## AIM
To write and execute PL/SQL trigger programs for automating actions in response to specific table events like INSERT, UPDATE, or DELETE.

---

## THEORY

A **trigger** is a stored PL/SQL block that is automatically executed or fired when a specified event occurs on a table or view. Triggers can be used for enforcing business rules, auditing changes, or automatic updates.

### Types of Triggers:
- **Before Trigger**: Executes before the operation (INSERT, UPDATE, DELETE).
- **After Trigger**: Executes after the operation.
- **Row-level Trigger**: Executes for each affected row.
- **Statement-level Trigger**: Executes once for the triggering statement.

**Basic Syntax:**
```sql
CREATE OR REPLACE TRIGGER trigger_name
BEFORE|AFTER INSERT|UPDATE|DELETE ON table_name
[FOR EACH ROW]
BEGIN
   -- trigger logic
END;
```

## 1. Write a trigger to log every insertion into a table.
**Steps:**
 Create two tables: `employees` (for storing data) and `employee_log` (for logging the inserts).
 Write an - **AFTER INSERT** trigger on the `employees` table to log the new data into the `employee_log` table.
  
  PL/SQL QUERY
  ```sql
create table employees11(
 employee_id INT primary key,
 first_name VARCHAR(50),
 dept_no INT,
 salary DECIMAL(10,2)
 );
 create table employee_log(
   log_id INT AUTO_INCREMENT primary key,
   employee_id INT,
   first_name VARCHAR(50),
   dept_no INT,
   salary DECIMAL(10,2),
   log_timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );
   DELIMITER $$
   create trigger trg_log_employee_insert
   AFTER INSERT ON employees11
   FOR EACH ROW
   BEGIN
      Insert into employee_log(employee_id,first_name,dept_no,salary)
      values(New.employee_id,new.first_name,new.dept_no,new.salary);
  END$$
  DELIMITER ;
  INSERT INTO employees11(employee_id,first_name,dept_no,salary)
  values(1,'Alice',10,5000.00);
  select * from employee_log;
  ```

**Expected Output:**
- A new entry is added to the `employee_log` table each time a new record is inserted into the `employees` table.

OUTPUT:
<img width="816" height="206" alt="447225963-d907c706-332f-4c1c-9ca4-269a75da395d" src="https://github.com/user-attachments/assets/79c6dda5-9006-4b0c-952c-5591649d5492" />

## 2. Write a trigger to prevent deletion of records from a sensitive table.
**Steps:**
- Write a **BEFORE DELETE** trigger on the `sensitive_data` table.
- Use `RAISE_APPLICATION_ERROR` to prevent deletion and issue a custom error message.
PL/SQL Query
```sql
create table sensitive_data(
    record_id INT primary key,
    info varchar(50)
    );
    DELIMITER $$
    create trigger trg_prevent_delete_sensitive
    BEFORE delete on sensitive_data
    FOR EACH ROW
    BEGIN
      SIGNAL SQLSTATE '45000'
      SET MESSAGE_TExT = 'ERROR:DELETION not allowed in this table';
END$$
DELIMITER ;
Insert INTO sensitive_data(record_id,info) values (1,'Top secret');
DELETE FROM sensitive_data where record_id=1;
```
**Expected Output:**
- If an attempt is made to delete a record from `sensitive_data`, an error message is raised, e.g., `ERROR: Deletion not allowed on this table.`
OUTPUT:

<img width="918" height="29" alt="447226026-795638f9-29af-4817-9a93-dc3e6439f988" src="https://github.com/user-attachments/assets/e241a14b-e183-43ae-a238-071667087ec2" />

---

## 3. Write a trigger to automatically update a `last_modified` timestamp.
**Steps:**
- Add a `last_modified` column to the `products` table.
- Write a **BEFORE UPDATE** trigger on the `products` table to set the `last_modified` column to the current timestamp whenever an update occurs.
PL/SQL Query
```sql
CREATE TABLE products 
(
    product_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    price NUMERIC(10, 2),
    stock_quantity INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    last_modified TIMESTAMP
);
DELIMITER $$

    CREATE TRIGGER trg_set_last_modified
    BEFORE UPDATE ON products
    FOR EACH ROW
    BEGIN
        SET NEW.last_modified = CURRENT_TIMESTAMP;
    END$$

DELIMITER ;

INSERT INTO products (name, description, price, stock_quantity)
VALUES 
('Wireless Mouse', 'Ergonomic wireless mouse with USB receiver', 25.99, 100),
('Mechanical Keyboard', 'RGB backlit mechanical keyboard', 59.99, 50),
('HD Monitor', '24-inch Full HD LED monitor', 149.99, 30);

UPDATE products SET price = 27.99 WHERE product_id = 1;
UPDATE products SET name = 'Key Board '  WHERE product_id = 2;
UPDATE products SET stock_quantity = 40 WHERE product_id = 3;

SELECT product_id, name, created_at, last_modified FROM products;
**Expected Output:**
- The `last_modified` column in the `products` table is updated automatically to the current date and time when any record is updated.
```
**Expected Output:**
- The `last_modified` column in the `products` table is updated automatically to the current date and time when any record is updated.

Output

Before updating the record:

<img width="784" height="149" alt="447226087-5d754bb3-d7d2-45dd-b19a-c8892ad11314" src="https://github.com/user-attachments/assets/689d6476-2d00-4d4b-a547-5a11f0703fe8" />

After updating the record:

<img width="817" height="160" alt="447226182-bc6fa0f7-6544-4276-b221-a2875b63f356" src="https://github.com/user-attachments/assets/fb654f77-ee40-4dfe-bce5-93de65a843df" />


---

## 4. Write a trigger to keep track of the number of updates made to a table.
**Steps:**
- Create an `audit_log` table with a counter column.
- Write an **AFTER UPDATE** trigger on the `customer_orders` table to increment the counter in the `audit_log` table every time a record is updated.

PL/SQL Query
```sql
CREATE TABLE customer_orders (
order_id INT PRIMARY KEY,
customer_name VARCHAR(100),
order_amount DECIMAL(10,2)
);
CREATE TABLE audit_log (
id INT PRIMARY KEY,
update_count INT DEFAULT 0
);
INSERT INTO audit_log (id, update_count) VALUES (1,0);
DELIMITER $$
CREATE TRIGGER trg_track_updates
AFTER UPDATE ON customer_orders
FOR EACH ROW
BEGIN
UPDATE audit_log
SET update_count = update_count + 1
WHERE id = 1;
END$$
DELIMITER ;
INSERT INTO customer_orders(order_id,customer_name,order_amount)
values(100,'Doe',150.00);
UPDATE customer_orders
SET order_amount = 200.00
WHERE order_id = 100;
UPDATE customer_orders
SET order_amount = 400.00
WHERE order_id = 100;
SELECT * FROM audit_log;
```
**Expected Output:**
- The `audit_log` table will maintain a count of how many updates have been made to the `customer_orders` table.

OUTPUT

<img width="819" height="151" alt="447226212-09828baa-9bf8-48e5-adeb-7706bc02bfaa" src="https://github.com/user-attachments/assets/78d3349a-3939-4824-8b23-a735f62cc581" />

---

##5. Write a trigger that checks a condition before allowing insertion into a table.
**Steps:**
- Write a **BEFORE INSERT** trigger on the `employees` table to check if the inserted salary meets a specific condition (e.g., salary must be greater than 3000).
- If the condition is not met, raise an error to prevent the insert.

PL/SQL Query

```sql
CREATE TABLE employee6 
(
    employee_id INT PRIMARY KEY,
    first_name VARCHAR(50),
    dept_no INT,
    salary DECIMAL(10,2)
);

DELIMITER $$

        CREATE TRIGGER trg_check_salary_before_insert2
        BEFORE INSERT ON employee6
        FOR EACH ROW
        BEGIN
            IF NEW.salary < 3000 THEN
                SIGNAL SQLSTATE '45000'
                SET MESSAGE_TEXT = 'ERROR: Salary Below Minimum Threshold.';
            END IF;
        END$$

DELIMITER ;

INSERT INTO employee6(employee_id, first_name, dept_no, salary) VALUES (1, 'Bob', 20, 299.00);
```
**Expected Output:**
- If the inserted salary in the `employees` table is below the condition (e.g., salary < 3000), the insert operation is blocked, and an error message is raised, such as: `ERROR: Salary below minimum threshold.`

OUTPUT

If the inserted salary in the employees table is below the condition (e.g., salary < 3000), the insert operation is blocked, and an error message is raised, such as: ERROR: Salary below minimum threshold.


<img width="822" height="85" alt="447226339-54228d51-298e-4d68-ad9c-b86fd8acbac0" src="https://github.com/user-attachments/assets/737c5483-d7b2-403a-a19b-e69bbdd86038" />

## RESULT
Thus, the PL/SQL trigger programs were written and executed successfully.
