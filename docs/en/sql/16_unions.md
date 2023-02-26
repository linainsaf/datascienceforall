# Union

## Explanation of Union in SQL

Union is a set operation in SQL that is used to combine the results of two or more `SELECT` statements into a single result set. Union returns a distinct set of rows that are present in either of the two or more `SELECT` statements. Union can be used to combine data from multiple tables, or to combine data from a single table that is split across multiple columns.

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

## Conclusion

Union is a powerful set operation in SQL that is used to combine the results of two or more `SELECT` statements into a single result set. Union can be used to combine data from multiple tables or to combine data from a single table that is split across multiple columns. By understanding how to use Union in SQL, you can simplify data analysis and retrieve a consolidated view of data that is spread across multiple sources.




