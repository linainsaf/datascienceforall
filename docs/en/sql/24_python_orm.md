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

- Define the database schema using Python classes or connecting an existing database.
- Create an engine that connects to the database.
- Define a session factory that will be used to create sessions.
- Use the session factory to create a session.
- Use the session to perform **CRUD (Create, Read, Update, Delete) operations** on the database.


Here is an example of how to use SQLAlchemy for ORM and perform a query with the SQLAlchemy Query API :
```py title="orm_0.py"
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

# create an engine to connect to the database
engine = create_engine('mysql://root:root@localhost:3306/employees')

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


## Wrap-up 

Whether you prefer to work with SQL directly or use an ORM, SQLalchemy has you covered. With its intuitive API, SQLalchemy makes it easy to connect to databases, perform queries, and manipulate data. Understanding how to use SQLalchemy is an essential skill for any Python developer working with databases.