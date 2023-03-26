# Python and SQLAlchemy 

SQLAlchemy is an open-source library for Python that provides a way to work with relational databases. It allows you to interact with databases using a high-level object-oriented interface, rather than writing SQL code directly. 

## Explanation of What SQLAlchemy is and Its Benefits for Working with Databases

SQLAlchemy provides several benefits for working with databases, including:

- **Abstraction**: SQLAlchemy provides a layer of abstraction between the application and the database, which makes it easier to switch between different databases without changing the application code.
- **Flexibility**: SQLAlchemy allows you to work with databases using an object-oriented paradigm, which can be more flexible than using SQL directly.
- **Portability**: SQLAlchemy is a Python library, which means it can be used on any platform that supports Python, including Windows, Mac, and Linux.
- **Security**: SQLAlchemy includes built-in security features, such as support for parameterized queries, which can help protect against SQL injection attacks.

## Overview of SQLAlchemy Concepts and How They Relate to SQL

SQLAlchemy provides several concepts that are used to work with databases, including:

- **Engine**: The Engine is the entry point for working with a database. It provides a way to connect to the database and execute SQL commands.
- **Connection**: A Connection is an instance of a connection to the database. It allows you to execute SQL commands and manage transactions.
- **Transaction**: A Transaction is a sequence of SQL commands that are executed as a single unit of work. Transactions provide a way to ensure that multiple SQL commands are executed atomically.
- **Session**: A Session is a high-level object that provides an interface for working with a database. It provides a way to manage database operations, such as creating, updating, and deleting records.
- **Model**: A Model is a Python class that represents a table in the database. It provides a way to interact with the data in the table using Python code.

## SQLAlchemy installation 

The first step is to install SQLAlchemy. You can install it using pip, the Python package manager. Open a terminal or command prompt and type the following command:
```bash
pip install sqlalchemy
```
This will download and install the latest version of SQLAlchemy. You can verify the installation with this command on your terminal : 
```bash
pip list | grep sqlalchemy
```

### Install a MySQL database connector

In order to connect to a MySQL database, you'll need to install a database connector (we will see later that we can connect multiple databases not only MySQL). There are several available, but the most popular one is `mysql-connector-python`. You can install it using pip:
```bash
pip install mysql-connector-python
```

### Verify the connection 
Now that you have both SQLAlchemy and the MySQL connector installed, you can connect to your MySQL database. First, import the necessary modules:
```py
from sqlalchemy import create_engine
import mysql.connector
```
Next, create an engine that connects to your MySQL database:
```py
engine = create_engine('mysql+mysqlconnector://user:password@host:port/database')
```
Replace `user` and `password` with your MySQL `username` and `password`, host with the hostname or IP address of your MySQL server, `port` with the `port` number (usually 3306), and database with the name of the database you want to connect to.

## Databases supports

SQLAlchemy supports a variety of different database connections, each with its own advantages and use cases. Here's an overview of some of the most common connection types with the `engine` object:

### SQLite
SQLite is a serverless, file-based database that is popular for small to medium-sized projects. SQLAlchemy provides a SQLite dialect that allows you to work with SQLite databases in Python. Since SQLite is a file-based database, it is relatively easy to set up and use, and is a good choice for applications that don't require a lot of concurrent users or high write volumes.

```py
from sqlalchemy import create_engine

engine = create_engine('sqlite:///mydatabase.db')
```
In this example, we use the create_engine() function to create an engine object that connects to a SQLite database file named mydatabase.db.


### MySQL
MySQL is a popular open-source relational database system that is commonly used for web applications. SQLAlchemy provides a MySQL dialect that allows you to work with MySQL databases in Python. MySQL is a good choice for applications that require scalability and performance, since it can handle high volumes of reads and writes.

```py
from sqlalchemy import create_engine

engine = create_engine('mysql+pymysql://username:password@host:port/database')
```
In this example, we use the create_engine() function to create an engine object that connects to a MySQL database named database running on a `host` with the specified `username`, `password`, `host`, and `port`. In our case we will be connection our `localhost`, so we can re write this command as follow : 
```py
engine = create_engine('mysql+mysqlconnector://user:password@localhost/database')
```

### Use MAMP SQL Server on Mac OS 
In case you have this error : 
```py
ModuleNotFoundError: No module named 'MySQLdb'
```

