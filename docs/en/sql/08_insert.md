# Inserting Data

Once you have created a table, you can start inserting data into it. The process of inserting data involves specifying the table name, the columns to insert data into, and the values to be inserted.

## Overview of Different Insert Commands and Syntax

There are a few different ways to insert data into a table in SQL, depending on how much information you have about the data you are inserting. Here are some common insert commands and their syntax:

### Inserting Values into Specific Columns

You can use the `INSERT INTO` command to insert values into specific columns in a table. Here is the syntax for inserting a single row of data into a table:

```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

For example, to insert a new row into a `users` table with the values "John Doe" for the `name` column, "johndoe@example.com" for the `email` column, and 25 for the `age` column, you would use the following command:

```sql
INSERT INTO users (name, email, age)
VALUES ('John Doe', 'johndoe@example.com', 25);
```

### Inserting Values into All Columns

If you have data to insert for every column in a table, you can omit the column names from the INSERT INTO command. Here is the syntax for inserting a single row of data into a table without specifying column names:

```sql
INSERT INTO table_name
VALUES (value1, value2, value3, ...);
```

For example, to insert a new row into the `users` table with the values "Jane Smith" for the `name` column, "janesmith@example.com" for the `email` column, 30 for the `age` column, and "female" for the `gender` column, you would use the following command:

```sql
INSERT INTO users
VALUES ('Jane Smith', 'janesmith@example.com', 30, 'female');

```

### Inserting Multiple Rows at Once

You can also use the `INSERT INTO` command to insert multiple rows of data at once. Here is the syntax for inserting multiple rows of data into a table:

```sql
INSERT INTO table_name (column1, column2, column3, ...)
VALUES
  (value1, value2, value3, ...),
  (value1, value2, value3, ...),
  (value1, value2, value3, ...),
  ...;
```

For example, to insert three new rows into the `users` table, you would use the following command:

```sql
INSERT INTO users (name, email, age)
VALUES
  ('Alice Johnson', 'alicejohnson@example.com', 35),
  ('Bob Williams', 'bobwilliams@example.com', 40),
  ('Charlie Brown', 'charliebrown@example.com', 45);
```


By using these different insert commands and syntax, you can efficiently add data to your SQL database tables.


