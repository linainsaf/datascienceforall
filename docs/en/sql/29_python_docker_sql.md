# SQLAlchemy and docker 

## Add an existing SQL database into a container 

In this section we will be looking at an existing database and put it into a docker container in order to query this database with a python script. 

<br />

Here's an example database let's call this script `init.sql`: 

```sql
CREATE DATABASE testdb;
USE testdb;

CREATE TABLE users (
  id INT NOT NULL AUTO_INCREMENT,
  name VARCHAR(50) NOT NULL,
  email VARCHAR(50) NOT NULL,
  PRIMARY KEY (id)
);

INSERT INTO users (name, email) VALUES
  ('John Doe', 'johndoe@example.com'),
  ('Jane Doe', 'janedoe@example.com'),
  ('Bob Smith', 'bobsmith@example.com');

GRANT ALL PRIVILEGES ON testdb.* TO 'user'@'%' IDENTIFIED BY 'user-password' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

This SQL script is used to create a new database called `testdb`, create a table called `users` with three columns `id`, `name`, and `email`, insert three records into the users table, and grant privileges to a user to access the testdb database.

### What is grant privileges 

The `GRANT ALL PRIVILEGES` command is used in MySQL to grant a user all possible privileges on a database or a specific table within a database. This command allows the user to perform any action on the specified database or table, including creating, modifying, and deleting data.

<br />

Here's a breakdown of the syntax of the `GRANT ALL PRIVILEGES` command:

```sql 
GRANT ALL PRIVILEGES ON database_name.* TO 'username'@'localhost' IDENTIFIED BY 'password';
FLUSH PRIVILEGES;
```

- `GRANT ALL PRIVILEGES` : This command grants all possible privileges to the user.
- `ON database_name.*` : This specifies the database and any tables within it that the user will have privileges on. The * wildcard character specifies all tables within the database.
- `TO 'username'@'localhost'` : This specifies the username and the host from which the user can connect to the database.
- `IDENTIFIED BY 'password'` : This specifies the password for the user.
- `FLUSH PRIVILEGES` : This line reloads the grant tables in the mysql database to apply the changes made by the GRANT command.

The `GRANT ALL PRIVILEGES` command is a powerful command that should be used with caution. It is recommended to only grant the necessary privileges to users to minimize the risk of data loss or security breaches.

**In our case this database is created in order to be connected by a python script and for that we must have a user, is it not recommended to connect the database as root user.**

### Write a Dockerfile with `init.sql` file 

Let's write a `Dockerfile` for our sql container : 

```Dockerfile 
FROM mysql:5.6

# set root password
ENV MYSQL_ROOT_PASSWORD=my-secret-pw

# create database and table
COPY init.sql /docker-entrypoint-initdb.d/

# add a new user
ENV MYSQL_USER=user
ENV MYSQL_PASSWORD=user-password

EXPOSE 3306
``` 

As you know in a `Dockerfile`, the `COPY` command is used to copy files or directories from the host machine into the Docker container.

<br />

In the context of our `Dockerfile`, the line  

```Dockerfile 
COPY init.sql /docker-entrypoint-initdb.d/ 
```
copies the `init.sql` file from the host machine into the `/docker-entrypoint-initdb.d/` directory within the Docker container.

<br />

**The `/docker-entrypoint-initdb.d/` directory is a default directory that is used by the mysql Docker image to automatically execute any SQL scripts that are located in this directory when the container is started up.**

<br />

By copying the `init.sql` file into the `/docker-entrypoint-initdb.d/` directory within the Docker container, you are instructing the mysql image to automatically execute this script when the container starts up. This allows you to automatically create our database, tables, and insert data into the database without having to manually execute SQL commands every time the container is started up.


### Run our MySQL container 

To build our custom MySQL image, you can run the following command in the directory containing the Dockerfile and `init.sql` script:

#### Build the image

```bash 
docker build -t my-mysql-image .
```
This command will build the Docker image using the Dockerfile in the current directory and tag it as `my-mysql-image:latest`.

#### Start the container

Once the image is built, you can start a new container using the following command:

```bash
docker run --name mysql-container -d -p 3306:3306 my-mysql-image
```
This command will start a new container (named `mysql-container`) using the custom MySQL image, now we can connect to our container in order to verify if all the options are passed. 

#### Connect to our container 

Go to your terminal and run this command in order to enter into the container : 
```bash
docker exec -it mysql-container /bin/bash
```
or the equivalent comnmand : 
```bash
docker exec -it <your-container-id> /bin/bash
```
Now that you are in the container we can access the MySQL CLI with the following command : 

```bash 
mysql -uuser -puser-password
```
You should see the following prompt :

```sql
mysql>
```
It means that you're currently in the MySQL Shell of the container. You can now view our `testdb` and `users` table with the command : 

```sql
mysql> show databases; 
```
you should see this output : 

```bash 
+--------------------+
| Database           |
+--------------------+
| information_schema |
| testdb             |
+--------------------+
2 rows in set (0.00 sec)
``` 
It means our `user` is able to see our database, you can also do a test query in order to verify the integrity of our database. In this section we have seen how to build a custom MySQL container with a database in it and how to access this database with the docker CLI 🚀


## Query a docker MySQL container with a python script 

Now, let's write a python script `connect_db.py` to connect our database 🥳

```python
from sqlalchemy import create_engine, MetaData, Table

