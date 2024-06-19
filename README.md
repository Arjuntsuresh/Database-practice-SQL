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

//LEFT JOIN
Give all the matches from the left table and matched records from the right table.
claysys=# SELECT employees.firstname, employees.lastname, departments.department_name
FROM employees
LEFT JOIN departments ON employees.department_id = departments.department_id;

//RIGHT JOIN
Give all the matches from the right table and matched records from the left table.
claysys=# SELECT employees.firstname, employees.lastname, departments.department_name
FROM employees
RIGHT JOIN departments ON employees.department_id = departments.department_id;

//FULL JOIN
Give the all matches when there is either a match in the left table or the right table.
claysys=# SELECT employees.firstname, employees.lastname, departments.department_name
FROM employees
FULL JOIN departments ON employees.department_id = departments.department_id;

7. To implement a CRUD operations with different stored procedure method for your given project.

//CREATE TABLE USING STORED PROCEDURE
CREATE OR REPLACE FUNCTION create_employee(
    p_firstname VARCHAR(50),
    p_lastname VARCHAR(50),
    p_dateofbirth DATE,
    p_email VARCHAR(100),
    p_address VARCHAR(100),
    p_username VARCHAR(50),
    p_password VARCHAR(50),
    p_gender VARCHAR(10),
    p_salary NUMERIC,
    p_department_id INT
)
RETURNS INTEGER AS $$
DECLARE
    new_id INTEGER;
BEGIN
    INSERT INTO employees (firstname, lastname, dateofbirth, email, address, username, password, gender, salary, department_id)
    VALUES (p_firstname, p_lastname, p_dateofbirth, p_email, p_address, p_username, p_password, p_gender, p_salary, p_department_id)
    RETURNING employee_id INTO new_id;

 RETURN new_id;
END;
$$ LANGUAGE plpgsql;


//CALLING THIS FUNCTION 
SELECT create_employee('John', 'Doe', '1990-05-15', 'john.doe@example.com', '123 Main St', 'johndoe', 'password123', 'Male', 50000, 1);

//READING TABLE USING STORED PROCEDURE
CREATE OR REPLACE FUNCTION get_employee_by_id(p_employee_id INTEGER)
RETURNS TABLE (
    employee_id INTEGER,
    firstname VARCHAR(50),
    lastname VARCHAR(50),
    dateofbirth DATE,
    email VARCHAR(100),
    address VARCHAR(100),
    username VARCHAR(50),
    gender VARCHAR(10),
    salary NUMERIC,
    department_name VARCHAR(100)
) AS $$
BEGIN
    RETURN QUERY
    SELECT e.employee_id, e.firstname, e.lastname, e.dateofbirth, e.email, e.address,
           e.username, e.gender, e.salary, d.department_name
    FROM employees e
    INNER JOIN departments d ON e.department_id = d.department_id
    WHERE e.employee_id = p_employee_id;
END;
$$ LANGUAGE plpgsql;

//CALLING THIS FUNCTION 
SELECT * FROM get_employee_by_id(1);

//UPDATING TABLE USING STORED PROCEDURE;
CREATE OR REPLACE FUNCTION update_employee(
    p_employee_id INTEGER,
    p_firstname VARCHAR(50),
    p_lastname VARCHAR(50),
    p_dateofbirth DATE,
    p_email VARCHAR(100),
    p_address VARCHAR(100),
    p_username VARCHAR(50),
    p_gender VARCHAR(10),
    p_salary NUMERIC,
    p_department_id INT
)
RETURNS VOID AS $$


BEGIN
    UPDATE employees
    SET firstname = p_firstname,
        lastname = p_lastname,
        dateofbirth = p_dateofbirth,
        email = p_email,
        address = p_address,
        username = p_username,
        gender = p_gender,
        salary = p_salary,
        department_id = p_department_id
    WHERE employee_id = p_employee_id;
END;
$$ LANGUAGE plpgsql;

//CALLING THIS FUNCTION
SELECT update_employee(1, 'Jane', 'Smith', '1991-08-20', 'jane.smith@example.com', '456 Elm St', 'janesmith', 'Female', 60000, 2);

//DELETING TABLE USING STORED PROCEDURE
CREATE OR REPLACE FUNCTION delete_employee(p_employee_id INTEGER)
RETURNS VOID AS $$
BEGIN
    DELETE FROM employees WHERE employee_id = p_employee_id;
END;
$$ LANGUAGE plpgsql;

//CALLING THIS FUNCTION
SELECT delete_employee(2);


To implement a CRUD operations with single stored procedure method for your given project.
Implementing CRUD operations using a single stored procedure in PostgreSQL involves using conditional logic to determine the action to perform based on input parameters.



//SINGLE STORE PROCEDURE CRUD
CREATE OR REPLACE FUNCTION manage_employee(
    p_action VARCHAR(10),  -- 'create', 'read', 'update', 'delete'
    p_employee_id INT DEFAULT NULL,
    p_firstname VARCHAR(50) DEFAULT NULL,
    p_lastname VARCHAR(50) DEFAULT NULL,
    p_dateofbirth DATE DEFAULT NULL,
    p_email VARCHAR(100) DEFAULT NULL,
    p_address VARCHAR(100) DEFAULT NULL,
    p_username VARCHAR(50) DEFAULT NULL,
    p_password VARCHAR(50) DEFAULT NULL,
    p_gender VARCHAR(10) DEFAULT NULL,
    p_salary NUMERIC DEFAULT NULL,
    p_department_id INT DEFAULT NULL
)


RETURNS VOID AS $$
BEGIN
    IF p_action = 'create' THEN
        INSERT INTO employees (firstname, lastname, dateofbirth, email, address, username, password, gender, salary, department_id)
        VALUES (p_firstname, p_lastname, p_dateofbirth, p_email, p_address, p_username, p_password, p_gender, p_salary, p_department_id); 
    ELSIF p_action = 'read' THEN
        -- Assuming we want to return employee details
        RETURN QUERY SELECT * FROM employees WHERE employee_id = p_employee_id;
    ELSIF p_action = 'update' THEN
        UPDATE employees
        SET firstname = COALESCE(p_firstname, firstname),
            lastname = COALESCE(p_lastname, lastname),
            dateofbirth = COALESCE(p_dateofbirth, dateofbirth),
            email = COALESCE(p_email, email),
            address = COALESCE(p_address, address),
            username = COALESCE(p_username, username),
            password = COALESCE(p_password, password),
            gender = COALESCE(p_gender, gender),
            salary = COALESCE(p_salary, salary),
            department_id = COALESCE(p_department_id, department_id)
        WHERE employee_id = p_employee_id;
    ELSIF p_action = 'delete' THEN
        DELETE FROM employees WHERE employee_id = p_employee_id;
    ELSE
        RAISE EXCEPTION 'Invalid action: %', p_action;
    END IF;
END;
$$ LANGUAGE plpgsql;



//CALLING THE FUNCTIONS
//CREATE
CALL manage_employee('create', NULL, 'John', 'Doe', '1990-05-15', 'john.doe@example.com', '123 Main St', 'johndoe', 'password123', 'Male', 50000, 1);



//READ
SELECT * FROM manage_employee('read', 1);



//UPDATE
CALL manage_employee('update', 1, 'Jane', 'Smith', '1991-08-20', 'jane.smith@example.com', '456 Elm St', 'janesmith', NULL, 'Female', 60000, 2);



//DELETE
CALL manage_employee('delete', 2);


