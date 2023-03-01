# Updating and Deleting Data with Python

## Updating Data with Python and SQLAlchemy

### Using `update()` method

The update() method in SQLAlchemy can be used to update data in a table. The syntax of update() method is as follows:
```py
table.update().where(condition).values(column_name=new_value)
```
Where : 

- `table`: The table that needs to be updated.
- `condition`: The condition that specifies which rows should be updated.
- `column_name`: The name of the column that needs to be updated.
- `new_value`: The new value for the column.

Here's an example of how to use the update() method in SQLAlchemy to update the salary of an employee:

```py title="update_db.py"
from sqlalchemy import create_engine, Table, Column, Integer, String, MetaData, ForeignKey, desc

# create an engine to connect to the database
engine = create_engine('mysql://user:password@localhost:3306/employees')

# create a metadata object to reflect the database schema
metadata = MetaData()

# Define the table
employees = Table('employees', metadata, autoload=True, autoload_with=engine)

# Update the salary of employee with emp_no = 10001
update_query = employees.update().where(employees.c.emp_no == 10001).values(first_name='jonh')

# Execute the query
with engine.connect() as conn:
    conn.execute(update_query)
```
Here, the line `employees = Table('employees', metadata, autoload=True, autoload_with=engine)`  loads the `employees` table from the database into a Table object named `employees` using the metadata object and the engine object.
Then we define the update operation to be performed on the `employees` table. This query will update the `first_name` column of the row where `emp_no` is equal to 10001 to `john`.

Let's verify if the database is updated with this query after the `update_db.py` script : 

```py
# Verify if the field has been modified
select_query = select([employees]).where(employees.c.emp_no == 10001)

# Execute the query
with engine.connect() as conn:
    result = conn.execute(select_query)
    for row in result:
        print(row)
```

### Using `execute()` method

Another way to update data in a table is by using the `execute()` method in SQLAlchemy. Here's an example:

```py
# Define the table
employees = Table('employees', metadata, autoload=True, autoload_with=engine)

# Define the query
update_query = "UPDATE employees SET first_name = 'jonh' WHERE emp_no = 10001"

# Execute the query
with engine.connect() as conn:
    conn.execute(update_query)
```

#### 💡 `update()` VS `execute()`

The `execute()` method is more flexible and can be used to execute any SQL statement, including `SELECT`, `INSERT`, `UPDATE`, `DELETE`, and more. However, it requires the statement to be provided as a string, making it more error-prone and harder to maintain in large applications.

On the other hand, the `update()` method provides a more structured way to update data in a table. It takes a filter condition that specifies which rows to update, and a set of values to apply to the specified rows. The `update()` method also returns a `ResultProxy` object that can be used to get information about the update operation, such as the number of affected rows.

In general, the `update()` method is preferred for modifying data in a table using SQLAlchemy, as it provides a more structured and safer approach. However, in some cases, the `execute()` method may be more appropriate for executing complex SQL statements that cannot be expressed using the `update()` method.


## Deleting Data with Python and SQLAlchemy

### Using `delete()` method

The `delete()` method in SQLAlchemy can be used to delete data from a table. The syntax of `delete()` method is as follows:
```py
table.delete().where(condition)
```
where :
- **table**: The table that needs to be updated.
- **condition**: The condition that specifies which rows should be deleted.

Here's an example of how to use the delete() method in SQLAlchemy to delete an employee from the employees table:

```py title="delete_dd.py"
#
#import and other stuff like all the previous scripts
#

# Define the table
employees = Table('employees', metadata, autoload=True, autoload_with=engine)

# Delete the employee with emp_no = 10001
delete_query = employees.delete().where(employees.c.emp_no == 10001)

# Execute the query
with engine.connect() as conn:
    conn.execute(delete_query)
```

In this example, we deleted the employee with `emp_no` 10001 from the `employees` table.

### Using `execute()` method

Another way to delete data from a table is by using the execute() method in SQLAlchemy. Here's an example:

```py 
# Define the table
employees = Table('employees', metadata, autoload=True, autoload_with=engine)

# Define the query
delete_query = "DELETE FROM employees WHERE emp_no = 10001"

# Execute the query
with engine.connect() as conn:
    conn.execute(delete_query)
```
Same principle as before. 

## Wrap-up

That's it! Now we know how to update and delete data in tables using Python and SQLAlchemy 🥳



