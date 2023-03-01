# Advanced Queries with Python

SQLAlchemy provides a wide range of advanced querying techniques that allow you to perform complex database queries using Python code. In this tutorial, we will cover how to use joins, subqueries, and other advanced SQL features with SQLAlchemy.

## Joins

Joins are used to combine data from two or more tables in a single query like we have seen in the SQL section. SQLAlchemy provides several ways to perform joins, including the `join()`, `outerjoin()`, and `select_from()` methods.

### Inner Join
An inner join returns only the rows that have matching values in both tables being joined. Here's an example:

```py title="inner_join.py"
from sqlalchemy import create_engine, Table, Column, Integer, String, MetaData
#connect database
engine = create_engine('mysql://user:password@localhost:3306/employees')
metadata = MetaData()

#Map employees tables employees and titles
employees = Table('employees', metadata, autoload=True, autoload_with=engine)
titles = Table('titles', metadata, autoload=True, autoload_with=engine)

#perform query
query = employees.join(titles, employees.c.emp_no == titles.c.emp_no)
result = engine.execute(query.select())
#print result
for row in result:
    print(row)
```
In the above example, we used the `join()` method to join the `employees` and `titles` tables on the `emp_no` column. The resulting query returns only the rows where there is a matching `emp_no` in both tables.

### Left Join
A left join returns all the rows from the left table and the matched rows from the right table. If there is no match in the right table, the result will contain NULL values. Here's an example:

```py title="left_join.py"
from sqlalchemy import create_engine, Table, Column, Integer, String, MetaData

engine = create_engine('mysql://user:password@localhost:3306/employees')
metadata = MetaData()
employees = Table('employees', metadata, autoload=True, autoload_with=engine)
salaries = Table('salaries', metadata, autoload=True, autoload_with=engine)

query = employees.join(salaries, employees.c.emp_no == salaries.c.emp_no, isouter=True)
result = engine.execute(query.select())

for row in result:
    print(row)
```
In the above example, we used the `join()` method to perform a `left join` between the `employees` and `salaries` tables on the `emp_no` column. The `isouter=True` argument specifies that we want to perform a `left join`. If there is no matching `emp_no` in the `salaries` table, the result will contain `NULL` values.

### Right Join

A `right join` returns all the rows from the right table and the matched rows from the left table. If there is no match in the left table, the result will contain `NULL` values. Here's an example:

```py title="right_join.py"
from sqlalchemy import create_engine, Table, Column, Integer, String, MetaData

engine = create_engine('mysql://user:password@localhost:3306/employees')
metadata = MetaData()
employees = Table('employees', metadata, autoload=True, autoload_with=engine)
salaries = Table('salaries', metadata, autoload=True, autoload_with=engine)

query = salaries.join(employees, salaries.c.emp_no == employees.c.emp_no, isouter=True)
result = engine.execute(query.select())

for row in result:
    print(row)
```
In the above example, we used the `join()` method to perform a right join between the `salaries` and `employees` tables on the `emp_no` column. The iso`uter=True` argument specifies that we want to perform a right join.

### Using multiple join conditions with `select_from()`

Let's try to print the 10 employees with the highest salary in the Development department : 

