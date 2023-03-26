# On delete 

## Introduction to ON DELETE

`ON DELETE` is a referential action that can be applied to foreign key constraints in SQL. It specifies what should happen to data in the child table when a row is deleted from the parent table. The different options available for `ON DELETE` include `CASCADE`, `SET NULL`, `RESTRICT`, and `NO ACTION`. Using `ON DELETE` is important for maintaining data integrity in a database.

## Overview of How to Use ON DELETE

Here's an example of creating a foreign key with `ON DELETE CASCADE` in SQL:
```sql
ALTER TABLE employees
ADD CONSTRAINT emp_dept_fk
FOREIGN KEY (dept_no)
REFERENCES departments (dept_no)
ON DELETE CASCADE;
```

In this example, a foreign key constraint named "emp_dept_fk" is added to the "employees" table. The foreign key references the "dept_no" column in the "departments" table, and specifies "ON DELETE CASCADE." This means that when a row is deleted from the "departments" table, any corresponding rows in the "employees" table will also be deleted.

<br />

Here's an example of creating a foreign key with "ON DELETE SET NULL" in SQL:

```sql
ALTER TABLE employees
ADD CONSTRAINT emp_manager_fk
FOREIGN KEY (emp_no)
REFERENCES employees (emp_no)
ON DELETE SET NULL;
```

In this example, a foreign key constraint named "emp_manager_fk" is added to the "employees" table. The foreign key references the "emp_no" column in the "employees" table, and specifies "ON DELETE SET NULL." This means that when a row is deleted from the "employees" table, any corresponding rows in other tables will have the foreign key column value set to NULL.

<br />

Here's an example of creating a foreign key with "ON DELETE RESTRICT" in SQL:
```sql
ALTER TABLE departments
ADD CONSTRAINT dept_manager_fk
FOREIGN KEY (emp_no)
REFERENCES employees (emp_no)
ON DELETE RESTRICT;
```

In this example, a foreign key constraint named "dept_manager_fk" is added to the "departments" table. The foreign key references the "emp_no" column in the "employees" table, and specifies "ON DELETE RESTRICT." This means that when a row is deleted from the "employees" table, any corresponding rows in the "departments" table cannot be deleted.

<br />

Here's an example of creating a foreign key with "ON DELETE NO ACTION" in SQL:
```sql
ALTER TABLE dept_emp
ADD CONSTRAINT dept_emp_fk
FOREIGN KEY (emp_no)
REFERENCES employees (emp_no)
ON DELETE NO ACTION;
```

In this example, a foreign key constraint named "dept_emp_fk" is added to the "dept_emp" table. The foreign key references the "emp_no" column in the "employees" table, and specifies "ON DELETE NO ACTION." This means that when a row is deleted from the "employees" table, any corresponding rows in the "dept_emp" table will cause an error and the delete operation will be rejected.

## Conclusion

Using "ON DELETE" in SQL is important for maintaining data integrity in a database. The different options available for "ON DELETE" include "CASCADE," "SET NULL," "RESTRICT," and "NO ACTION." By understanding how to use "ON DELETE" in SQL, you can ensure that your database remains consistent and accurate over time.

