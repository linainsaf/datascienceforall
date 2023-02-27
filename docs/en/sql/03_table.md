# Tables & Keys

In a database, a table is a collection of related data that is organized into rows and columns. Tables are the primary way to store data in a relational database management system (RDBMS). In order to organize data effectively, tables are structured with columns that define the data that can be stored in each row.

## Keys 

Keys are an important concept in database design. They are used to ensure data integrity and to establish relationships between tables. There are several types of keys in a database:

* Primary Key: A primary key is a column or set of columns that uniquely identifies each row in a table. The primary key is used to enforce data integrity and to ensure that there are no duplicate rows in the table.
* Foreign Key: A foreign key is a column or set of columns that refers to the primary key of another table. It is used to establish a relationship between two tables.
* Composite Key: A composite key is a combination of two or more columns that together uniquely identify each row in a table.

### Examples of social security number

* Primary Key: A primary key is a unique identifier for a record in a table. It is used to ensure that each record in the table is unique and can be easily identified. For example, a person's social security number (SSN) can be used as a primary key in a table of customer data to ensure that each customer is unique and can be easily searched for.
* Foreign Key: A foreign key is used to link two tables together in a relational database. It is a field in one table that refers to the primary key in another table. For example, a customer's order history can be linked to their customer record using a foreign key. The foreign key in the order history table would refer to the primary key in the customer table, allowing for easy retrieval of all orders associated with a particular customer.
* Composite Key: A composite key is a key that consists of more than one field. It is used to ensure that a combination of fields in a record is unique. For example, in a table of product inventory, a combination of the product name and the manufacturer's part number could be used as a composite key to ensure that each product is unique and can be easily searched for.

### Examples of social media database 

* Primary Key: In a social media website's database, each user's unique username or email address can be used as a primary key to identify and manage their account. This ensures that each user has a unique identifier, making it easy for the website to maintain user data, track user activity, and provide personalized content.
* Foreign Key: In a hospital's database, a patient's medical record can be linked to their lab results using a foreign key. The foreign key would refer to the primary key in a table of lab test results, allowing doctors to easily access each patient's test results and track their medical history.
* Composite Key: In a hotel's database, a room reservation can be linked to a specific guest's booking using a composite key consisting of the guest's name and reservation number. This ensures that each guest has a unique reservation and allows the hotel to easily track each guest's room preference and check-in/check-out dates.

## Creating Tables

Creating a table is the first step in building a database. The syntax for creating a table varies depending on the specific database management system being used. Here's an example of how to create a simple table with a primary key:

```sql
CREATE TABLE employees (
  id INT PRIMARY KEY,
  first_name VARCHAR(50),
  last_name VARCHAR(50),
  age INT
);
```


In this example, we have created a table called employees with four columns: `id`, `first_name`, `last_name`, and `age`. The id column is designated as the primary key for the table.

To create a table with a foreign key, we first need to create the primary key in the table it will reference, and then add the foreign key to the table we're creating. Here's an example:

```sql
CREATE TABLE departments (
  id INT PRIMARY KEY,
  name VARCHAR(50)
);

CREATE TABLE employees (
  id INT PRIMARY KEY,
  first_name VARCHAR(50),
  last_name VARCHAR(50),
  age INT,
  department_id INT,
  FOREIGN KEY (department_id) REFERENCES departments(id)
);

```

In this example, we have created two tables - `employees` and `departments`. The `departments` table has two columns - `id` and `name`, with `id` designated as the primary key. The `employees` table has five columns - `id`, `first_name`, `last_name`, `age`, and `department_id`. The `id` column is the primary key, and the `department_id` column is a foreign key that references the `id` column in the `departments` table.

By using tables and keys, we can organize data effectively and establish relationships between tables. This allows us to build powerful and flexible databases that can store and manipulate large amounts of data.

Don't worry about understunding the syntaxe at this point we will explain more later 🤓