We have seen in the [chapter 5](http://192.168.0.71:5010/sql/05_MacOS_install/) how to install a SQL server with MAMP so we will leverage this installation for connecting our python code to MAMP SQL Server ! 
Follow these steps : 

1. Install the latest version of XCode & XCode developer tools.
2. Install `brew` package manager, here you can find the official [documentation](https://brew.sh/index_fr)
3. Install MySQL using this command: 
```
brew install mysql
```
4. Run this command into your terminal to make a async link between MySQL socket: 
```
sudo ln -s /Applications/MAMP/tmp/mysql/mysql.sock /tmp/mysql.sock
``` 
(your socket url could be different if you using are xampp or any other local server.)
5. Start MAMP/XAMMP MySQL server
6. Then install (or re-install) sqlalchemy etc.

We will not see in detail the other types of connectors for others databases, it's very similar as you can see bellow.  

### PostgreSQL
PostgreSQL is a powerful open-source relational database system that is known for its robustness and scalability. SQLAlchemy provides a PostgreSQL dialect that allows you to work with PostgreSQL databases in Python. PostgreSQL is a good choice for applications that require high performance, scalability, and data integrity.

```py
from sqlalchemy import create_engine

engine = create_engine('postgresql+psycopg2://username:password@host:port/database')
```
In this example, we use the create_engine() function to create an engine object that connects to a PostgreSQL database named database running on a host with the specified username, password, host, and port.

### Oracle
Oracle is a commercial relational database system that is popular in enterprise settings. SQLAlchemy provides an Oracle dialect that allows you to work with Oracle databases in Python. Oracle is a good choice for applications that require high performance, scalability, and data integrity, and can handle large amounts of data and concurrent users.

```py
from sqlalchemy import create_engine

engine = create_engine('oracle+cx_oracle://username:password@host:port/SID')
```
In this example, we use the create_engine() function to create an engine object that connects to an Oracle database identified by its SID, running on a host with the specified username, password, and port.

### Microsoft SQL Server
Microsoft SQL Server is a popular relational database system that is commonly used in Windows environments. SQLAlchemy provides a SQL Server dialect that allows you to work with SQL Server databases in Python. SQL Server is a good choice for applications that require high performance and scalability, and can handle large amounts of data and concurrent users.
```py
from sqlalchemy import create_engine

engine = create_engine('mssql+pymssql://username:password@host:port/database')
```
In this example, we use the create_engine() function to create an engine object that connects to a Microsoft SQL Server database named database running on a host with the specified username, password, host, and port.

As you can see, it is pretty much the same process. 

### Choosing the right Database 
Choosing the right database connection is an important consideration when working with SQLAlchemy. By understanding the strengths and weaknesses of different database systems and dialects, you can choose the best option for your project. SQLAlchemy provides a consistent, high-level interface to all of these systems, allowing you to work with them in a uniform way. With SQLAlchemy, you can write Python code that works with a wide range of databases, and easily switch between them as your needs change.


## Connect to our Employees database 

Let's write a python script `connect_db.py` who connects to our MySQL employees database using SQLAlchemy library and retrieves some data (or metadata) about the database tables. 

```py
from sqlalchemy import create_engine, inspect
host = '127.0.0.1'
db = 'employees'
# create an engine to connect to the database
print(f'--- Connecting database : mysql://user:password@{host}:3306/{db}---\n')

engine = create_engine(f'mysql://user:password@localhost:3306/{db}')
# create an inspector to get metadata about the database
inspector = inspect(engine)
print(f'--- ✅ Connection database OK ✅---')
# get a list of all the table names in the database
table_names = inspector.get_table_names()

print('Tables : \n\n')
# iterate over the table names and print the table schema
for table_name in table_names:
    columns = inspector.get_columns(table_name)
    print(f"Table: {table_name}")
    for column in columns:
        print(f"{column['name']}: {column['type']}")
    print("\n")

# explicitly close the connection to the database
engine.dispose()
```

Here is a step-by-step explanation of what the script does:

1. The script starts by importing the required modules from the SQLAlchemy library, including the create_engine and inspect functions.
2. The host and db variables are defined to store the connection details for the MySQL database. In this case, the host is set to 127.0.0.1 and the database name is employees.
- The create_engine function is used to create a connection to the MySQL database using the specified host, port, username, password, and database name. The connection details are passed as a string to the create_engine function, which returns an engine object.
- An inspector object is created using the inspect function and the engine object. The inspector is used to retrieve metadata about the database tables.
- A success message is printed to the console to indicate that the database connection was successful.
- The get_table_names function is called on the inspector object to retrieve a list of all table names in the database.
- A loop is then used to iterate over the table names and retrieve the column information for each table.
- Inside the loop, the get_columns function is called on the inspector object to retrieve the column details for the current table.
- The column details are then printed to the console, including the column name and data type.
- Once all tables have been processed, the loop ends and the script exits and the connection to the database is closed.

In summary, this script connects to a MySQL database, retrieves metadata about the tables, and displays the column details for each table. This is a useful way to quickly check the schema of a database and verify that it is set up correctly.

Do not forget to use you `username` and `password` in order to insure a good connection.  

### Closing the connection 

When a SQLAlchemy Engine is created, it creates a pool of connections to the database which are automatically checked out and returned when needed by the application. This means that once an Engine has been created, it can be used for the lifetime of the application without the need to explicitly close the connection or perform any other cleanup.

<br />

However, it's good practice to close the connection when you're done using it, especially in long-running applications, to ensure that database resources are properly released. To close the connection in SQLAlchemy, you can use the Engine.dispose() method, which will release all resources associated with the Engine.


