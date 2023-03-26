# Basic Queries

In the world of database management, tables and queries go hand in hand. Queries are a fundamental component of any database, as they allow you to retrieve and manipulate data in meaningful ways. 

<br />

In this chapter, we will explore the basics of SQL queries and how they are used to extract data from tables. To demonstrate this, we will use two example tables, the `orders` and `customers` tables. 

<br />

These tables will be linked together using a foreign key to show how queries can retrieve data from multiple tables at once. Understanding queries and the relationship between tables is essential for effective database management, as it enables developers to extract valuable insights and make informed decisions based on data.

<br />

This is the example table called `customers`:

```sql
CREATE TABLE customers (
  id INT PRIMARY KEY,
  first_name VARCHAR(50),
  last_name VARCHAR(50),
  email VARCHAR(100),
  address VARCHAR(100),
  city VARCHAR(50),
  state VARCHAR(50),
  zip_code VARCHAR(20)
);
```

This table has columns for a customer's ID, first name, last name, email address, street address, city, state, and zip code. The `id` column is the primary key for the table, which means that each row in the table is uniquely identified by its value in the `id` column.

<br />

This is the example of an `orders` table:

```sql
CREATE TABLE orders (
  order_id INT PRIMARY KEY,
  customer_id INT,
  order_date DATE,
  total_price DECIMAL(10,2)
);
```


This table has four columns: `order_id`, `customer_id`, `order_date`, and `total_price`. **The `order_id` column is the primary key of the table, which means that each row has a unique value in that column.**

<br />

**The `customer_id` column is a foreign key that references the `customer_id` column in the `customers` table.** 

<br />

This establishes a relationship between the two tables.
The orders table contains information about each order placed by a customer. The `customer_id` column is used to link each order to a specific customer in the `customers` table. 

<br />

The `order_date` column contains the date that the order was placed, and the `total_price` column contains the total price of the order. **By joining the `orders` table with the `customers` table on the `customer_id` column, we can retrieve information about both the customer and their order in a single query.**

## Overview of Different SELECT Commands and Syntax

The `SELECT` statement has a variety of options for retrieving and manipulating data. Here are some examples:

* The `WHERE` clause is used to filter data based on a specified condition:
```sql
SELECT * FROM customers WHERE city = 'London';
```
This statement retrieves all columns and rows from the `customers` table where the city is `London`.

* The `ORDER BY` clause is used to sort data by one or more columns:
```sql
SELECT * FROM customers ORDER BY last_name;
```
This statement retrieves all columns and rows from the `customers` table, sorted by the `last_name` column.

* The `GROUP BY` clause is used to group data by one or more columns:
```sql
SELECT city, COUNT(*) FROM customers GROUP BY city;
```
This statement retrieves the `city` column and a count of how many times each `city` appears in the `customers` table.

* The `JOIN` command is used to combine data from two or more tables:
```sql
SELECT * FROM customers JOIN orders ON customers.customer_id = orders.customer_id;
```
This statement retrieves all columns and rows from both the `customers` and `orders` tables where the `customer_id` column matches in both tables.



