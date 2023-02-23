# What is a Database ?

A database is an organized collection of data that is designed to be easy to access, manage, and update. Databases are used in a wide range of applications, from social media platforms to scientific research to financial systems. They are essential for storing and managing large amounts of data in a way that makes it easy to find, update, and retrieve the information you need quickly.

## Some examples 

One of the simplest examples of a database is an address book. Think about the contacts list on your phone. You have a list of people you know, along with their phone numbers, email addresses, and other details. This list is essentially a simple database, with each contact representing a record in the database. You can easily search for a contact by name, update their information, or delete a contact when you no longer need it.

Another example of a database is an online store. When you shop online, you browse through a catalog of products, add items to your cart, and then complete your purchase. Behind the scenes, the online store is using a database to store information about the products, customers, and orders. When you search for a product, the database is used to find all the relevant information about that product, including the price, description, and availability. When you complete your purchase, the database is updated with information about the order, including the items you bought, the shipping address, and the payment method.

## Types of Databases

There are many different types of databases, each with its own strengths and weaknesses. Some of the most common types of databases include:

* Relational databases: These databases organize data into tables with columns and rows, similar to a spreadsheet. They are the most common type of database and are used in many different applications.
* NoSQL databases: These databases are designed to handle large amounts of unstructured or semi-structured data. They are commonly used in big data applications, such as social media analytics and scientific research.
* Object-oriented databases: These databases store data as objects, which can be manipulated using object-oriented programming techniques. They are commonly used in software development and data modeling.

In this course, we will be focusing on relational databases, specifically MySQL, which is a popular open-source database management system. We will also introduce the concept of Object Relational Mapping (ORM) using Python and the SQLAlchemy library. 

By the end of the course, you will have a solid understanding of the different types of databases and when to use them, as well as practical experience working with relational databases and SQL.

## Examples of day-to-day use cases for each type of database

### Relational databases

* Customer database example : A business might use a relational database to keep track of customer information, such as their name, contact details, and purchase history. They could then use this information to target customers with personalized marketing campaigns based on their previous purchases.
* Inventory management example : A store might use a relational database to manage their inventory, with one table for products and another table for suppliers. They could then use SQL queries to quickly retrieve information on which products are in stock, which products are selling quickly, and which suppliers they need to contact to restock their inventory.
* Employee scheduling example : A company might use a relational database to manage employee schedules, with one table for employees and another table for shifts. They could then use SQL queries to quickly retrieve information on which employees are available to work on a particular day or time, and which shifts still need to be filled.

### NoSQL databases 

* Social media analytics: Social media platforms like Facebook and Twitter use NoSQL databases to store and analyze massive amounts of user data, such as likes, comments, and shares. This allows them to quickly retrieve and analyze user data to provide better ad targeting and personalized content.
* Internet of Things (IoT) devices: IoT devices like smart thermostats and security cameras generate a huge amount of data, which can be stored and analyzed in NoSQL databases. This allows manufacturers to track device usage patterns, identify and fix bugs, and improve device performance over time.
* Gaming: Many video games use NoSQL databases to store player data, such as character stats and in-game achievements. This allows players to continue their game progress across different devices, and enables game developers to quickly retrieve and analyze player data to identify areas for improvement.

### Object-oriented databases

* Geolocation data: Companies that rely on geolocation data, such as mapping and navigation services, often use object-oriented databases to store and retrieve this data. This allows them to quickly retrieve and analyze large amounts of geolocation data in real-time.
* E-commerce: An e-commerce website might use an object-oriented database to store and manage product information, such as product images and descriptions. This allows them to easily update and manage product information across multiple platforms, such as their website, mobile app, and social media.
* Medical records: Hospitals and healthcare providers often use object-oriented databases to manage patient medical records, which can include a wide range of data types, such as images, test results, and diagnoses. This allows healthcare providers to easily access and update patient information, and can help improve patient care and outcomes.


In this part we will focus only on relational databases. 

## How to interact with databases : MySQL client and CLI

Think of a database like a big organized warehouse full of information. In order to get information from the warehouse or add information to it, we need a way to talk to it. 

That's where the MySQL client comes in. It's like the messenger that we use to talk to the warehouse, and it allows us to read or update information stored in the database.

We use a command line interface (CLI) to interact with the MySQL client, which allows us to send specific commands to the database. A command-line interface (CLI) is a text-based interface used to interact with a computer's operating system or software by typing commands in a terminal window. It allows you to perform various tasks such as managing files and directories, executing programs, and interacting with databases, by entering commands and receiving text-based output.

In the context of MySQL, the CLI is a tool that enables you to interact with a MySQL database from a terminal window or a graphic interface by typing SQL commands. This means you can create, modify, and query your databases without using a graphical user interface. The CLI is a powerful tool that gives you fine-grained control over your database, but it requires some knowledge of SQL commands and syntax to use effectively.

### Vocabulary 

Here's an additional point on the differences between MySQL client, server, and SQL:

* MySQL client is a command-line tool that allows you to interact with the MySQL server, execute SQL queries, and manage databases.
* MySQL server is the software that stores and manages databases, and allows multiple clients to connect to it and perform operations on the databases.
* SQL (Structured Query Language) is the language used to interact with relational databases like MySQL, and it provides a standardized syntax for creating, modifying, and querying databases.


