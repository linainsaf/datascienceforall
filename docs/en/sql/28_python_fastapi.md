# SQLAlchemy and FastAPI

## Introduction

FastAPI is a popular Python web framework that allows for fast and efficient development of APIs. It includes a built-in support for SQLAlchemy, a powerful SQL toolkit and ORM. SQLAlchemy provides a way to interact with databases in a more pythonic way than using raw SQL, and FastAPI's integration with SQLAlchemy makes it easy to build high-performance web applications with a database backend.

In this tutorial, we will use FastAPI and SQLAlchemy to build a RESTful API that interacts with a MySQL database containing employee data.

### Why building an API 

An API provides a structured and standardized way for applications to communicate with each other. By creating an API for your database, you allow other applications to access the data in a controlled manner. This can provide a number of benefits:

- **Security**: By creating an API, you can implement security measures to protect your database from unauthorized access. You can limit access to certain endpoints and require authentication to access the data.
- **Scalability**: An API allows you to separate the front-end and back-end of your application, which makes it easier to scale each component independently. You can also use caching and other techniques to improve performance.
- **Flexibility**: By exposing your database through an API, you can allow other applications to access and use the data in a variety of ways. This can lead to new and innovative use cases that you may not have considered.
- **Standardization**: By using a standardized API format, such as REST, you make it easier for other developers to understand and use your API. This can help to encourage collaboration and adoption.

In summary, creating an API provides a number of benefits for accessing and managing data. It can help to improve security, scalability, flexibility, and standardization.


## Prerequisites

Before we begin, make sure you have the following installed:

- Python 3.6 or later
- Pip
- MySQL database
- MySQL Python connector

To install FastAPI and SQLAlchemy, run the following command:
```py
pip install fastapi sqlalchemy
```

## Setting up the Database in FastAPI

For this tutorial, we will be using the MySQL employees database as usual. 

Once you have the database set up, create a new file `main.py` and add the following code:

```py title="main.py"
from fastapi import FastAPI, HTTPException
from sqlalchemy.orm import Session
from sqlalchemy import create_engine
from sqlalchemy.ext.automap import automap_base

# create the FastAPI instance
app = FastAPI()

# create the database engine
engine = create_engine("mysql://root:root@localhost:3306/employees")

# reflect the database schema to an ORM base
Base = automap_base()
Base.prepare(engine, reflect=True)

# map the tables to ORM classes
Employees = Base.classes.employees
Departments = Base.classes.departments
Dept_Emp = Base.classes.dept_emp
Salaries = Base.classes.salaries

# create a function to get a database session
def get_db():
    try:
        db = Session(bind=engine)
        yield db
    finally:
        db.close()
```
The new thing in the code is a function called `get_db()` which is used to create a database session to interact with the database.

The function first tries to create a session using `Session(bind=engine)` and binds the session to the database engine object engine. The yield keyword makes the function a generator function which can be iterated over to yield a database session.

When the session is no longer needed, the finally block of the function is executed which closes the database session using `db.close()`. This ensures that the session is closed properly and any resources associated with the session are released back to the system.

Using this function, you can get a database session by calling `get_db()`, which returns a generator object. You can then iterate over the generator to get a database session, like so:
```py 
with get_db() as db:
    # interact with the database session here
```

## Addind Endpoints to our app 

In FastAPI, an endpoint or route is a combination of a specific HTTP method (such as GET, POST, PUT, DELETE, etc.) and a URL path that a client can use to communicate with the server and perform some specific action or operation.

Endpoints are defined in your FastAPI application using decorated functions that are called view functions. These view functions handle the request/response cycle for a specific endpoint, which includes handling any incoming requests with specific parameters and returning the appropriate response data.

In other words, endpoints are the primary building blocks of a FastAPI application, and they determine how clients can interact with your API. They enable you to define a clear and well-structured API that can be easily understood and used by other developers, while also providing a standardized way for clients to communicate with your application.

### Endpoint `/employees`

This route is used to retrieve a list of employees from the database. It returns a JSON object containing the employee's first name, last name, gender, and hire date. It also accepts query parameters for filtering the results by gender and hire date.

### Endpoint `/employees/{emp_no}`

This route is used to retrieve information about a specific employee by their employee number. It returns a JSON object containing the employee's `first name`, `last name`, `gender`, and `hire date`.

