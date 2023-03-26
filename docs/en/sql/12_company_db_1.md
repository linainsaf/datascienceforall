# Company Database Introduction 

In the modern era of technology, most businesses depend on software to manage and track various aspects of their operations. One of the most important types of software that businesses rely on is the database management system (DBMS). A DBMS allows companies to store, manage, and retrieve information in a structured and organized way. A company database is a type of DBMS that is specifically designed to help organizations store and manage information about their employees, customers, products, and services.

<br />

A company database can be used for a wide range of purposes, such as tracking inventory, processing transactions, generating reports, and analyzing data. By keeping all the relevant information in a centralized location, a company database provides a more efficient and accurate way to manage and analyze data. This, in turn, enables businesses to make informed decisions based on reliable and up-to-date information.

<br />

Having a well-designed and properly maintained company database is vital to the success of any business. A good company database can help to improve operational efficiency, streamline processes, and enhance customer satisfaction. It can also help organizations to identify trends, analyze performance, and make strategic decisions based on real data.

<br />

In this chapter, we will explore the various components of a company database, including tables, fields, and relationships. We will also learn how to design and build a company database from scratch, as well as how to use SQL to retrieve, analyze, and manipulate data. By the end of this tutorial, you should have a good understanding how to build a database from scratch for your own business or project.

## Summary of the project 

This project will focus on building a company database that includes seven tables:
* employees
* departments
* projects
* department_projects
* employee_projects
* jobs
* location

These tables will be linked together using foreign keys and relationships, allowing organizations to easily access and manage information. The project will also include the creation of primary keys, indexes, and constraints to ensure the integrity and consistency of the data. This database will provide a robust platform for companies to store, organize, and access data in a way that enhances their ability to make data-driven decisions.

## Set up the project 

Start by opening your MySQL client and connecting to your server. In our case just start MAMP like in the installation section. 

### Database creation 
Create a new database call `company` with the graphic interface like in the installation section or with the SQL command line : 

```sql
CREATE DATABASE company;
```

if you used the command line option run also this command :

```slq
USE company;
``` 
like you've guess it just tell to MySQL to use our database for the futur queries.

### Tables creation

This is the SQL script that creates the necessary tables : 

```sql
-- Create Employees table
CREATE TABLE Employees (
    EmployeeID INT AUTO_INCREMENT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    Email VARCHAR(50),
    Phone VARCHAR(20),
    HireDate DATE,
    Salary DECIMAL(10,2),
    CommissionPct DECIMAL(4,2),
    ManagerID INT,
    DepartmentID INT,
    JobID INT,
    LocationID INT,
    CONSTRAINT fk_manager FOREIGN KEY (ManagerID) REFERENCES Employees(EmployeeID),
    CONSTRAINT fk_department FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID),
    CONSTRAINT fk_job FOREIGN KEY (JobID) REFERENCES Jobs(JobID),
    CONSTRAINT fk_location FOREIGN KEY (LocationID) REFERENCES Locations(LocationID)
);

-- Create Departments table
CREATE TABLE Departments (
    DepartmentID INT AUTO_INCREMENT PRIMARY KEY,
    DepartmentName VARCHAR(50),
    ManagerID INT,
    LocationID INT,
    CONSTRAINT fk_department_manager FOREIGN KEY (ManagerID) REFERENCES Employees(EmployeeID),
    CONSTRAINT fk_department_location FOREIGN KEY (LocationID) REFERENCES Locations(LocationID)
);

-- Create Projects table
CREATE TABLE Projects (
    ProjectID INT AUTO_INCREMENT PRIMARY KEY,
    ProjectName VARCHAR(50),
    StartDate DATE,
    EndDate DATE,
    Budget DECIMAL(15,2)
);

-- Create Department_Projects table
CREATE TABLE Department_Projects (
    DepartmentID INT,
    ProjectID INT,
    CONSTRAINT fk_department_project_department FOREIGN KEY (DepartmentID) REFERENCES Departments(DepartmentID),
    CONSTRAINT fk_department_project_project FOREIGN KEY (ProjectID) REFERENCES Projects(ProjectID)
);

-- Create Employee_Projects table
CREATE TABLE Employee_Projects (
    EmployeeID INT,
    ProjectID INT,
    HoursWorked DECIMAL(8,2),
    CONSTRAINT fk_employee_project_employee FOREIGN KEY (EmployeeID) REFERENCES Employees(EmployeeID),
    CONSTRAINT fk_employee_project_project FOREIGN KEY (ProjectID) REFERENCES Projects(ProjectID)
);

-- Create Jobs table
CREATE TABLE Jobs (
    JobID INT AUTO_INCREMENT PRIMARY KEY,
    JobTitle VARCHAR(50),
    MinSalary DECIMAL(10,2),
    MaxSalary DECIMAL(10,2)
);

-- Create Locations table
CREATE TABLE Locations (
    LocationID INT AUTO_INCREMENT PRIMARY KEY,
    Address VARCHAR(50),
    City VARCHAR(50),
    StateProvince VARCHAR(50),
    Country VARCHAR(50),
    PostalCode VARCHAR(50)
);

```