```py title="multiple_join.py"
from sqlalchemy import create_engine, Table, Column, Integer, String, MetaData, ForeignKey, desc, select, func

# create an engine to connect to the database
engine = create_engine('mysql://user:password@localhost:3306/employees')

# create a metadata object to reflect the database schema
metadata = MetaData()

# Define the tables
employees = Table('employees', metadata, autoload=True, autoload_with=engine)
departments = Table('departments', metadata, autoload=True, autoload_with=engine)
dept_emp = Table('dept_emp', metadata, autoload=True, autoload_with=engine)
salaries = Table('salaries', metadata, autoload=True, autoload_with=engine)

# Define the query to select the 10 employees with the highest salary in the Development department
query = select([employees.c.emp_no, employees.c.first_name, employees.c.last_name, salaries.c.salary]).\
        where(employees.c.emp_no == salaries.c.emp_no).\
        where(dept_emp.c.emp_no == employees.c.emp_no).\
        where(dept_emp.c.dept_no == departments.c.dept_no).\
        where(departments.c.dept_name == 'Development').\
        order_by(desc(salaries.c.salary)).\
        limit(10).\
        select_from(employees.join(salaries).join(dept_emp).join(departments))

# Execute the query
result = engine.execute(query)

# Print the results
for row in result:
    print(row)

```
The query selects the columns `emp_no`, `first_name`, `last_name`, and `salary` from the `employees` and `salaries` tables, respectively. It then joins the `dept_emp` and `departments` tables to get only the `employees` in the Development department, and sorts the result in descending order by salary, and limits the result to the top 10 highest paid `employees`. 

Finally, it uses the `select_from()` method to specify the join conditions between the tables.

The execute method is called on the engine object with the query object as an argument, and the results are printed in a loop as usual.


## Sub Queries and `correlate()` function 

In SQLAlchemy, the correlate() function is used to control correlated subqueries, which are subqueries that reference the outer query. A correlated subquery allows you to filter or aggregate data in a subquery based on the results of the outer query.

The correlate() function is used to specify which tables in the subquery should be correlated to the outer query. It takes one or more tables as arguments and returns a new Select object that is a correlated subquery.

Here's the same example as before modify for using `correlate()` function :

```py title="subquery_db.py"
from sqlalchemy import select, func
from sqlalchemy.orm import correlate

# define the tables
employees = Table('employees', metadata, autoload=True, autoload_with=engine)
salaries = Table('salaries', metadata, autoload=True, autoload_with=engine)
dept_emp = Table('dept_emp', metadata, autoload=True, autoload_with=engine)

# Define the subquery to calculate average salary of development department employees
subquery = select([func.avg(salaries.c.salary)]).\
    where(salaries.c.emp_no == employees.c.emp_no).\
    where(dept_emp.c.dept_no == 'd001').\
    correlate(employees).\
    select_from(dept_emp.join(employees, employees.c.emp_no == dept_emp.c.emp_no).\
                join(salaries, salaries.c.emp_no == employees.c.emp_no))

# Define the query to select the 10 most paid employees in development department
query = select([employees.c.emp_no, employees.c.first_name, employees.c.last_name, salaries.c.salary]).\
    where(dept_emp.c.dept_no == 'd001').\
    where(salaries.c.emp_no == employees.c.emp_no).\
    where(salaries.c.salary > subquery).\
    order_by(desc(salaries.c.salary)).\
    limit(10).\
    select_from(dept_emp.join(employees, employees.c.emp_no == dept_emp.c.emp_no).\
                join(salaries, salaries.c.emp_no == employees.c.emp_no))
# execute the query and print the results
result = engine.execute(query)

for row in result:
    print(row)
```
This query is similar to the previous one we discussed, which also selects the 10 most highly paid employees in the development department. However, in this query, we use the `correlate()` function to manage auto-correlation between tables.

The subquery is used to calculate the average salary of employees in the development department. We use the `func.avg()` function to calculate the average salary and apply two where clauses to filter the salaries of employees in the development department. 

To handle auto-correlation between tables, we use the `correlate()` function and pass the employees table as the argument. Finally, we join the `dept_emp`, `employees`, and `salaries` tables together using the `join()` method.

The main query selects the `emp_no`, `first_name`, `last_name`, and `salary` fields from the `employees` and `salaries` tables. We again use the where clause to filter the `employees` in the development department and use the `correlate()` function to handle auto-correlation. 
We also add a third where clause to compare the salaries of employees in the development department with the average salary calculated by the subquery. 

Finally, we join the `dept_emp`, `employees`, and `salaries` tables together using the `join()` method. We sort the results in descending order of salaries and limit the output to the top 10 results.