Let's code this in python : 

```py
# define a route to get the employee data by ID
@app.get("/employees/{emp_no}")
async def get_employee(emp_no: int, db: Session = Depends(get_db)):
    employee = db.query(Employees).filter(Employees.emp_no == emp_no).first()
    if not employee:
        raise HTTPException(status_code=404, detail="Employee not found")
    return employee
```

### Endpoint `/departments`
This route is used to retrieve a list of departments from the database. It returns a JSON object containing the department's name and number.

### Endpoint  `/departments/{dept_no}`
This route is used to retrieve information about a specific department by its department number. It returns a JSON object containing the department's name and number.

Let's implement this : 
```py 
# define a route to get the department data by ID
@app.get("/departments/{dept_no}")
async def get_department(dept_no: str, db: Session = Depends(get_db)):
    department = db.query(Departments).filter(Departments.dept_no == dept_no).first()
    if not department:
        raise HTTPException(status_code=404, detail="Department not found")
    return department
```

### Endpoint `/employees/{emp_no}/salaries`

This route is used to retrieve a list of salaries for a specific employee by their employee number. It returns a JSON object containing the salary amount, start and end dates of the salary period.

```py 
# define a route to get the salary history of an employee by ID
@app.get("/employees/{emp_no}/salaries")
async def get_employee_salaries(emp_no: int, db: Session = Depends(get_db)):
    salaries = db.query(Salaries).filter(Salaries.emp_no == emp_no).all()
    if not salaries:
        raise HTTPException(status_code=404, detail="No salary history found for employee")
    return salaries
```

## Writing our API

Let wrap up and add a final endpoint `/departments/{dept_no}/employees` : 

```py title="main.py"
from fastapi import FastAPI, HTTPException
from sqlalchemy.orm import Session
from sqlalchemy import create_engine
from sqlalchemy.ext.automap import automap_base

# create the FastAPI instance
app = FastAPI()

# create the database engine
engine = create_engine("mysql://root:root@localhost:3306/employees")

# reflect the database schema to an ORM base
Base = automap_base()
Base.prepare(engine, reflect=True)

# map the tables to ORM classes
Employees = Base.classes.employees
Departments = Base.classes.departments
Dept_Emp = Base.classes.dept_emp
Salaries = Base.classes.salaries

# create a function to get a database session
def get_db():
    try:
        db = Session(bind=engine)
        yield db
    finally:
        db.close()

# define a route to get the employee data by ID
@app.get("/employees/{emp_no}")
async def get_employee(emp_no: int, db: Session = Depends(get_db)):
    employee = db.query(Employees).filter(Employees.emp_no == emp_no).first()
    if not employee:
        raise HTTPException(status_code=404, detail="Employee not found")
    return employee

# define a route to get the department data by ID
@app.get("/departments/{dept_no}")
async def get_department(dept_no: str, db: Session = Depends(get_db)):
    department = db.query(Departments).filter(Departments.dept_no == dept_no).first()
    if not department:
        raise HTTPException(status_code=404, detail="Department not found")
    return department

# define a route to get the list of employees in a department by ID
@app.get("/departments/{dept_no}/employees")
async def get_department_employees(dept_no: str, db: Session = Depends(get_db)):
    employees = db.query(Employees).\
        join(Dept_Emp, Employees.emp_no == Dept_Emp.emp_no).\
        filter(Dept_Emp.dept_no == dept_no).all()
    if not employees:
        raise HTTPException(status_code=404, detail="No employees found in department")
    return employees

# define a route to get the salary history of an employee by ID
@app.get("/employees/{emp_no}/salaries")
async def get_employee_salaries(emp_no: int, db: Session = Depends(get_db)):
    salaries = db.query(Salaries).filter(Salaries.emp_no == emp_no).all()
    if not salaries:
        raise HTTPException(status_code=404, detail="No salary history found for employee")
    return salaries
```

To run this script, you can save it as a Python file (e.g. main.py) and then run the following command in your terminal:

```bash
uvicorn main:app --reload
```

This will start a local web server at http://localhost:8000/docs where you can access the different routes defined in the script 🤓

Go on the first endpoint `employees/{emp_no}` and click on `try it out` and take an example employee ID (let's say 10001) you should see this in your browser : 

![Screenshot](../img/fastapi.png)