Note that the foreign keys are created using the `CONSTRAINT` keyword and the `REFERENCES` keyword to specify the table and column to which the key refers. The `AUTO_INCREMENT` keyword is used to specify that the primary key column should automatically increment for each new row.


### Insert data

Then populate the tables with this script : 

```sql
-- Insert 10 employees
INSERT INTO employees (first_name, last_name, email, phone, hire_date, job_id, salary, manager_id, department_id)
VALUES
  ('John', 'Doe', 'johndoe@example.com', '555-555-1234', '2022-01-01', 1, 50000, NULL, 1),
  ('Jane', 'Doe', 'janedoe@example.com', '555-555-5678', '2022-01-01', 2, 60000, 1, 1),
  ('Bob', 'Smith', 'bobsmith@example.com', '555-555-9012', '2022-02-01', 3, 75000, 2, 2),
  ('Alice', 'Johnson', 'alicejohnson@example.com', '555-555-3456', '2022-02-01', 4, 85000, 2, 2),
  ('Mark', 'Lee', 'marklee@example.com', '555-555-7890', '2022-03-01', 5, 95000, 2, 3),
  ('Emily', 'Chen', 'emilychen@example.com', '555-555-2345', '2022-03-01', 5, 80000, 2, 3),
  ('Sara', 'Kim', 'sarakim@example.com', '555-555-6789', '2022-04-01', 6, 70000, 3, 4),
  ('Michael', 'Wu', 'michaelwu@example.com', '555-555-0123', '2022-04-01', 7, 65000, 3, 4),
  ('David', 'Nguyen', 'davidnguyen@example.com', '555-555-4567', '2022-05-01', 8, 55000, 4, 5),
  ('Jennifer', 'Garcia', 'jennifergarcia@example.com', '555-555-8901', '2022-05-01', 9, 60000, 4, 5);

-- Insert 3 departments
INSERT INTO departments (name, manager_id, location_id)
VALUES
  ('Engineering', 1, 1),
  ('Marketing', 2, 2),
  ('Sales', 3, 3);

-- Insert 4 projects
INSERT INTO projects (name, start_date, end_date)
VALUES
  ('Project A', '2022-01-01', '2022-03-01'),
  ('Project B', '2022-02-01', '2022-05-01'),
  ('Project C', '2022-03-01', '2022-06-01'),
  ('Project D', '2022-04-01', '2022-07-01');

-- Insert 4 department_projects relationships
INSERT INTO department_projects (department_id, project_id)
VALUES
  (1, 1),
  (1, 2),
  (2, 3),
  (3, 4);

-- insert 10 rows into department_projects table
INSERT INTO department_projects (department_id, project_id) VALUES
  (1, 1),
  (1, 2),
  (2, 1),
  (2, 3),
  (3, 2),
  (3, 3),
  (4, 1),
  (4, 2),
  (5, 1),
  (5, 3);

-- insert 10 rows into jobs table
INSERT INTO jobs (title, min_salary, max_salary) VALUES
  ('Manager', 70000, 120000),
  ('Salesperson', 20000, 40000),
  ('Developer', 50000, 100000),
  ('Accountant', 35000, 60000),
  ('HR Manager', 45000, 80000),
  ('Marketing Specialist', 40000, 75000),
  ('Administrative Assistant', 25000, 35000),
  ('Designer', 45000, 80000),
  ('Writer', 30000, 50000),
  ('Engineer', 55000, 90000);

-- insert 10 rows into location table
INSERT INTO location (city, state, country) VALUES
  ('New York', 'NY', 'USA'),
  ('Los Angeles', 'CA', 'USA'),
  ('San Francisco', 'CA', 'USA'),
  ('Chicago', 'IL', 'USA'),
  ('Houston', 'TX', 'USA'),
  ('London', NULL, 'England'),
  ('Paris', NULL, 'France'),
  ('Berlin', NULL, 'Germany'),
  ('Sydney', NULL, 'Australia'),
  ('Tokyo', NULL, 'Japan');

-- Insert 10 rows into the employee_projects table
INSERT INTO employee_projects (employee_id, project_id, start_date, end_date)
VALUES (1, 1, '2021-01-01', '2021-06-30'),
       (1, 2, '2021-07-01', '2021-12-31'),
       (2, 1, '2021-01-01', '2021-06-30'),
       (2, 3, '2021-07-01', '2021-12-31'),
       (3, 2, '2021-01-01', '2021-06-30'),
       (3, 3, '2021-07-01', '2021-12-31'),
       (4, 2, '2021-01-01', '2021-06-30'),
       (4, 1, '2021-07-01', '2021-12-31'),
       (5, 3, '2021-01-01', '2021-06-30'),
       (5, 1, '2021-07-01', '2021-12-31');

```