# create engine to connect to MySQL
engine = create_engine('mysql://user:user-password@0.0.0.0:3306/testdb')

# create metadata object
metadata = MetaData()

# reflect the users table
users_table = Table('users', metadata, autoload=True, autoload_with=engine)

# select all rows from the users table
select_query = users_table.select()

# execute the query
with engine.connect() as conn:
    result = conn.execute(select_query)
    for row in result:
        print(row)

```

Let's break down the different parts of the script:

- `create_engine('mysql://root:my-secret-pw@0.0.0.0:3306/testdb')`: This creates a SQLAlchemy engine that connects to the MySQL database running in the Docker container. The username is "root", the password is "my-secret-pw", the host is "localhost", the port is "3306", and the database name is "testdb".
- `metadata = MetaData()` : This creates a metadata object that will be used to reflect the database schema.
- `users_table = Table('users', metadata, autoload=True, autoload_with=engine)` : This reflects the "users" table from the database using the metadata object.
- `select_query = users_table.select()` : This creates a query that selects all rows from the "users" table.
- `with engine.connect() as conn` : result = conn.execute(select_query): This creates a connection to the database using the engine, executes the select query, and stores the result in a variable.
- `for row in result: print(row)` : This loops through the result set and prints each row.

This script assumes that you have the necessary dependencies installed, including SQLAlchemy and the MySQL driver for Python. You can install these dependencies using `pip`:

```bash
pip install sqlalchemy pymysql
```
Note that the driver used to connect to the MySQL database is "pymysql", which is a Python MySQL client library that works with SQLAlchemy.

If you execute this script, you should see this output :
 
```bash 
(1, 'John Doe', 'johndoe@example.com')
(2, 'Jane Doe', 'janedoe@example.com')
(3, 'Bob Smith', 'bobsmith@example.com')
```

## Next steps 

In this section we covered a pretty simple database and python script so for the next steps if you want to upgrade this project you can consider : 

- Add FastAPI CRUD endpoints to interact with the database 
- Add a better database and `init.sql` script 
- Wrap-up the project with a `docker-compose.yml` file 
- Code a dashboard route in FastAPI in order to visualize some data of your database 
- Be creative 😃


## Wrap-up 

Let's summarize whant we have seen in this section :

- Docker provides a consistent and portable environment for running applications, including databases like MySQL.
- We can use Docker to run a MySQL database in a container by pulling the mysql image and specifying the appropriate environment variables and port mappings.
- We can use the mysql-connector-python library in Python to connect to a MySQL database and perform SQL queries.
- We can create a SQL script to create tables and insert data into the MySQL database, and include this script in the Docker image to automatically execute it when the container starts up.
- We have learned how to write a Dockerfile to define the configuration of a Docker container, and how to use it to build a Docker image.
- Overall, connecting Docker MySQL and Python provides a more efficient and reliable way to manage and run databases and applications, and reduces the complexity and time involved in setting up and maintaining software installations.


