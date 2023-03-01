# Querying Data with Python

In this section we will see how to use SQLAlchemy for connecting MySQL database and play with the data. 

## Querying an existing database 

Let's write a python script `query_db.py` who connects to a MySQL database using SQLAlchemy library, select a table and perform a query. 

```py title="query_db.py" hl_lines="14 20 21 22"
from sqlalchemy import create_engine, inspect

# create an engine to connect to the database
engine = create_engine('mysql://user:password@localhost:3306/employees')

# create an inspector to get metadata about the database
inspector = inspect(engine)

# get a list of all the table names in the database
table_names = inspector.get_table_names()

# iterate over the table, select employees table and perform a query
for table_name in table_names:
    if table_name == 'employees':
        columns = inspector.get_columns(table_name)
        print(f"Table: {table_name}")
        for column in columns:
            print(f"{column['name']}: {column['type']}")
        print("\n")
        employees = engine.execute("SELECT * FROM employees WHERE gender = 'M' LIMIT 10")
        for row in employees:
            print(row)
```


The only difference with the previous code `connect_db.py` is the highlight part when we iterate over each table name in the list and checks if the table name is `employees`. If it is, it gets the columns for the `employees` table and prints out each column name and data type.

Next, the code executes a SQL query to select the first 10 male employees in the `employees` table and prints out each row returned by the query.

Overall, this code demonstrates how to use SQLAlchemy to connect to a MySQL database, inspect its metadata, and execute SQL queries to retrieve data from a specific table.


## Using `session()` object

We don't necessarily need to use a session object if we only need to execute simple queries, as we can execute queries directly using the engine object. 

However, if you need to work with transactions or make more complex database operations, using a session object would be a good practice. A session object provides a transactional scope and also allows you to perform more complex operations such as creating, updating or deleting records.

Let's rewrite the query above with a `session()` object : 

```py title="query_db_session.py" hl_lines="2 5 8 9 12 17"
from sqlalchemy import create_engine, inspect
from sqlalchemy.orm import sessionmaker

# create an engine to connect to the database
engine = create_engine('mysql://user:password@localhost:3306/employees')

# create a session to work with the database
Session = sessionmaker(bind=engine)
session = Session()

# perform a query to select customers from London
result = session.execute("SELECT * FROM employees WHERE gender = 'M' LIMIT 10")
for row in result:
    print(row)

# close the session
session.close()
```
Note that in this example, we first create a session object using the sessionmaker function and passing the engine object as an argument. Then we can use the session object to execute the query and work with the results. Finally, we close the session to release resources.

### Pro and Con using `session()`

Goods:

- Sessions help manage transactions by keeping track of the state of the database and the changes made to it. This helps ensure that changes are made in a consistent and safe way.
- Sessions can help improve performance by batching together changes made to the database and reducing the number of round-trips to the database.
- Sessions can provide an additional layer of abstraction that makes it easier to work with the database in a more object-oriented way.
- Sessions can be used to automatically generate SQL statements to perform CRUD operations on the database, reducing the amount of SQL code that needs to be written.

Bads:

- Sessions can add complexity to the code and make it more difficult to understand and maintain.
Sessions can be prone to race conditions and deadlocks, especially in high-concurrency environments.
- Sessions can sometimes make it more difficult to diagnose and debug issues with the database.
- Sessions can sometimes lead to inconsistent or unexpected behavior if not used correctly or if there are bugs in the code.

## Using `with session()` 

Using the with statement to automatically close the session after the execution of the query : 

```py title="query_db_session_with.py" hl_lines="11"
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

# create an engine to connect to the database
engine = create_engine('mysql://user:password@localhost:3306/employees')

# create a session to work with the database
Session = sessionmaker(bind=engine)

# perform a query to select customers from London
with Session() as session:
    result = session.execute("SELECT * FROM employees WHERE gender = 'M' LIMIT 10")
    for row in result:
        print(row)
```


## Using `query()` 

Using the query method of the session:

```py title="query_db_session_query.py" hl_lines="2 10"
from sqlalchemy import create_engine
from sqlalchemy.orm import Session

# create an engine to connect to the database
engine = create_engine('mysql://root:root@localhost:3306/employees')

# create a session to work with the database
with Session(engine) as session:
    # perform a query to select employees with a salary greater than 100000
    result = session.query(Employees).filter(Employees.salary > 100000).limit(10)
    for row in result:
        print(row.first_name, row.last_name, row.salary)

```
In this example, we use the with statement to automatically close the session when we're done with it. We also use the query method to execute the SQL statement, and the filter method to filter the results based on the salary column. Finally, we use the limit method to limit the number of rows returned to 10.

Note that in order to use this method, **we first need to define the Employees class using the declarative_base function from SQLAlchemy.** This allows us to represent the employees table as a Python class, making it easier to query the database using SQLAlchemy we will see that in the next chapter. 
