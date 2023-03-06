# Python wrap-up project 

## Project: Employee Management System

### Description
You are tasked with building an Employee Management System for a company using FastAPI and SQLAlchemy. The system should allow the HR department to manage employee records, including personal information, salary, and department information.

### Requirements

- Create three database tables: employees, departments, and salaries. The employees table should have columns for emp_no, first_name, last_name, gender, hire_date, and dept_no. The departments table should have columns for dept_no and dept_name. The salaries table should have columns for emp_no, salary, and from_date and to_date.
- Implement an endpoint to add new employees to the database. The endpoint should accept a JSON payload containing the employee's personal information and department. The endpoint should validate the data and insert it into the database. If the department does not exist in the departments table, it should be created.
- Implement an endpoint to update an employee's salary. The endpoint should accept a JSON payload containing the employee's emp_no and new salary. The endpoint should validate the data and update the salaries table accordingly.
- Implement an endpoint to retrieve a list of employees with their current salary and department. The endpoint should accept an optional query parameter to filter by department. If the parameter is not provided, the endpoint should return all employees.
- Implement an endpoint to retrieve a list of departments and their total salary budget. The endpoint should calculate the total salary budget for each department based on the current salaries of its employees.
- Implement an endpoint to delete an employee from the database. The endpoint should accept an emp_no parameter and delete the corresponding record from the employees, salaries, and dept_emp tables.
- Implement appropriate error handling for each endpoint.
- Write unit tests for each endpoint.
- Use FastAPI's dependency injection system to inject the database session into each endpoint.
- Use SQLAlchemy's built-in ORM features to define the database schema and manage database transactions.
- The system should incorporate triggers to notify HR personnel when an employee is due for a performance review, salary increase, or other HR-related tasks.
- The system should be able to join multiple tables to provide HR personnel with a holistic view of employee records and performance.
Bonus points :

- Use Docker to containerize the application and its dependencies.
- Deploy the application to a cloud platform such as Heroku or AWS Elastic Beanstalk.

### Example Workflow:

- HR personnel log in to the system and are presented with a dashboard displaying employee records and performance metrics.
- HR personnel search for a specific employee record and update their personal information, such as address, phone number, or emergency contact.
- The system sends a notification to HR personnel when an employee is due for a performance review, prompting them to schedule a meeting with the employee.
- HR personnel use the system's data analysis tools to identify top-performing employees and provide them with bonuses or promotions.
- The system generates HR reports that provide insights into employee performance, attendance, and payroll data.


Overall, this project would require a strong understanding of SQL, including views, triggers, and joins, as well as web development with FastAPI and SQLAlchemy.
It would also require careful attention to detail and robust error handling to ensure the system is secure, reliable, and scalable (for the bonus point).

## Project: Online Bookstore Management System

### Description 

Build an online bookstore management system using FastAPI and SQLAlchemy. The system should allow users to browse, search, and purchase books online. It should also allow administrators to manage the books, customers, orders, and inventory.

### Requirements:

- Create a database schema with at least four tables: books, customers, orders, and inventory.
- Use SQLAlchemy to map the database tables to Python classes.
- Implement RESTful API endpoints using FastAPI to allow users to browse and search books, place orders, and manage their accounts.
- Implement a view to show the top 10 best-selling books based on sales data.
- Implement triggers to update the inventory when orders are placed or cancelled.
- Implement a view to show the current inventory levels and the books that are running low on stock.
- Implement joins to retrieve customer order history and book sales data.

Bonus: 

- Implement authentication and authorization using JWT tokens.
- Implement an admin panel with secure login for administrators to manage the books, customers, orders, and inventory.
- Implement email notifications to customers when they place orders or their orders are shipped.

This project will require skills in database design, SQL, Python programming, web development, and system architecture. It will also provide you an opportunity to gain experience with common web development frameworks and tools such as authentication, authorization and email notifications.

## Project: E-commerce platform

### Description 

Create a web application for an e-commerce platform using SQLAlchemy and FastAPI. The application should have the following features:

- User registration and authentication: Users should be able to register for an account and authenticate themselves to access their account information, order history, and other features.
- Product management: Users with administrative access should be able to add, modify, and delete product listings.
- Shopping cart: Users should be able to add products to a shopping cart and proceed to checkout to complete their purchase.
- Order history: Users should be able to view their order history and check the status of their current orders.
- Search functionality: Users should be able to search for products by name, description, category, and other criteria.

### Requirements:

- Use SQLAlchemy to create and manage the database schema.
- Use FastAPI to create RESTful endpoints for user authentication, product management, shopping cart, order history, and search functionality.
- Implement CRUD operations for product management.
- Implement a shopping cart using SQLAlchemy ORM.
- Use SQLAlchemy to create triggers for order history and updating product inventory.
- Use SQLAlchemy to create views to provide reports such as total sales and most popular products.
- Use SQLAlchemy to create joins to combine information from multiple tables.
- Use Python and SQLAlchemy to seed the database with sample data from generatedata.com 
- Deploy the application to a cloud-based server such as AWS or Heroku.

This project will provide students with experience in building a complex web application using SQLAlchemy and FastAPI, as well as deploying and managing the application on a cloud-based server. They will also gain experience in implementing advanced database features such as triggers, views, and joins.