### More informations about `CONSTRAINT` and `REFERENCES`

The CONSTRAINT and REFERENCES keywords are used to create foreign key constraints in SQL. A foreign key constraint is a rule that ensures the values in a column or set of columns in one table are matched by values in another table.

<br />

In the example code I provided earlier, we used the CONSTRAINT keyword to create foreign key constraints between the employee_projects and employees tables, as well as between the department_projects and departments tables.

<br />

Here is an example of the foreign key constraint between the employee_projects and employees tables:

```
CREATE TABLE employee_projects (
    id INT AUTO_INCREMENT PRIMARY KEY,
    employee_id INT,
    project_id INT,
    hours_worked DECIMAL(5,2),
    CONSTRAINT fk_employee_project_employee
        FOREIGN KEY (employee_id)
        REFERENCES employees (id)
);
```

In this example, we first create the employee_projects table with the id, employee_id, project_id, and hours_worked columns. Then we use the CONSTRAINT keyword to create a foreign key constraint named fk_employee_project_employee. The FOREIGN KEY clause specifies the employee_id column as the foreign key column, and the REFERENCES clause specifies the employees table and the id column as the referenced column.

<br />

This foreign key constraint ensures that every value in the employee_id column in the employee_projects table must exist in the id column of the employees table.

<br />

The CONSTRAINT and REFERENCES keywords are powerful tools that allow you to establish relationships between tables in a database. They can help ensure data integrity and improve the accuracy and reliability of your queries.

## Tests some queries for verification --> TO TEST 
Let's take a look to ten example queries to verify the data in the seven tables:

* Retrieve all employees who work in the "Sales" department:
```
SELECT e.first_name, e.last_name, d.department_name
FROM employees e
JOIN departments d ON e.department_id = d.department_id
WHERE d.department_name = 'Sales';
```
* Retrieve all projects that are assigned to the "Marketing" department:
```
SELECT p.project_name, d.department_name
FROM projects p
JOIN department_projects dp ON p.project_id = dp.project_id
JOIN departments d ON dp.department_id = d.department_id
WHERE d.department_name = 'Marketing';
```
* Retrieve all projects that have an estimated cost greater than $100,000:
```
SELECT project_name, estimated_cost
FROM projects
WHERE estimated_cost > 100000;
```
* Retrieve all employees who are working on the "Big Project":
```
SELECT e.first_name, e.last_name, p.project_name
FROM employees e
JOIN employee_projects ep ON e.employee_id = ep.employee_id
JOIN projects p ON ep.project_id = p.project_id
WHERE p.project_name = 'Big Project';
```
* Retrieve all job titles and the number of employees who hold each job title:
```
SELECT j.job_title, COUNT(*) AS num_employees
FROM employees e
JOIN jobs j ON e.job_id = j.job_id
GROUP BY j.job_title;
```
* List all employees and their department:
```
SELECT e.employee_id, e.first_name, e.last_name, d.department_name 
FROM employees e 
INNER JOIN departments d ON e.department_id = d.department_id;
```
* List all departments and their location:
```
SELECT d.department_name, l.city, l.state 
FROM departments d 
INNER JOIN location l ON d.location_id = l.location_id;
```
* List all projects and their department:
```
SELECT p.project_id, p.project_name, d.department_name 
FROM projects p 
INNER JOIN department_projects dp ON p.project_id = dp.project_id 
INNER JOIN departments d ON dp.department_id = d.department_id;
```
* List all employees and the projects they are working on:
```
SELECT e.first_name, e.last_name, p.project_name 
FROM employees e 
INNER JOIN employee_projects ep ON e.employee_id = ep.employee_id 
INNER JOIN projects p ON ep.project_id = p.project_id;
```
* List all employees, their job title, and salary:
```
SELECT e.first_name, e.last_name, j.job_title, j.salary 
FROM employees e 
INNER JOIN jobs j ON e.job_id = j.job_id;
```


## The importance of schema and organization

Creating a schema and diagrams for a database is critical, especially when dealing with large databases with many tables and relationships. Without proper organization and documentation, it can be challenging to understand the structure and relationships between different tables. 

<br />

This is particularly true when many people are working on the database or when there is a lot of data being added and updated regularly. Having a clear schema and diagrams can help developers and users understand the structure and relationships of the data, leading to more efficient and effective use of the database. Additionally, it can help to identify and prevent errors in the data or in the database design itself. Overall, investing time in creating a clear schema and diagrams can save time and resources in the long run and make the database easier to manage and use.


## Wrap-up 

* In this project, we learned how to create a company database using MySQL.
* We created 7 tables: employees, departments, projects, department_projects, employee_projects, jobs, and location.
* We added primary keys to all tables with auto-increment options for unique identification and added foreign keys to establish relationships between tables.
* We inserted data into each table and test some queries.
* We also learned about the importance of schema and diagrams for databases, especially as the number of tables and relationships grows, and the significance of foreign keys in ensuring data integrity and consistency.



