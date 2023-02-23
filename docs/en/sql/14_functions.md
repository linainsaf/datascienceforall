# SQL Functions

## Introduction to Functions in SQL

SQL functions are built-in functions that are used to perform operations on data in a database. They take one or more arguments as input, perform a specific operation, and return a result. Functions can be used in SELECT, WHERE, HAVING, and ORDER BY clauses of a SQL query.

There are many different types of functions in SQL, including aggregate functions, scalar functions, date and time functions, and string functions. Each type of function performs a specific operation on data and returns a result.

Overall, SQL functions are essential for data analysis because they allow you to perform complex calculations, filter data based on specific criteria, clean up messy data, aggregate data to provide insights into trends and patterns, and transform data from one format to another. By mastering SQL functions, you can become a more effective data analyst and make more informed decisions based on your data.

## Overview of Different Types of Functions and Examples

### Aggregate Functions

Aggregate functions are used to perform calculations on groups of rows and return a single value. Some common aggregate functions are:

- `COUNT()`: returns the number of rows in a table or the number of non-null values in a column.
```sql
SELECT COUNT(*) as num_employees
FROM employees;
```
This query counts the number of rows in the "employees" table and assigns the name "num_employees" to the output column.
- `SUM()`: returns the sum of values in a column.
```
SELECT SUM(salary) as total_salary
FROM salaries;
```
This query calculates the total salary of all employees in the "salaries" table and assigns the name "total_salary" to the output column.
-` AVG()`: returns the average value of a column.
```sql
SELECT AVG(salary) as avg_salary
FROM salaries;
```
This query calculates the average salary of all employees in the "salaries" table and assigns the name "avg_salary" to the output column.
- `MAX()`: returns the maximum value in a column.
```sql
SELECT MAX(salary) as max_salary
FROM salaries;
```
This query finds the highest salary in the "salaries" table and assigns the name "max_salary" to the output column.
- `MIN()`: returns the minimum value in a column.
```sql
SELECT MIN(salary) as min_salary
FROM salaries;
```
This query finds the lowest salary in the "salaries" table and assigns the name "min_salary" to the output column.

### Scalar Functions

Scalar functions are used to perform operations on individual values and return a single value. Some common scalar functions are:

- `UPPER()`: converts a string to uppercase.
```sql
SELECT UPPER(first_name) as upper_first_name
FROM employees;
```
This query converts the first name of all employees in the "employees" table to uppercase and assigns the name "upper_first_name" to the output column.
- `LOWER()`: converts a string to lowercase.
```sql
SELECT LOWER(last_name) as lower_last_name
FROM employees;
```
This query converts the last name of all employees in the "employees" table to lowercase and assigns the name "lower_last_name" to the output column.
-` LENGTH()`: returns the length of a string.
```sql
SELECT first_name, LENGTH(first_name) as name_length
FROM employees;
```
This query returns the first name of all employees in the "employees" table, and calculates the length of each name and assigns the name "name_length" to the output column.

### Date and Time Functions

Date and time functions are used to perform operations on date and time values. Some common date and time functions are:

- `DATE()`: extracts the date part from a datetime value.
```sql
SELECT hire_date, DATE(hire_date) as hire_date_only
FROM employees;
```
This query extracts the date part from the "hire_date" column of the "employees" table and assigns the name "hire_date_only" to the output column.
- `YEAR()`: returns the year from a date value.
```sql
SELECT hire_date, YEAR(hire_date) as hire_year
FROM employees;
```
This query returns the "hire_date" column of the "employees" table and calculates the year each employee was hired. The name "hire_year" is assigned to the output column.

Same `MONTH()` function : 
```sql
SELECT hire_date, MONTH(hire_date) as hire_month
FROM employees;
```
This query returns the "hire_date" column of the "employees" table and calculates the month each employee was hired. The name "hire_month" is assigned to the output column.

### String Functions

String functions are used to perform operations on string values. Some common string functions are:

