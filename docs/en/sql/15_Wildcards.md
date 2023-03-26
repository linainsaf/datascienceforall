# Wildcards & Unions

## Wildcaards

Wildcards are special characters that are used in SQL to represent one or more characters in a string. They are used in conjunction with the LIKE operator to perform pattern matching on text values. Wildcards allow you to search for strings that match a specific pattern, even if you don't know the exact value of the string.

<br />

Wildcards are particularly useful when searching for records that have similar but not identical values in a column. For example, you can use a wildcard to find all employees with a first name that starts with the letter "J", or all employees with a last name that ends in "son".

## Overview of Different Wildcard Characters and Their Uses

There are three main wildcard characters in SQL: the percent sign (%), the underscore (\_), and the square brackets ([]). Each wildcard character serves a different purpose and can be used in different ways.

### The Percent Sign (%)

The percent sign is used to represent zero or more characters in a string. It can be used at the beginning, end, or in the middle of a search pattern.

<br />

For example, to find all employees with a first name that starts with the letter "J", you can use the following SQL query:

```sql
SELECT * FROM employees
WHERE first_name LIKE 'J%';
```

This query returns all employees whose first name starts with the letter "J". The % wildcard character is used to match any number of characters that come after the letter "J".

<br />

Similarly, to find all employees with a last name that ends in "son", you can use the following SQL query:

```sql
SELECT * FROM employees
WHERE last_name LIKE '%son';
```

This query returns all employees whose last name ends in the letters "son". The % wildcard character is used to match any number of characters that come before the letters "son".

### The Underscore (\_)

The underscore is used to represent a single character in a string. It can be used at the beginning, end, or in the middle of a search pattern.

<br />

For example, to find all employees with a first name that starts with the letter "J" and has a second letter that is an "o", you can use the following SQL query:

```sql
SELECT * FROM employees
WHERE first_name LIKE 'J_o%';
```

This query returns all employees whose first name starts with the letter "J", has a second letter that is an "o", and has any number of characters that come after the second letter.

### The Square Brackets ([])

The square brackets are used to represent a single character that can be any one of the characters specified within the brackets. For example, to find all employees with a first name that starts with the letters "J" or "P", you can use the following SQL query:

```sql
SELECT * FROM employees
WHERE first_name LIKE '[JP]%';
```

This query returns all employees whose first name starts with the letters "J" or "P". The square brackets are used to specify that the first letter can be either "J" or "P".

<br />

You can also use the square brackets to search for ranges of characters. For example, to find all employees with a last name that starts with the letters "M" to "Z", you can use the following SQL query:

```sql
SELECT * FROM employees
WHERE last_name LIKE '[M-Z]%';
```
This query returns all employees whose last name starts with any letter from "M" to "Z". The square brackets and the "-" symbol are used to specify the range of letters.

## Union

Union is a set operation in SQL that is used to combine the results of two or more `SELECT` statements into a single result set. Union returns a distinct set of rows that are present in either of the two or more `SELECT` statements. Union can be used to combine data from multiple tables, or to combine data from a single table that is split across multiple columns.

<br />

Union can be useful when you need to combine data from multiple sources or when you want to merge two or more tables with similar structure. Union can help simplify data analysis by providing a consolidated view of data that is spread across multiple sources.

## Overview of How to Use Union to Combine Data

The syntax for using Union in SQL is as follows:

```sql
SELECT column1, column2, ... FROM table1
UNION
SELECT column1, column2, ... FROM table2;
```

In this syntax, the first `SELECT` statement selects data from table1 and the second `SELECT` statement selects data from `table2`. The column names and data types of the columns in each `SELECT` statement must match.

Union combines the results of the two `SELECT` statements and removes any duplicate rows. The columns in the result set are determined by the columns in the first `SELECT` statement.

Here are a few examples of how to use Union to combine data:

### Example 1: Combine Data from Two Tables

Suppose you have two tables in the "employees" database: "sales" and "marketing". Both tables have the same structure and contain sales data for different regions. You can use Union to combine the sales data from both tables into a single result set:

```sql
SELECT region, sales_amount, year FROM sales
UNION
SELECT region, sales_amount, year FROM marketing;
```

This query combines the sales data from the "sales" and "marketing" tables and returns a result set that includes the region, sales amount, and year for each sale. Union removes any duplicate rows from the result set.

### Example 2: Combine Data from Multiple Columns

Suppose you have a table in the "employees" database that stores the names of employees in two columns: "first_name" and "last_name". You can use Union to combine the data from both columns into a single column:

```sql
SELECT first_name as name FROM employees
UNION
SELECT last_name as name FROM employees;
```
This query combines the data from the "first_name" and "last_name" columns into a single column called "name". Union removes any duplicate names from the result set.

### Example 3: Use Union All to Include Duplicate Rows

By default, Union removes duplicate rows from the result set. If you want to include all rows from both `SELECT` statements, including duplicates, you can use Union All instead:

```sql
SELECT region, sales_amount, year FROM sales
UNION ALL
SELECT region, sales_amount, year FROM marketing;
```
This query combines the sales data from the "sales" and "marketing" tables and returns a result set that includes all rows, including duplicates.



