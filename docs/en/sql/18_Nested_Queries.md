# Nested Queries

## Introduction to Nested Queries

A nested query, also known as a subquery, is a query that is nested inside another query. A subquery can be used to retrieve data that will be used in the main query, allowing for complex queries that would be difficult to write using a single query. Subqueries can be used with various clauses in SQL, such as `SELECT`, `WHERE`, and `HAVING`.

## Overview of How to Use Subqueries

A subquery is typically enclosed in parentheses and used in conjunction with an operator such as `IN`, `EXISTS`, or `=.` The subquery can be used in various parts of a query, depending on the desired result.

Here's an example of a subquery used in a `SELECT` statement:
```sql
SELECT first_name, last_name, birth_date
FROM employees
WHERE birth_date > (SELECT birth_date FROM employees WHERE emp_no = 10001);
```
This query returns a result set that includes the first name, last name, and birth date for each employee whose birth date is later than that of the employee with `emp_no = 10001`. The subquery is used in the `WHERE` clause to retrieve the birth date of the employee with `emp_no = 10001`.

Here's an example of a subquery used in a `HAVING` clause:
```sql
SELECT departments.dept_name, AVG(salaries.salary) AS avg_salary
FROM employees
INNER JOIN dept_emp ON employees.emp_no = dept_emp.emp_no
INNER JOIN departments ON dept_emp.dept_no = departments.dept_no
INNER JOIN salaries ON employees.emp_no = salaries.emp_no
GROUP BY departments.dept_name
HAVING AVG(salaries.salary) > (SELECT AVG(salary) FROM salaries);
```

This query joins the employees, salaries, dept_emp, and departments tables together based on the employee number, department number, and salary information. It then groups the results by department name using the `GROUP BY` clause and calculates the average salary for each department using the `AVG` function.

The `HAVING` clause is used to filter the results based on the condition that the average salary for a department is greater than the overall average salary of all employees, which is calculated using a subquery that selects the average salary from the salaries table.

## Some examples with MySQL Employees database

### Example 1: Retrieving data for employees who are currently managers
```sql
SELECT first_name, last_name, hire_date
FROM employees
WHERE emp_no IN (
	SELECT emp_no
	FROM dept_manager
);
```
This query returns a result set that includes the first name, last name, and hire date for each employee who is currently a manager. The subquery is used in the WHERE clause to retrieve the employee numbers of all employees who are currently department managers.

### Example 2: Retrieving data for employees who were hired in the same year as a specific employee
 
```sql
SELECT first_name, last_name, hire_date
FROM employees
WHERE YEAR(hire_date) = (
	SELECT YEAR(hire_date)
	FROM employees
	WHERE emp_no = 10001
);
```
This query returns a result set that includes the first name, last name, and hire date for each employee who was hired in the same year as the employee with emp_no = 10001. The subquery is used in the WHERE clause to retrieve the year in which the employee with emp_no = 10001 was hired.

### Example 3: Find the 10th employee with the highest salary, along with their job title

```sql
SELECT employees.emp_no, employees.first_name, employees.last_name, MAX(salaries.salary) AS max_salary, MAX(titles.title) AS title
FROM employees
JOIN salaries ON employees.emp_no = salaries.emp_no
JOIN titles ON employees.emp_no = titles.emp_no
GROUP BY employees.emp_no
ORDER BY max_salary DESC
LIMIT 10; 
```

The query starts by selecting specific columns from the employees table, including the employee number, first name, and last name. It then joins the salaries and titles tables to the employees table based on the employee number, using the `JOIN` clause. The `MAX` function is used to find the maximum salary and job title for each employee.

The `GROUP BY` clause is used to group the results by employee number. This ensures that the maximum salary and job title returned for each employee correspond to the same person. The `ORDER BY` clause is used to sort the results in descending order based on the maximum salary, so that the employee with the highest salary is at the top.

Finally, the `LIMIT` clause is used to limit the results to only the top row, which corresponds to the employee with the highest salary.

## Wrap-up

Nested queries, or subqueries, are a powerful tool in SQL that allows for complex queries to be written by breaking them down into smaller, more manageable queries. Subqueries can be used in various parts of a query, including `SELECT`, `WHERE`, and `HAVING` clauses. By understanding how to use subqueries in SQL, you can write more complex queries and retrieve more specific data from a database.

Here's a summary of what we've learned about nested queries in SQL:

- A nested query, also known as a subquery, is a query that is nested inside another query.
- Subqueries can be used in various parts of a query, such as SELECT, WHERE, and HAVING clauses.
- Subqueries are typically enclosed in parentheses and used in conjunction with an operator such as IN, EXISTS, or =.
- Subqueries can be used to retrieve data that will be used in the main query, allowing for complex queries that would be difficult to write using a single query.
- Examples of subqueries include retrieving data for employees who are currently managers, retrieving data for employees who were hired in the same year as a specific employee, and retrieving data for departments with the highest average salary.
- Understanding how to use subqueries in SQL can help you write more complex queries and retrieve more specific data from a database.


