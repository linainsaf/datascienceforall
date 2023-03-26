# Triggers

## Introduction to Triggers in SQL

In SQL, a trigger is a special kind of stored program that is automatically executed in response to specific events or actions that occur within a database. Triggers can be used to automate tasks, maintain data integrity, and enforce business rules. They are often used in conjunction with other SQL features, such as constraints and indexes, to ensure that a database remains consistent and accurate over time.

## Overview of How to Use Triggers to Automate Tasks

Here's an example of creating a trigger in SQL that automatically sets the "to_date" field of a row in the "dept_emp" table to the current date whenever a new row is inserted:

```sql
CREATE TRIGGER update_dept_emp_to_date
BEFORE INSERT ON dept_emp
FOR EACH ROW
SET NEW.to_date = NOW();
```

In this example, a trigger named "update_dept_emp_to_date" is created using the CREATE TRIGGER statement. The trigger is defined to execute "BEFORE INSERT" on the "dept_emp" table, and is set to execute "FOR EACH ROW" that is inserted. The body of the trigger consists of a single statement that sets the value of the "to_date" field for the newly inserted row to the current date and time, using the NOW() function.

<br />

Here's an example of creating a trigger in SQL that prevents the deletion of a row in the "employees" table if that row is currently assigned to a department:

```sql
CREATE TRIGGER prevent_emp_delete
BEFORE DELETE ON employees
FOR EACH ROW
BEGIN
	DECLARE dept_count INT;
	SELECT COUNT(*) INTO dept_count
	FROM dept_emp
	WHERE emp_no = OLD.emp_no;
	IF dept_count > 0 THEN
		SIGNAL SQLSTATE '45000'
			SET MESSAGE_TEXT = 'Cannot delete employee who is currently assigned to a department.';
	END IF;
END;
```

In this example, a trigger named "prevent_emp_delete" is created using the CREATE TRIGGER statement. The trigger is defined to execute "BEFORE DELETE" on the "employees" table, and is set to execute "FOR EACH ROW" that is deleted. The body of the trigger consists of a block of code that checks whether the employee being deleted is currently assigned to a department, and if so, raises an error using the SIGNAL statement to prevent the delete operation.

## Examples of using triggers on the employees database

### Example 1: Audit trail for changes to the employees table

```sql
CREATE TABLE employees_audit (
    action VARCHAR(50) NOT NULL,
    emp_no INT NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    first_name VARCHAR(50) NOT NULL,
    timestamp DATETIME DEFAULT NOW()
);

DELIMITER //

CREATE TRIGGER employees_audit_insert
AFTER INSERT ON employees
FOR EACH ROW
BEGIN
    INSERT INTO employees_audit (action, emp_no, last_name, first_name)
    VALUES ('insert', NEW.emp_no, NEW.last_name, NEW.first_name);
END//

CREATE TRIGGER employees_audit_update
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
    INSERT INTO employees_audit (action, emp_no, last_name, first_name)
    VALUES ('update', NEW.emp_no, NEW.last_name, NEW.first_name);
END//

CREATE TRIGGER employees_audit_delete
AFTER DELETE ON employees
FOR EACH ROW
BEGIN
    INSERT INTO employees_audit (action, emp_no, last_name, first_name)
    VALUES ('delete', OLD.emp_no, OLD.last_name, OLD.first_name);
END//

DELIMITER ;
```

In this example, a new table "employees_audit" is created to store an audit trail of changes to the "employees" table. Three triggers are then defined using the CREATE TRIGGER statement to execute after each insert, update, and delete operation on the "employees" table. These triggers use the INSERT statement to add a new row to the "employees_audit" table that records the action, employee number, last name, first name, and timestamp of the change.

### Example 2: Preventing changes to the departments table

```sql
CREATE TRIGGER prevent_dept_change
BEFORE UPDATE ON departments
FOR EACH ROW
BEGIN
    SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Changes to departments table are not allowed.';
END;
```

In this example, a trigger named "prevent_dept_change" is defined using the CREATE TRIGGER statement to execute before any update operation on the "departments" table. The body of the trigger consists of a single statement that raises an error using the SIGNAL statement to prevent the update operation.

### Example 3: Automatic promotion to manager for department heads

```sql
CREATE TRIGGER promote_to_manager
AFTER UPDATE ON dept_manager
FOR EACH ROW
BEGIN
    IF NEW.to_date = '9999-01-01' THEN
        UPDATE employees SET emp_title = 'Manager' WHERE emp_no = NEW.emp_no;
    END IF;
END;
```

In this example, a trigger named "promote_to_manager" is defined using the CREATE TRIGGER statement to execute after any update operation on the "dept_manager" table. The body of the trigger consists of an IF statement that checks whether the "to_date" field for the updated row is set to the special value of '9999-01-01', which indicates that the employee is currently a department head. If so, the trigger uses the UPDATE statement to change the employee's title to "Manager" in the "employees" table.

## Why triggers are important for data analysis job 

Triggers are important in data analysis because they allow you to automate tasks and enforce data integrity rules within a database. By using triggers, you can ensure that data is consistent and accurate over time, and that business rules are enforced consistently across different operations and users.

<br />

For example, if you are analyzing sales data in a database, you might use triggers to automatically update certain fields or tables whenever new sales data is added or existing data is updated. You could also use triggers to prevent certain types of changes to the database, or to notify you when certain events occur.

<br />

By automating tasks and enforcing rules using triggers, you can save time and reduce the risk of errors or inconsistencies in your data. This can help you to make more informed decisions and gain deeper insights into the patterns and trends in your data.

<br />

Overall, triggers are an important tool for data analysts and other database professionals who need to ensure the accuracy and integrity of their data. By mastering the use of triggers in SQL, you can become more effective at managing and analyzing data in a wide range of contexts.

## Conclusion

Using triggers in SQL is a powerful way to automate tasks, maintain data integrity, and enforce business rules. Triggers can be used to respond to specific events or actions that occur within a database, and can be defined to execute before or after specific operations such as inserts, updates, and deletes. By understanding how to use triggers in SQL, you can ensure that your database remains consistent and accurate over time.