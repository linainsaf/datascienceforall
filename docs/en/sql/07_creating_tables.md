# Creating Tables 

We said before, in SQL a table is a collection of data stored in rows and columns. To create a table, you need to define the table schema which includes the table name, column names, data types, and any constraints on the data. The CREATE TABLE statement is used to create a new table in a database.

## In-depth look at how to create tables

To create a new table in SQL, you use the `CREATE TABLE` command followed by the table name and a list of column definitions. Each column definition specifies the column name, data type, and any constraints on the data.

<br />

Here is an example of creating a simple table with two columns:

```sql
CREATE TABLE users (
  id INT PRIMARY KEY,
  name VARCHAR(50) NOT NULL
);
```

This creates a table named `users` with two columns: `id` and `name`. The `id` column is defined as an integer and is marked as the primary key for the table. The `name` column is defined as a variable-length string with a maximum length of 50 characters and is marked as required `(NOT NULL)`.

## Overview of different data types and how to use them

SQL provides a wide range of data types that can be used to define columns in a table. These data types include integers, floating-point numbers, strings, dates, and more. Here are some common data types:

* INT: used for integer values
* VARCHAR(n): used for variable-length character strings with a maximum length of n
* DATE: used for date values
* FLOAT: used for floating-point numbers
* BOOLEAN: used for boolean values

Here is an example of creating a table with columns of different data types:

```sql
CREATE TABLE products (
  id INT PRIMARY KEY,
  name VARCHAR(50) NOT NULL,
  price FLOAT,
  in_stock BOOLEAN,
  created_at DATE
);
```

### Add primary and foreign keys and link an other table 

In a relational database, a primary key is a unique identifier for each row in a table. It is used to ensure that each row can be uniquely identified and is commonly used to link to other tables in the database.

<br />

A foreign key is a column in one table that refers to the primary key of another table. This is used to create relationships between tables and enforce referential integrity.

<br />

Here is an example of creating a table with a primary key and a foreign key:

```sql
CREATE TABLE orders (
  id INT PRIMARY KEY,
  product_id INT,
  quantity INT,
  FOREIGN KEY (product_id) REFERENCES products(id)
);
```

This creates a table named orders with three columns: `id`, `product_id`, and `quantity`. The `id` column is defined as the primary key for the table. The `product_id` column is defined as a foreign key that references the id column in the `products` table, which creates a relationship between the two tables. Here, the `quantity` column is defined as an integer.

