## Basic Python Concepts

### Variables

 Variables are used to store values in Python. They are like containers that hold data, and you can use them to perform operations on that data. Variables are declared using the assignment operator (=) and can be of different types, such as integers, floating-point numbers, strings, and more.

```python
# Example of variable assignment
x = 5
y = "Hello World"
z = [1, 2, 3]
```
### Data types and structures

Python supports several built-in data types, such as integers, floating-point numbers, strings, lists, tuples, and dictionaries. Each data type has its own set of characteristics and methods.

```python
# Example of different data types
x = 5 # integer
y = 3.14 # floating-point number
z = "Hello World" # string
a = [1, 2, 3] # list
b = (4, 5, 6) # tuple
c = {"name": "John", "age": 30} # dictionary
```
### Operators

Python supports various types of operators, such as arithmetic operators (+, -, x , /, %), comparison operators (>, <, >=, <=, ==, !=), and logical operators (and, or, not). These operators are used to perform different types of operations on variables and data.

```python
# Example of operators
x = 5
y = 2

# arithmetic operators
print(x + y) # 7
print(x - y) # 3
print(x * y) # 10
print(x / y) # 2.5
print(x % y) # 1

# comparison operators
print(x == y) # False
print(x > 2) # True

# logical operators 
print(x == y and x > 2) # False
print(x == y or x > 2) # True
```

### Control flow

Control flow or Conditional Statements allows us to control the flow of execution of our program based on certain conditions. For example : 

* if-elif-else statements 


```python
x = 5
if x > 0:
    print("x is positive")
elif x < 0:
    print("x is negative")
else:
    print("x is zero")
```

* for loop


```python
for i in range(5):
    print(i)
```

* while loop  

```python
x = 5
while x > 0:
    print(x)
    x -= 1
```

### Functions

Functions are a way to group together a set of instructions to perform a specific task. Functions are defined using the def keyword and can take input arguments and return output values.

```python
def add(x, y):
    return x + y

result = add(5, 2)
print(result) # 7
```

### Modules and Libraries

Python has a large number of built-in modules and libraries that provide a wide range of functionality. You can use these modules to perform various tasks, such as working with the file system, performing mathematical calculations, and more.

```python
# Example of importing a module
import math

result = math.sqrt(16)
print(result) # 4.0

# Example of importing a specific function from a module
from math import sqrt

result = sqrt(16)
print(result) # 4.0
```
### Exception Handling

Python supports exception handling, which allows you to handle errors and exceptions that may occur while running your program. This helps you to write robust code that can handle unexpected situations

```python
while True:
    try:
        x = int(input("Please enter a number: "))
        y = int(input("Please enter another number: "))
        print("The result of x/y is:", x/y)
        break
    except ValueError:
        print("Oops! One of the inputs was not a valid number. Try again...")
    except ZeroDivisionError:
        print("Oops! You cannot divide by zero. Try again...")
```

In this example, program will handle two types of exception: ValueError and ZeroDivisionError, as the user input may not be valid number and also user may try to divide by zero.

<br />

Now, let's code ! 



### Things to Remember

- Case Sensitivity: Python is a case-sensitive language, so be mindful of the case when naming variables, functions, and classes. This means, **Variable** and **variable** are not the same.

- **Each variable, function and class should have a unique name within your code**

- The only variables you need to consider inside your function are the arguments of that function

- Always give the identifiers a name that makes sense. While **c = 10** is a valid name, writing **count = 10** would make more sense, and it would be easier to figure out what it represents when you look at your code after a long gap.

- Multiple words can be separated using an underscore, like **this_is_a_long_variable**.

- Comments: Use the pound symbol (#) to denote comments in your code, which will be ignored by the interpreter. In computer programming, comments are hints that we use to make our code more understandable. 

- Quotation Marks: In Python, you can use either single quotes or double quotes to denote strings, but be consistent within your code.

- Colon (:): Colons are used to denote the start of a new block of code, such as in a for loop, if statement, or function definition.

- White Space: Be mindful of white space, as it can affect the way your code is interpreted. For example, leading white space is used to denote blocks of code.

- Parentheses: Parentheses are used to group expressions, to call functions, and to define tuples.

- Import Statement: Use the import statement to import libraries and modules into your code.

- print() function: Use the print() function to output text to the console.

