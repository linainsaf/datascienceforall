## Introduction

In the last chapter we learnt the basics of programming, you were shown how to store data in data structures such as lists, strings, integers, dictionaries, and others. And you were shown how to create behavior for your program using keywords, and later using functions to group these keywords. This coding approache is called Logic Programming.

<br />

However, there are different approaches or perspectives in computer programming which we call programming paradigms. They provide various ways of organizing and structuring code to solve a particular problem.

<br />

Each paradigm has its own strengths, weaknesses, and suitability for different types of problems and use cases. Some programming languages may support multiple paradigms, while others may have limited support for one specific paradigm.

<br />

In this course we will get to know the object-oriented programming (OOP), a programming paradigm widely used in Python.

<br />

PS: At this stage of the course, we assume that you know the basics of Python.

## Object-oriented programming (OOP)

### What is it ?

Object-oriented programming (OOP) is an approach that organizes software design based on objects, which are data fields with unique attributes and behaviors, instead of functions and logic. 


### Why we use it ?

One of the main benefits of OOP is its organization, which makes it easier for developers to collaborate on a project by dividing it into smaller groups. Additionally, OOP offers several other advantages, such as code reusability, scalability, and efficiency.

### OOP Basic Concepts 

- **Class**: A class is a blueprint for creating objects. It defines a set of attributes (properties) and methods (functions) that the objects created from the class will have. For example, you could create a "Person" class with attributes like name, age, and address, and methods like "introduce" and "greet".

- **Object**: An object is an instance of a class. When you create an object from a class, you get a specific "realization" of the class, with its own set of attributes and methods. For example, you could create two "Person" objects, "John" and "Jane", each with their own name, age, and address.

- **Attributes**: Attributes are the properties or characteristics of an object. They define the state of the object. In the example of the "Person" class, the attributes would be name, age, and address.

- **Methods**: Methods are the actions or behaviors of an object. They define what the object can do. In the example of the "Person" class, the methods would be "introduce" and "greet".

Here is a simple example of a Python class that defines a "Person" object:

```python
class Person:
    def __init__(self, name, age, address):
        self.name = name
        self.age = age
        self.address = address
    
    def introduce(self):
        return f"Hi, my name is {self.name} and I am {self.age} years old."
    
    def greet(self, other_person):
        return f"Hello {other_person.name}, it's nice to meet you!"
```
And here is how you can create objects from the "Person" class and use their attributes and methods:

```python
john = Person("John", 30, "123 Main St.")
jane = Person("Jane", 25, "456 Elm St.")

print(john.introduce())  # Output: Hi, my name is John and I am 30 years old.
print(jane.greet(john))  # Output: Hello John, it's nice to meet you!
```

### When to use it ?


- **Modeling real-world objects**: You can create classes to model real-world objects in Python, such as dogs, cars, or books. For example, you can create a "Dog" class with properties like breed, name, and age, and methods like "bark", "eat", and "sleep". This makes it easier to manipulate and work with instances of the class, and to keep track of the state of each object.

- **Building games**: OOP is often used in game development to model game objects and their behaviors. For example, you can create a "Player" class to represent a player in a game, with properties like position, health, and score, and methods like "move", "attack", and "jump".

- **Database applications**: You can use OOP to interact with databases in Python. For example, you can create a "Record" class to represent a record in a database table, with properties like id, name, and date, and methods like "insert", "update", and "delete".

- **Web development**: OOP is commonly used in web development to build applications and services. For example, you can create a "User" class to represent a user of your application, with properties like name, email, and password, and methods like "register", "login", and "logout".

- **Scientific simulations**: OOP can be used to create scientific simulations, such as physical simulations or financial models. For example, you can create a "Particle" class to represent a particle in a physical simulation, with properties like position, velocity, and mass, and methods like "move", "collide", and "absorb".

These are just a few examples of how OOP can be used in Python. With its powerful and flexible object-oriented features, OOP is a widely used paradigm in Python and can be applied to many different types of projects.

### Some examples 

- **Car**: Create a class Car that represents a car. The class should have properties brand, model, and year, and a method drive that makes the car drive (print "Driving the car !").


```python
## Class implementation
class Car:
    def __init__(self, brand, model, year):
        self.brand = brand
        self.model = model
        self.year = year
    
    def drive(self):
        print("Driving the car !")

## Object declaration
car = Car("Toyota", "Camry", 2020)

print(car.drive())
# Output: Driving the car !

```

- **Dog**: Create a class Dog that represents a dog. The class should have properties name, breed, and age, and a method bark that makes the dog bark (print "Woof!").