- CONCAT(): concatenates two or more strings together.
```sql
SELECT CONCAT(first_name, ' ', last_name) as full_name
FROM employees;
```
This query combines the first name and last name columns of the "employees" table and assigns the name "full_name" to the output column.
- LEFT(): returns the leftmost characters of a string.
```sql
SELECT first_name, LEFT(first_name, 3) as initial
FROM employees;
```
This query returns the first name of all employees in the "employees" table and extracts the first three characters of each name. The name "initial" is assigned to the output column.
- REPLACE(): replaces a substring in a string with another substring.
```sql
SELECT REPLACE(email, 'gmail', 'yahoo') as new_email
FROM employees;
```
This query returns the email column of the "employees" table and replaces the substring 'gmail' with 'yahoo' in each email address. The name "new_email" is assigned to the output column.

## Mix up 
Let's take a look at three examples of SQL queries that use a mix of functions on the "employees" database again. 

### Find the average salary of employees by department, and round the results to two decimal places:

```sql
SELECT department, ROUND(AVG(salary), 2) as avg_salary
FROM employees
JOIN dept_emp ON employees.emp_no = dept_emp.emp_no
JOIN departments ON dept_emp.dept_no = departments.dept_no
GROUP BY department;
```
This query joins the "employees", "dept_emp", and "departments" tables, and uses the AVG() function to calculate the average salary of employees in each department. The ROUND() function is used to round the results to two decimal places. The output includes the department name and the average salary for each department.

Don't worry about the `JOIN` clause we will get to it in detail later. 

### Find the top 10 most common first names among employees, and show the number of employees with each name:

```sql
SELECT first_name, COUNT(*) as num_employees
FROM employees
GROUP BY first_name
ORDER BY num_employees DESC
LIMIT 10;
```
This query uses the COUNT() function to count the number of employees with each first name, and the GROUP BY clause to group the results by first name. The ORDER BY clause is used to sort the results in descending order by the number of employees, and the LIMIT clause is used to show only the top 10 results.

### Find the number of employees hired in each year, and show the results as a percentage of the total number of employees:

```sql
SELECT YEAR(hire_date) as hire_year, COUNT(*) / (SELECT COUNT(*) FROM employees) * 100 as percentage
FROM employees
GROUP BY hire_year;
```

This query uses the YEAR() function to extract the year from the hire date of each employee, and the COUNT() function to count the number of employees hired in each year. The subquery (SELECT COUNT(*) FROM employees) is used to calculate the total number of employees in the "employees" table. The percentage of employees hired in each year is calculated by dividing the count by the total number of employees and multiplying by 100. The output includes the hire year and the percentage of employees hired in that year.


### Find the number of employees born in each month, and sort the results by month:

```sql
SELECT MONTH(birth_date) as birth_month, COUNT(*) as num_employees
FROM employees
GROUP BY birth_month
ORDER BY birth_month;
```

This query uses the MONTH() function to extract the month from the birth date of each employee, and the COUNT() function to count the number of employees born in each month. The GROUP BY clause is used to group the results by birth month, and the ORDER BY clause is used to sort the results by month.

### Find the number of employees who were hired in each year, and show the results as a bar chart:

```sql
SELECT YEAR(hire_date) as hire_year, COUNT(*) as num_employees
FROM employees
GROUP BY hire_year;
```

This query uses the YEAR() function to extract the year from the hire date of each employee, and the COUNT() function to count the number of employees hired in each year. The GROUP BY clause is used to group the results by hire year. You can visualize the results as a bar chart in a data visualization tool, such as Tableau or Power BI, to see the distribution of hires over time.


## Conclusion

SQL functions are powerful tools that allow you to perform operations on data and return meaningful results. They can be used to calculate aggregate values, manipulate strings, and work with date and time values. By understanding the different types of functions available in SQL and how to use them in queries, you can perform complex data analysis and retrieve valuable insights from your data.


