# Constraints

Constraints are rules that you can apply to a table in a database to enforce data integrity. They play a vital role in ensuring that the data within the database remains consistent and accurate. There are various types of constraints that can be applied to a table, each serving a specific purpose.

## Types of constraints 

One type of constraint is the primary key constraint, which enforces the uniqueness of a column or a group of columns within a table. Another type of constraint is the foreign key constraint, which establishes a relationship between two tables based on the values of their respective columns.

<br />

Other types of constraints include the NOT NULL constraint, which ensures that a column cannot have a NULL value, and the UNIQUE constraint, which ensures that the values in a column are unique.

## Examples of customers table one without constraints and the other with constraints

### Without Constraints

```sql
CREATE TABLE customers (
    id INT,
    first_name VARCHAR(50),
    last_name VARCHAR(50),
    email VARCHAR(100)
);

INSERT INTO customers (id, first_name, last_name, email)
VALUES
    (1, 'John', 'Doe', 'john.doe@example.com'),
    (2, 'Jane', 'Doe', 'jane.doe@example.com'),
    (3, 'Bob', 'Smith', 'bob.smith@example.com'),
    (4, 'Alice', 'Johnson', 'alice.johnson@example.com');

```

In this example, we create a table named `customers` with four columns - `id`, `first_name`, `last_name`, and `email`. We then insert some sample data into the table. However, there are no constraints set on the table to enforce any rules about the data being inserted. For example, we can insert multiple rows with the same `id` value, which can lead to inconsistencies in the data.


### With Constraints

```sql
CREATE TABLE customers (
    id INT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE
);

INSERT INTO customers (id, first_name, last_name, email)
VALUES
    (1, 'John', 'Doe', 'john.doe@example.com'),
    (2, 'Jane', 'Doe', 'jane.doe@example.com'),
    (3, 'Bob', 'Smith', 'bob.smith@example.com'),
    (4, 'Alice', 'Johnson', 'alice.johnson@example.com');

INSERT INTO customers (id, first_name, last_name, email)
VALUES
    (1, 'Mark', 'Smith', 'mark.smith@example.com'); -- This will fail due to duplicate primary key constraint

INSERT INTO customers (id, first_name, last_name, email)
VALUES
    (5, 'Sam', 'Jones', 'bob.smith@example.com'); -- This will fail due to unique constraint on email column

```

In this example, we create the same customers table, but with additional constraints. We set the id column as the primary key, which means that it must be unique for each row. We also set the first_name and last_name columns as NOT NULL, which means that they cannot be empty. Finally, we set the email column as UNIQUE, which means that each email must be unique in the table.

<br />

When we try to insert data into the table, the constraints are enforced. The first INSERT statement will work fine because it does not violate any constraints. However, the second INSERT statement will fail because it tries to insert a row with a duplicate id value, which violates the primary key constraint. Similarly, the third INSERT statement will fail because it tries to insert a row with a duplicate email value, which violates the unique constraint on the email column.

