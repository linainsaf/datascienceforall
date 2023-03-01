# Object Relational Mapping Python  

## What is ORM?

ORM stands for Object-Relational Mapping, which is a programming technique that allows developers to work with a relational database using an object-oriented paradigm. It maps the data stored in a database to objects in a programming language, which makes it easier for developers to work with data in their application.

![Screenshot](https://i.stack.imgur.com/seqRv.jpg)

Traditionally, when working with a relational database, developers use SQL to write queries and retrieve data. SQL is a powerful language, but it can be difficult to work with and maintain, especially for large and complex systems. ORM provides a layer of abstraction between the database and the application, allowing developers to work with objects instead of raw SQL.

### ORM benefits for developers

- Simplification of code: With ORM, developers can work with objects and classes instead of complex SQL queries. This makes the code more readable and maintainable.
- Portability: ORM allows the same code to work with different databases, as it provides a common interface to access the data.
- Database abstraction: ORM shields developers from the complexities of different databases and database-specific SQL syntax, allowing them to focus on the application logic.
- Security: ORM provides built-in protection against SQL injection attacks, as it automatically sanitizes input and uses parameterized queries.
- Increased productivity: With ORM, developers can write code faster, as they don't have to write complex SQL queries manually.

## Why SQLAlchemy

SQLalchemy ORM (Object-Relational Mapping) is another way of working with databases using SQLalchemy. It supports a wide range of relational databases, including MySQL, PostgreSQL, SQLite, and Oracle.
**In ORM, database tables are represented as classes and rows in tables are represented as objects of those classes.**

SQLAlchemy provides several key features:

- Declarative base classes: SQLAlchemy allows developers to define their database schema using Python classes. This makes it easy to map tables to classes, and provides a clear separation between the database and application logic.
- Session management: SQLAlchemy provides a session management system that tracks changes made to objects and commits them to the database.
- Query API: SQLAlchemy provides a powerful and flexible query API that allows developers to construct complex queries using Python syntax. Like the `filter()` function we have seen in the previous course. 
- Support for transactions: SQLAlchemy provides support for transactions, allowing developers to roll back changes if necessary.
- Built-in caching: SQLAlchemy provides built-in caching to improve performance.
Support for migrations: SQLAlchemy provides support for database migrations, allowing developers to change the schema of their database over time.

## How to use SQLAlchemy for ORM

Using SQLAlchemy for ORM involves several steps:

- Define the database schema using Python classes. 
- Create an engine that connects to the database.
- Define a session factory that will be used to create sessions.
- Use the session factory to create a session.
- Use the session to perform **CRUD (Create, Read, Update, Delete) operations** on the database.


Here is an example of how to use SQLAlchemy for ORM and perform a query with the SQLAlchemy Query API :
```py title="orm_0.py"
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

# create an engine to connect to the database
engine = create_engine('mysql://user:password@localhost:3306/employees')

# create a session to work with the database
Session = sessionmaker(bind=engine)
session = Session()

# import the Employee model
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column, Integer, String, Date, Enum

Base = declarative_base()

class Employee(Base):
    __tablename__ = 'employees'
    emp_no = Column(Integer, primary_key=True)
    birth_date = Column(Date)
    first_name = Column(String(14))
    last_name = Column(String(16))
    gender = Column(Enum('M','F'))
    hire_date = Column(Date)

# query the database using the ORM
employees = session.query(Employee).filter(Employee.gender == 'M').limit(10).all()

# print the results
for employee in employees:
    print(employee.emp_no, employee.first_name, employee.last_name)

# close the session
session.close()
```
In this example, we define an ORM class Employee that represents the employees table in the database. We define the columns of the table as attributes of the class.

To query the database, we create a session using sessionmaker and use the query method to perform a query. We filter the query to only include employees with gender 'M' and limit the results to 10 rows. We then iterate over the results and print them.

The main advantage of ORM is that it allows developers to work with databases in a more object-oriented way. They can use the familiar syntax of Python classes and objects to interact with databases. It also allows for better organization and abstraction of database code.

SQLalchemy provides a powerful ORM that allows developers to work with databases in a high-level, Pythonic way. The ORM provides an abstraction layer between the Python code and the database, making it easier to work with databases without needing to know SQL.


## Mapping the employees database

```py title="orm_1.py"
from sqlalchemy import create_engine, Table, Column, Integer, String, MetaData, ForeignKey

# create an engine to connect to the database
engine = create_engine('mysql://user:password@localhost:3306/employees')

# create a metadata object to reflect the database schema
metadata = MetaData()

# define the departments table
departments = Table('departments', metadata,
    Column('dept_no', String(4), primary_key=True),
    Column('dept_name', String(40))
)

# define the dept_emp table
dept_emp = Table('dept_emp', metadata,
    Column('emp_no', Integer, ForeignKey('employees.emp_no')),
    Column('dept_no', String(4), ForeignKey('departments.dept_no')),
    Column('from_date', String(10)),
    Column('to_date', String(10))
)

# define the dept_manager table
dept_manager = Table('dept_manager', metadata,
    Column('emp_no', Integer, ForeignKey('employees.emp_no')),
    Column('dept_no', String(4), ForeignKey('departments.dept_no')),
    Column('from_date', String(10)),
    Column('to_date', String(10))
)

# define the employees table
employees = Table('employees', metadata,
    Column('emp_no', Integer, primary_key=True),
    Column('birth_date', String(10)),
    Column('first_name', String(14)),
    Column('last_name', String(16)),
    Column('gender', String(1)),
    Column('hire_date', String(10))
)

# define the salaries table
salaries = Table('salaries', metadata,
    Column('emp_no', Integer, ForeignKey('employees.emp_no')),
    Column('salary', Integer),
    Column('from_date', String(10)),
    Column('to_date', String(10))
)

# define the titles table
titles = Table('titles', metadata,
    Column('emp_no', Integer, ForeignKey('employees.emp_no')),
    Column('title', String(50)),
    Column('from_date', String(10)),
    Column('to_date', String(10))
)

# create a session to interact with the database
from sqlalchemy.orm import sessionmaker
Session = sessionmaker(bind=engine)
session = Session()

# example query to select all employees
result = session.query(employees).all()
for row in result:
    print(row.emp_no, row.first_name, row.last_name)

# close the session
session.close()
```

In this example, we first create an `engine` to connect to the database using the `create_engine` function. Then we define the tables in the database using the `Table` and `Column` objects from the sqlalchemy module, and create a metadata object to reflect the database schema.

We then create a session to interact with the database using the `sessionmaker` function, and use the `query` method of the session object to execute a `SELECT` statement on the employees table, selecting all rows and printing : *the employee number, first name, and last name.*

Finally, we close the session using the close method. We can refactor our code like this : 

```py title="orm_2.py" 
from sqlalchemy import create_engine, Table, Column, Integer, String, MetaData, ForeignKey

# create an engine to connect to the database
engine = create_engine('mysql://user:password@localhost:3306/employees')

# create a metadata object to reflect the database schema
metadata = MetaData()

# create a session to interact with the database
from sqlalchemy.orm import sessionmaker
Session = sessionmaker(bind=engine)
session = Session()

#
# define tables like in orm_1.py script  
#

# example query 
try:
    result = session.query(employees, salaries).filter(employees.c.emp_no == salaries.c.emp_no).limit(10)
    for row in result:
        print(row)
finally:
    session.close()
```
Let's dig into this code 

- First, we import the necessary modules, including create_engine, Table, Column, Integer, String, MetaData, ForeignKey, and sessionmaker.
- Next, we create an engine to connect to our MySQL database, using the create_engine function.
- We then create a session to work with the database using sessionmaker.
- We define metadata using MetaData.
- We define the employees and salaries tables using Table and Column.
- We join the two tables using the filter function, specifying the emp_no columns to join on.
- We limit the results to 10 using the limit function.
- Finally, we print the results of the query.

This code joins the `employees` and `salaries` tables on their respective `emp_no` columns and returns the first 10 results. The result is a list of tuples, with each tuple containing the corresponding rows from the two tables.

### Perform queries

Let's take a look at the top 5 highest paid employees : 

```py title="orm_3.py"
from sqlalchemy import create_engine, Table, Column, Integer, String, MetaData, ForeignKey

# create an engine to connect to the database
engine = create_engine('mysql://user:password@localhost:3306/employees')

# create a metadata object to reflect the database schema
metadata = MetaData()

# create a session to interact with the database
from sqlalchemy.orm import sessionmaker
Session = sessionmaker(bind=engine)
session = Session()

#
# define tables like in orm_1.py script  
#

# example query 
try:
    result = (
    session.query(employees, salaries)\
    .filter(employees.c.emp_no == salaries.c.emp_no)\
    .order_by(desc(salaries.c.salary))\
    .limit(5)
)
    for row in result:
        print(f'Employee {row.first_name}-{row.last_name} earn {row.salary}$/year')
        #print(f"{employee.first_name} earns {salary.salary} dollars")
finally:
    session.close()
```

Here we've added an `order_by()` method to sort the results in descending order by the salary column. The `desc()` function is used to specify a descending sort. Finally, we use the `limit()` method to limit the results to the top 10 highest paid employees.

Let's take a look to an other query, this time without using `session()` and ORM mapping. 

### Using SQLAlchemy `Table` object to join data 

Let's take a look at SQLAlchemy join and why this method has several benefits over manual SQL  joins:

- **Abstraction**: SQLAlchemy provides a high-level object-oriented abstraction layer over SQL, making it easier to write queries and perform joins without needing to write low-level SQL code.
- **Portability**: Because SQLAlchemy provides a layer of abstraction, it makes it easier to switch databases without having to re-write queries. This is especially useful for larger projects where databases may need to be switched due to scaling or other requirements.
- **Security**: SQLAlchemy's query system provides a safe and secure way to build complex queries, helping to prevent SQL injection attacks.
- **Easier to read and maintain**: SQLAlchemy queries are often easier to read and maintain than raw SQL queries. This is because they are written in Python, which is a more expressive and easier to read language than SQL.
- **Object-Relational Mapping (ORM)**: SQLAlchemy provides an ORM that maps database tables to Python classes, making it easy to work with data in an object-oriented manner. This can simplify code and make it easier to reason about the data model.

Overall, using SQLAlchemy to join data provides a more efficient and effective way to interact with databases, reducing the likelihood of errors and making it easier to maintain code in the long-term.


#### Using `Table()`

The primary difference between using `Table` and the ORM approach in SQLAlchemy is how the data is represented and accessed.

When using `Table`, the data is represented as tables and columns in the database. Queries are constructed using SQL-like syntax, and the results are returned as tuples or dictionaries. This approach requires a good understanding of SQL and database structure, as well as the ability to write complex queries.

```py title="join_query.py"
from sqlalchemy import create_engine, MetaData, Table, select

# Define the engine
engine = create_engine('mysql://user:password@localhost:3306/employees')

# Define the metadata
metadata = MetaData()

# Define the tables
employees = Table('employees', metadata, autoload=True, autoload_with=engine)
dept_emp = Table('dept_emp', metadata, autoload=True, autoload_with=engine)
departments = Table('departments', metadata, autoload=True, autoload_with=engine)

# Define the query
query = select([employees.c.emp_no, employees.c.first_name, employees.c.last_name, departments.c.dept_name]).\
    select_from(employees.join(dept_emp).join(departments, dept_emp.c.dept_no == departments.c.dept_no))

# Execute the query
result = engine.execute(query)

# Print the results
for row in result:
    print(row)
```

This query selects the `emp_no`, `first_name`, `last_name`, and `dept_name` columns from the `employees`, `dept_emp`, and `departments` tables, and joins them together based on the `dept_no` column in `dept_emp` and `departments`. It then executes the query and prints the results.

#### ORM or `Table`

On the other hand, the ORM approach in SQLAlchemy allows developers to work with Python classes that represent database tables, and instances of those classes represent rows in the database. Queries are constructed using Python methods and attributes, which are then translated to SQL statements by SQLAlchemy. The results are returned as instances of the corresponding Python classes, making it easier to work with the data in a more object-oriented way.

Overall, the ORM approach is more intuitive for developers who are more familiar with object-oriented programming and less familiar with SQL. It also provides better abstraction from the underlying database structure, making it easier to make changes to the schema without affecting the application code. However, the Table approach may be more appropriate in cases where more fine-grained control over the SQL queries is required.

## Wrap-up 

Whether you prefer to work with SQL directly or use an ORM, SQLalchemy has you covered. With its intuitive API, SQLalchemy makes it easy to connect to databases, perform queries, and manipulate data. Understanding how to use SQLalchemy is an essential skill for any Python developer working with databases.