```python
## Class implementation
class Dog:
    def __init__(self, name, breed, age):
        self.name = name
        self.breed = breed
        self.age = age
    
    def bark(self):
        print("Woof!")

## Object declaration
dog = Dog("Max", "Labrador", 5)
print(dog.bark())
# Output: Woof!
```
 
 Let's code !

### Operator overloading

Operator overloading allows objects of user-defined classes to behave like built-in data types. This means that you can use familiar syntax for objects of your classes, making your code more intuitive and easier to read.

<br />

For example, consider a class for a 2D point:

```python
class Point2D:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return f"({self.x}, {self.y})"
```
This class defines a 2D point, with x and y coordinates. You can create instances of the class and display them:

```python
p1 = Point2D(1, 2)
print(p1)  # Output: (1, 2)
```
Now, if you want to add two points, you can overload the addition operator + by defining the \_\_add\_\_ method:

```python
class Point2D:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return f"({self.x}, {self.y})"
    
    def __add__(self, other):
        return Point2D(self.x + other.x, self.y + other.y)
```
With this change, you can now add two points:

```python
p1 = Point2D(1, 2)
p2 = Point2D(3, 4)
p3 = p1 + p2
print(p3)  # Output: (4, 6)
```

In this example, the \_\_add\_\_ method takes another Point2D object as its argument and returns a new Point2D object that represents the sum of the two points. This allows you to use the + operator with instances of your class, just like you would with built-in data types.

<br />

You can also overload other operators, such as -, \*, /, <, >, and so on, by defining the corresponding special methods, such as \_\_sub\_\_, \_\_mul\_\_, \_\_truediv\_\_, \_\_lt\_\_, \_\_gt\_\_, and so on. 

<br />

Here's an example that overloads the subtraction operator:

```python
class Point2D:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __str__(self):
        return f"({self.x}, {self.y})"
    
    def __add__(self, other):
        return Point2D(self.x + other.x, self.y + other.y)
    
    def __sub__(self, other):
        return Point2D(self.x - other.x, self.y - other.y)
```
With this change, you can now subtract two points:

```python
p1 = Point2D(1, 2)
p2 = Point2D(3, 4)
p3 = p1 - p2
print(p3)  # Output: (-2, -2)
```
##### Cheat sheet for Class Special Methods:

