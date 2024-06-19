# Database-practice-SQL
This is the task that given by Claysys for improving SQL database management system.

2. To create a database and tables with minimum 10 field requirements for your own project, and
then implement insert, update, select and delete operations.
//Creating a database
Creating a database  CREATE DATABASE CLAYSYS;

//Creating a table
claysys=# CREATE TABLE EMPLOYEES(
claysys(# USERID INT PRIMARY KEY,
claysys(# FIRSTNAME VARCHAR(50) NOT NULL,
claysys(# LASTNAME VARCHAR(50) NOT NULL,
claysys(# DATEOFBIRTH DATE,
claysys(# EMAIL VARCHAR(50) UNIQUE NOT NULL,
claysys(# ADDRESS VARCHAR(250),
claysys(# USERNAME VARCHAR(50) UNIQUE NOT NULL,
claysys(# PASSWORD VARCHAR (50) NOT NULL,
claysys(# GENDER VARCHAR(10) CHECK (GENDER IN('MALE','FEMALE','OTHERS')));

//Adding data in to the table
claysys=# INSERT INTO EMPLOYEES (USERID,FIRSTNAME,LASTNAME,DATEOFBIRTH,EMAIL,ADDRESS,USERNAME,PASSWORD,GENDER) VALUES
claysys-# (9,'RINTO','KP','2001-11-22','rinto@gmail.com','kannur','rinto','Rinto12345','MALE'),
claysys-# (10,'SHAHUL','KP','1999-11-22','shahul@gmail.com','kottayam','shahul','Shahul12345','MALE');
INSERT 0 2

//Update table 
claysys=# UPDATE EMPLOYEES SET ADDRESS='changnaserri' WHERE FIRSTNAME='SONA';

//Select table
claysys=# SELECT * FROM EMPLOYEES WHERE FIRSTNAME='ARJUN';

//Delete a table 
claysys=# DELETE FROM EMPLOYEES WHERE FIRSTNAME='SONA';

3. To create an employee table and how to achieve or get the second highest salary from the table.
   claysys=# SELECT DISTINCT SALARY FROM EMPLOYEES ORDER BY SALARY DESC LIMIT 1 OFFSET 1;
   
5. To perform the SQL statement for to lists the number of employees in each department.
  claysys=# SELECT DEPARTMENT,COUNT(USERID) AS EMPLOYEES_TOTAL FROM EMPLOYEES GROUP BY DEPARTMENT;

5. To create two tables and implementing all the SQL joins concepts.
//INNER JOIN
Give matching values in both tables.
claysys=# SELECT employees.firstname, employees.lastname, departments.department_name
FROM employees
INNER JOIN departments ON employees.department_id = departments.department_id;