### Joins and `correlate()` 

The `correlate()` function is different from joins in that it allows you to filter or aggregate data in a subquery based on the results of the outer query, without actually joining the tables together. 

🔎 This can be more efficient than using `joins` in certain situations, especially when dealing with large datasets. However, correlated subqueries can also be slower than `joins`, especially if the subquery is complex or returns a large amount of data. It's important to test and optimize your queries to ensure that they perform well for your specific use case.

## Difference between the `select_from()` and the joins methods

The `select_from()` method and the `join()` method are both used in SQLAlchemy to specify the tables used in a SQL query, but they differ in how they are used and the types of queries they can generate.

The `join()` method is used to specify a join between two or more tables. It generates an `INNER JOIN` by default, but can be used to generate other types of joins as well. This method allows you to specify the join condition, i.e. the columns used to match rows between the tables. The `join()` method is useful when you need to combine data from multiple tables into a single query result.

The `select_from()` method, on the other hand, is used to specify the main table(s) used in a SQL query. It is used to specify the primary table or tables that the query will be based on. This method is useful when you need to specify a complex subquery or a nested query, or when you need to select from a view or other non-standard source of data.

In general, the `join()` method is used to combine data from multiple tables, while the `select_from()` method is used to specify the primary table or tables used in the query. However, both methods can be used together to generate more complex queries that combine data from multiple tables and use subqueries or nested queries to filter or manipulate the data.

## Working with `VIEW` 
Let's dig a little aroud SQL view concept. 


### What is a View in SQL?
A view in SQL is a virtual table that is created based on a query. It is a named query that is saved in the database and can be used like a table in other queries. A view can be used to simplify complex queries, filter data, or provide users with access to a subset of data in the database without having to grant access to the underlying tables.

### Creating a View
A view is created using the CREATE VIEW statement, which defines the name of the view, the columns to include, and the SELECT statement that defines the query used to create the view. Here's an example:

```sql
CREATE VIEW sales_report AS
SELECT year, month, sum(revenue) as total_revenue
FROM sales
GROUP BY year, month;
```
This creates a view called "sales_report" that contains three columns: year, month, and total_revenue. The view is based on the sales table, and calculates the total revenue for each month and year.

### Using a View
Once a view is created, it can be used like any other table in SQL. Here's an example:

```sql
SELECT * FROM sales_report WHERE year = 2022;
```
This query uses the "sales_report" view to select all records where the year is equal to 2022. The view simplifies the query by hiding the complexity of the underlying data.

### Updating a View
A view can be updated using the ALTER VIEW statement. Here's an example:
```sql
ALTER VIEW sales_report AS
SELECT year, month, sum(revenue) as total_revenue
FROM sales
WHERE year > 2020
GROUP BY year, month;
```
This updates the "sales_report" view to only include records where the year is greater than 2020.

### Dropping a View
A view can be dropped using the DROP VIEW statement. Here's an example:
```sql
DROP VIEW sales_report;
```
This deletes the "sales_report" view from the database.

### Importance of Views in Data Analysis Job

Views are important in day-to-day data analysis because they simplify complex queries and provide a way to control access to sensitive data. Views allow users to focus on the data that is important to them, without having to understand the underlying database schema or query language. Views can also be used to filter data, hide sensitive information, and provide access to a subset of data in the database.

In summary, views in SQL are a powerful tool for simplifying complex queries and providing users with access to a subset of data in the database. They are an important part of day-to-day data analysis and can help improve productivity, accuracy, and security.

### `VIEW` with SQLAlchemy

Let's create a python script call `view_0.py` with a view that shows the number of employees hired each year and just print the result. 

