# Joins

## Explanation of Joins in SQL

Joins in SQL are used to combine data from two or more tables in a relational database. Joins allow you to retrieve data that is spread across multiple tables by linking related data together.

A join creates a new virtual table that contains data from the tables being joined. The data in the virtual table is a combination of data from the original tables that match a specific condition. The condition for joining tables is typically based on the values of a common column or set of columns in each table.

Joins are an essential tool for retrieving complex data from a database. By linking related data together, joins allow you to retrieve data that is spread across multiple tables in a single query.

## Overview of Different Types of Joins and How to Use Them

There are several types of joins in SQL, including `inner join`, `left join`, `right join`, and `full outer join`. Each type of join is used to combine data from two or more tables in a different way.

Here a summary schema for each type of join : 


![Screenshot](https://cartman34.fr/wp-content/uploads/2017/01/sql_joins.jpg)

- **Inner Join**: This join returns only the rows that have matching values in both tables. In the image, the result of an inner join between tables A and B is shown. Only the rows that have matching values in both tables are included in the result set.
- **Left Join**: This join returns all the rows from the left table and the matching rows from the right table. If there is no match in the right table, the result will contain NULL values for those columns. In the image, the result of a left join between tables A and B is shown. All the rows from table A are included in the result set, and the matching rows from table B are included. Rows in table A that have no matching rows in table B are included, with NULL values for the columns in table B.
- **Right Join**: This join returns all the rows from the right table and the matching rows from the left table. If there is no match in the left table, the result will contain NULL values for those columns. In the image, the result of a right join between tables A and B is shown. All the rows from table B are included in the result set, and the matching rows from table A are included. Rows in table B that have no matching rows in table A are included, with NULL values for the columns in table A.
- **Full Outer Join**: This join returns all the rows from both tables, with NULL values in the columns where there is no match. In the image, the result of a full outer join between tables A and B is shown. All the rows from both tables are included in the result set, with NULL values in the columns where there is no match.
- **Left Outer Join or Left Excluding Join**: This join returns all the rows from the left table that do not have a matching row in the right table. In the image, the result of a left outer join (or left excluding join) between tables A and B is shown. Only the rows from table A that do not have a matching row in table B are included in the result set.
- **Right Outer Join or Right Excluding Join**: This join returns all the rows from the right table that do not have a matching row in the left table. In the image, the result of a right outer join (or right excluding join) between tables A and B is shown. Only the rows from table B that do not have a matching row in table A are included in the result set.

### Some examples on MySQL Employees Database

#### Inner Join

An inner join returns only the rows that have matching values in both tables being joined. The syntax for an inner join is as follows:

```sql
SELECT column1, column2, ... FROM table1
INNER JOIN table2
ON table1.column = table2.column;
```

In this syntax, the `INNER JOIN` keyword specifies that an inner join is being performed. The `ON` keyword specifies the condition for joining the tables. The columns being joined must have the same data type and contain similar data.

Here's an example of an inner join that combines data from the "employees" and "departments" tables:

```sql
SELECT employees.emp_no, employees.first_name, employees.last_name, departments.dept_name
FROM employees
INNER JOIN departments
ON employees.dept_no = departments.dept_no;
```

This query returns a result set that includes the employee number, first name, last name, and department name for each employee. The INNER JOIN operator links the "employees" and "departments" tables on the "dept_no" column, and returns only the rows where there is a match between the two tables.

#### Left Join

A left join returns all the rows from the left table and the matching rows from the right table. If there are no matching rows in the right table, the result set will contain NULL values for the columns in the right table. The syntax for a left join is as follows:

```sql
SELECT column1, column2, ... FROM table1
LEFT JOIN table2
ON table1.column = table2.column;
```

In this syntax, the `LEFT JOIN` keyword specifies that a left join is being performed. The ON keyword specifies the condition for joining the tables.

Here's an example of a left join that combines data from the `employees` and `departments` tables:

```sql
SELECT employees.emp_no, employees.first_name, employees.last_name, departments.dept_name
FROM employees
LEFT JOIN departments
ON employees.dept_no = departments.dept_no;
```
This query returns a result set that includes the employee number, first name, last name, and department name for each employee. The `LEFT JOIN` operator links the `employees` and `departments` tables on the `dept_no` column, and returns all the rows from the `employees` table, and the matching rows from the `departments` table.

#### Right Join

A right join returns all the rows from the right table and the matching rows from the left table. If there are no matching rows in the left table, the result set will contain `NULL` values for the columns in the left table. 

Here's an example of a right join that combines data from the `employees` and `departments` tables:

```sql
SELECT employees.emp_no, employees.first_name, employees.last_name, departments.dept_name
FROM employees
RIGHT JOIN departments
ON employees.dept_no = departments.dept_no;
```

This query returns a result set that includes the employee number, first name, last name, and department name for each employee. The `RIGHT JOIN` operator links the `employees` and `departments` tables on the `dept_no` column, and returns all the rows from the `departments` table, and the matching rows from the `employees` table.

#### Full Outer Join

A full outer join returns all the rows from both tables being joined, and `NULL` values for the columns that do not have matching values in the other table. The syntax for a full outer join varies depending on the database management system being used. In MySQL, a full outer join can be simulated using a combination of left join and union operators:

```sql
SELECT employees.emp_no, employees.first_name, employees.last_name, departments.dept_name
FROM employees
LEFT JOIN departments
ON employees.dept_no = departments.dept_no
UNION
SELECT employees.emp_no, employees.first_name, employees.last_name, departments.dept_name
FROM employees
RIGHT JOIN departments
ON employees.dept_no = departments.dept_no
WHERE employees.dept_no IS NULL;
```

This query combines the results of a `left join` and a `right join` to simulate a full outer join. The first `SELECT` statement performs a `left join` and returns all the rows from the `employees` table and the matching rows from the `departments` table. The second `SELECT` statement performs a right join and returns all the rows from the `departments` table and the matching rows from the `employees` table where there is no match in the `employees` table. The `UNION` operator combines the results of the two `SELECT` statements.

This is the same version with the `full outer join` keyword : 

```sql
SELECT employees.emp_no, employees.first_name, employees.last_name, departments.dept_name
FROM employees
FULL OUTER JOIN departments
ON employees.dept_no = departments.dept_no
WHERE employees.dept_no IS NULL OR departments.dept_no IS NULL;
```
In this query, the `FULL OUTER JOIN` returns all the rows from both tables, including those that do not have a match in the other table. The `WHERE` clause filters the result set to include only the rows where either the `employees.dept_no` or `departments.dept_no` is `NULL`, which indicates that there is no match in the other table.

### More examples of `left join`, `inner join` and `right join`

#### Inner Join Example 1

Suppose you want to retrieve data that shows the salary of each employee along with the department name for the department they work in. You can use an inner join to link the `employees` and `dept_emp` tables on the `emp_no` column and the `departments` and `dept_emp` tables on the `dept_no` column:

```sql
SELECT e.emp_no, e.first_name, e.last_name, d.dept_name, s.salary
FROM employees e
INNER JOIN dept_emp de ON e.emp_no = de.emp_no
INNER JOIN departments d ON de.dept_no = d.dept_no
INNER JOIN salaries s ON e.emp_no = s.emp_no;
```

This query returns a result set that includes the employee number, first name, last name, department name, and salary for each employee. The `INNER JOIN` operator links the `employees` and `dept_emp` tables on the `emp_no` column, and links the `departments` and `dept_emp` tables on the `dept_no` column, and links the `salaries` table on the `emp_no` column.

#### Inner Join Example 2

Suppose you want to retrieve data that shows the department name and manager's name for each department in the company. You can use an inner join to link the `departments` and `dept_manager` tables on the `dept_no` column, and link the `employees` table on the `emp_no` column to get the name of the manager:

```sql
SELECT d.dept_name, e.first_name, e.last_name
FROM departments d
INNER JOIN dept_manager dm ON d.dept_no = dm.dept_no
INNER JOIN employees e ON dm.emp_no = e.emp_no;
```

This query returns a result set that includes the department name and the first and last name of the manager for each department. The INNER JOIN operator links the "departments" and "dept_manager" tables on the "dept_no" column, and links the "employees" table on the "emp_no" column to get the name of the manager.

#### Left Join Example 1

Suppose you want to retrieve data that shows the name and department of each employee, even if they are not currently assigned to a department. You can use a left join to link the "employees" and "dept_emp" tables on the "emp_no" column, and link the "departments" table on the "dept_no" column:

```sql
SELECT e.first_name, e.last_name, d.dept_name
FROM employees e
LEFT JOIN dept_emp de ON e.emp_no = de.emp_no
LEFT JOIN departments d ON de.dept_no = d.dept_no;
```

This query returns a result set that includes the first name, last name, and department name for each employee. The LEFT JOIN operator links the "employees" and "dept_emp" tables on the "emp_no" column, and links the "departments" table on the "dept_no" column. Even if an employee is not currently assigned to a department, their name will still appear in the result set with a NULL value for the "dept_name" column.

#### Left Join Example 2

Suppose you want to retrieve data that shows the total number of sales made by each employee, even if they have not made any sales. You can use a left join to link the "employees" and "sales" tables on the "emp_no" column:

```sql
SELECT e.emp_no, e.first_name, e.last_name, COUNT(s.sales_amount) as total_sales
FROM employees e
LEFT JOIN sales s ON e.emp_no = s.emp_no
GROUP BY e.emp_no;
```

#### Right Join Example 1 

Retrieving data that shows the name and department of each employee, even if the department has no employees assigned to it.

```sql
SELECT e.first_name, e.last_name, d.dept_name
FROM dept_emp de
RIGHT JOIN employees e ON de.emp_no = e.emp_no
RIGHT JOIN departments d ON de.dept_no = d.dept_no;
```

#### Right Join Example 2

Retrieving data that shows the total number of sales made by each employee, even if they have not made any sales.

```sql
SELECT e.emp_no, e.first_name, e.last_name, COUNT(s.sales_amount) as total_sales
FROM sales s
RIGHT JOIN employees e ON s.emp_no = e.emp_no
GROUP BY e.emp_no;
```

## Conclusion

Joins are an essential tool in SQL for retrieving data that is spread across multiple tables in a relational database. There are several types of joins available, including `inner join`, `left join`, `right join`, and `full outer join`. By understanding how to use joins in SQL, you can retrieve complex data from a database and link related data together.