| Operator    | Method       |
|-------------|--------------|
| String representation     | \_\_str\_\_      |
| Addition (+)              | \_\_add\_\_      |
| Subtraction (-)           | \_\_sub\_\_      |
| Multiplication (\*)       | \_\_mul\_\_      |
| Power (\*\*)              | \_\_pow\_\_      |
| Division (/)              | \_\_truediv\_\_  |
| Floor Division (//)       | \_\_floordiv\_\_ |
| Remainder (modulo)(%)     | \_\_mod\_\_      |
| Lesser than (<)           | \_\_lt\_\_       |
| Greater than (>)          | \_\_gt\_\_       |
| Lesser than or equal (<=) | \_\_le\_\_       |
| Greater than or equal (>=)| \_\_ge\_\_       |
| Equal (==)                | \_\_eq\_\_       |
| Not equal (!=)            | \_\_ne\_\_       |
| Absolute value (abs())    | \_\_abs\_\_      |
| Bitwise AND(&)            | \_\_and\_\_      |
| Bitwise OR (\|)           | \_\_or\_\_       |
| Bitwise NOT(~)            | \_\_invert\_\_   |

### Inheritance in Object-Oriented Programming

#### What is it ?


Inheritance is a mechanism in Object-Oriented Programming (OOP) that allows a new class to be defined based on an existing class. The new class, known as the subclass, inherits attributes and behavior from the existing class, known as the superclass. This enables code reuse, allowing new classes to be defined with little or no modifications to the existing classes.

For example, consider a superclass called Animal which has attributes such as name, species, and age, and a method called make_sound() that returns the sound the animal makes. We can create a subclass of Animal called Dog which inherits all of the attributes and behavior of the Animal class. We can also add additional attributes specific to dogs such as breed and override the make_sound() method to return the specific sound a dog makes, like "bark".

```python
class Animal:
    def __init__(self, name, species, age):
        self.name = name
        self.species = species
        self.age = age
    
    def make_sound(self):
        return "Sound made by the animal"

class Dog(Animal):
    def __init__(self, name, breed, age):
        Animal.__init__(self, name, "Dog", age)
        self.breed = breed
    
    def make_sound(self):
        return "Bark"

dog = Dog("Rufus", "Labrador", 5)
print(dog.name) # Rufus
print(dog.species) # Dog
print(dog.age) # 5
print(dog.breed) # Labrador
print(dog.make_sound()) # Bark

```

In this example, the Dog class inherits the attributes name, species, and age from the Animal class and also adds an additional attribute breed. The method make_sound() is also overridden in the Dog class to return a specific sound for dogs.

Inheritance provides a way to model relationships between classes, and is an important concept in OOP for code reuse and organization. In Python, inheritance is an is-a relationship. That is, we use inheritance only if there exists an is-a relationship between two classes. For example,

- Car is a Vehicle
- Student is a Person
- Cat is an Animal

Here, Car can inherit from Vehicle, Apple can Student from Person, and so on.

#### Polymorphism and Abstraction

- **Polymorphism** : Is the ability of an object to take on multiple forms. It allows objects of different classes to be treated as objects of the same class. This means that you can write a single function or method that can work with objects of different classes, as long as they implement the same interface.

- **Abstraction** : Is a technique that allows you to simplify complex systems by ignoring irrelevant details and focusing on the essential features of an object. It is achieved by defining an interface that exposes the essential features of an object, while hiding the implementation details.


Here's an example in Python to demonstrate the difference between polymorphism and abstraction:

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return 3.14 * self.radius * self.radius

def print_area(shape):
    print(shape.area())

rect = Rectangle(10, 20)
circ = Circle(5)

print_area(rect) # 200
print_area(circ) # 78.5

```

In this example, the Shape class serves as an abstract class and defines an abstract method area. The Rectangle and Circle classes inherit from Shape and provide their own implementation of the area method. 

<br />

This demonstrates polymorphism, as objects of different classes (Rectangle and Circle) can be treated as objects of the same class (Shape). The print_area function demonstrates polymorphism, as it can accept objects of different classes (Rectangle and Circle) as long as they implement the same interface (area method).

<br />

At the same time, the Shape class demonstrates abstraction by defining an interface that exposes the essential features of a shape, while hiding the implementation details. The Rectangle and Circle classes implement the area method, but the details of the implementation are hidden from the user. The user only sees the essential features of the object (the area method).

#### Multiple and Multilevel Inheritance

 - **Multiple Inheritance**: is a feature of Object Oriented Programming (OOP) languages where a class can inherit properties and attributes from more than one parent class. This means that a single class can have multiple base classes and it can inherit properties and attributes from all of them. 

 	For example, consider a scenario where you have two classes: Shape and Color. The class Shape defines the properties of a 2-D shape such as its area, perimeter, etc. The class Color defines the color of an object. Now, you want to create a new class Rectangle which is a shape and has a color. You can achieve this by using multiple inheritance. 


Here's an example implementation in Python:

```python
class Shape:
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

    def perimeter(self):
        return 2 * (self.width + self.height)

class Color:
    def __init__(self, color):
        self.color = color

class Rectangle(Shape, Color):
    pass

rect = Rectangle(10, 20, 'red')
print(rect.area())
print(rect.perimeter())
print(rect.color)
```

In this example, the class Rectangle inherits from both Shape and Color classes. This means that it has all the attributes and methods defined in both the classes.

<br />


- **Multilevel Inheritance** : is a type of inheritance where a class inherits properties and attributes from its parent class, and the parent class inherits from its parent class, and so on.


	For example, consider a scenario where you have a class Animal which defines basic properties of an animal such as its name, age, and breed. Another class Mammal inherits from Animal class and adds new properties such as type of fur, etc. Finally, a new class Cat is created which inherits from Mammal class and adds new properties specific to cats such as the type of meow, etc. 

Here's an example implementation in Python:

```python
class Animal:
    def __init__(self, name, age, breed):
        self.name = name
        self.age = age
        self.breed = breed

class Mammal(Animal):
    def __init__(self, name, age, breed, fur_type):
        Animal.__init__(self, name, age, breed)
        self.fur_type = fur_type

class Cat(Mammal):
    def __init__(self, name, age, breed, fur_type, meow_type):
        Mammal.__init__(self, name, age, breed, fur_type)
        self.meow_type = meow_type

cat = Cat('Tom', 3, 'Siamese', 'short hair', 'loud')
print(cat.name)
print(cat.age)
print(cat.breed)
print(cat.fur_type)
print(cat.meow_type)
```

In this example, the class Cat inherits from the class Mammal which in turn inherits from the class Animal. This creates a hierarchy of classes where each class inherits properties and attributes from its parent class.