```py title="view_0.py"
from sqlalchemy import create_engine, MetaData, Table, Column, Integer, String, Date, select, text

# create an engine to connect to the database
engine = create_engine('mysql://user:password@localhost:3306/employees')

# create a metadata object to reflect the database schema
metadata = MetaData()

# define the tables to reflect the database schema
employees = Table('employees', metadata, autoload=True, autoload_with=engine)

# create a view that shows the number of employees hired each year
query = text("""
    CREATE VIEW employee_hires AS
    SELECT YEAR(hire_date) AS year, COUNT(*) AS hires
    FROM employees
    GROUP BY year;
""")

# execute the view creation query
with engine.connect() as conn:
    conn.execute(query)

# define a select statement to query the view
employee_hires_view = select([text("year, hires")]).select_from(text("employee_hires"))

# execute the select statement and print the results
with engine.connect() as conn:
    result = conn.execute(employee_hires_view)
    for row in result:
        print(row)
```

In this example, we create a view called employee_hires that shows the number of employees hired each year. We use the `text()` function from SQLAlchemy to define the SQL query for creating the view. Then, we execute the query using the `engine.connect()` method.

Next, we define a `select` statement that queries the `employee_hires view` using the `select_from()` method and the `text()` function to specify the table name. Finally, we execute the select statement and print the results using the `engine.connect()` method.

Note that the `select_from()` method is used to specify the table or view to select data from. In this example, we use the `text()` function to specify the name of the `employee_hires view`. This is similar to specifying a table name using the `Table()` function, but with the added benefit of being able to specify more complex SQL queries using the `text()` function.

Let's take a look at an other example with `VIEW` for select the 10 most paid employees in development department : 

```py title="view_1.py" hl_lines="15 16"
from sqlalchemy import create_engine, MetaData, Table, Column, Integer, String, Date, select, text, func

# create an engine to connect to the database
engine = create_engine('mysql://user:password@localhost:3306/employees')

# create a metadata object to reflect the database schema
metadata = MetaData()

# define the tables to reflect the database schema
employees = Table('employees', metadata, autoload=True, autoload_with=engine)
salaries = Table('salaries', metadata, autoload=True, autoload_with=engine)
dept_emp = Table('dept_emp', metadata, autoload=True, autoload_with=engine)

# drop the existing view if it exists
with engine.connect() as conn:
    conn.execute(text("DROP VIEW IF EXISTS employee_hires")) # (1)

# create a view that shows the number of employees hired each year
query = text("""
    CREATE VIEW employee_hires AS
    SELECT YEAR(hire_date) AS year, COUNT(*) AS hires
    FROM employees
    GROUP BY year;
""")

# execute the view creation query
with engine.connect() as conn:
    conn.execute(query)

# define the subquery to calculate average salary of development department employees
subquery = select([func.avg(salaries.c.salary)]).\
    where(salaries.c.emp_no == employees.c.emp_no).\
    where(dept_emp.c.dept_no == 'd001').\
    correlate(employees).\
    select_from(dept_emp.join(employees, employees.c.emp_no == dept_emp.c.emp_no).\
                join(salaries, salaries.c.emp_no == employees.c.emp_no))

# define the query to select the 10 most paid employees in development department
query = select([employees.c.emp_no, employees.c.first_name, employees.c.last_name, salaries.c.salary]).\
    where(dept_emp.c.dept_no == 'd001').\
    where(salaries.c.emp_no == employees.c.emp_no).\
    where(salaries.c.salary > subquery).\
    order_by(salaries.c.salary.desc()).\
    limit(10).\
    select_from(dept_emp.join(employees, employees.c.emp_no == dept_emp.c.emp_no).\
                join(salaries, salaries.c.emp_no == employees.c.emp_no))

# execute the query and print the results
with engine.connect() as conn:
    result = conn.execute(query)
    for row in result:
        print(row)
```


1.  🔎  You may have an error message indicates that the view `employee_hires` already exists in the database so we deleted it before re write it. You can either drop the existing view before creating it again, or modify the create statement to use `CREATE` OR `REPLACE VIEW` which will create the view if it does not exist, or replace the existing view if it does.

