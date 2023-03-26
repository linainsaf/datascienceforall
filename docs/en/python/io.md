### Introduction

File Input/Output (I/O) operations in Python allow you to read from and write to files on your local file system. The open function is the main function used for working with files in Python, and it returns a file object that can be used to perform various operations on the file.

### Opening a File

The basic syntax for opening a file in Python is as follows:

```python
file = open("filename.extension", "mode")
```

The first argument is the name of the file, and the second argument is the mode in which you want to open the file. The most common modes are "r" for reading, "w" for writing, and "a" for appending.

<br />

For example, to open a text file called example.txt in read mode, you would write the following code:

```python
file = open("example.txt", "r")
```

### Reading from a File
Once you have opened a file, you can read from it using various methods, such as read, readline, and readlines.

<br />

The read method reads the entire contents of a file as a single string. For example:

```python
file = open("example.txt", "r")
contents = file.read()
print(contents)
file.close()
```

The readline method reads a single line of a file. For example:

```python
file = open("example.txt", "r")
first_line = file.readline()
print(first_line)
file.close()
```

The readlines method reads all lines of a file as a list of strings, where each string is a single line. For example:

```python
file = open("example.txt", "r")
lines = file.readlines()
print(lines)
file.close()
```

### Writing to a File

To write to a file, you can use the write method. The basic syntax for writing to a file is as follows:

```python
file = open("filename.extension", "mode")
file.write("data to be written")
file.close()
```


For example, to write the string "Hello, World!" to a file called example.txt, you would write the following code:

```python
file = open("example.txt", "w")
file.write("Hello, World!")
file.close()
```

If the file specified in the open function does not exist, it will be created. If the file does exist, its contents will be overwritten by the new data.

### Appending to a File

To append data to an existing file, you can open the file in append mode ("a") instead of write mode ("w"). The basic syntax for appending to a file is as follows:

```python
file = open("filename.extension", "mode")
file.write("data to be written")
file.close()
```

For example, to append the string "Hello, World!" to a file called example.txt, you would write the following code:

```python
file = open("example.txt", "a")
file.write("Hello, World!")
file.close()
```

### Context Manager

One important thing to note when working with files in Python is that you should always close the file when you are done with it. This can be done using the close method, as demonstrated in the previous examples. However, there is a better way to ensure that the file is always closed, even if an error occurs, and that is by using a context manager.

<br />

A context manager is an object that provides a convenient way to manage resources, such as files, that need to be cleaned up after they are used. In Python, the with statement is used to create a context manager. The basic syntax for using a context manager to open a file is as follows:

```python
with open("filename.extension", "mode") as file:
    # Perform file I/O operations
```

For example, to read the contents of a file called example.txt using a context manager, you would write the following code:

```python
with open("example.txt", "r") as file:
    contents = file.read()
    print(contents)
```

With this approach, the file is automatically closed when the with block is exited, even if an error occurs.

### CSV Library

#### Reading data from a CSV file

To read data from a CSV file, you can use the csv.reader function. This function returns an iterator that you can loop over to access the rows of the CSV file. Here is an example:

```python
import csv

with open('my_file.csv', 'r') as csv_file:
    csv_reader = csv.reader(csv_file)

    # Loop over each row in the CSV file
    for row in csv_reader:
        # Access the values of each row
        print(row)
```

This code opens the my_file.csv file in read mode and creates a csv.reader object using the csv.reader function. Then, it loops over each row in the CSV file and prints out the values of each row.

#### Writing data to a CSV file

To write data to a CSV file, you can use the csv.writer function. This function takes a file object and returns a writer object that you can use to write rows to the CSV file. Here is an example:

```python
import csv

with open('my_file.csv', 'w', newline='') as csv_file:
    csv_writer = csv.writer(csv_file)

    # Write rows to the CSV file
    csv_writer.writerow(['Title', 'Author', 'Publisher'])
    csv_writer.writerow(['To Kill a Mockingbird', 'Harper Lee', 'Grand Central Publishing'])
```

This code opens the my_file.csv file in write mode and creates a csv.writer object using the csv.writer function. Then, it writes two rows to the CSV file.


#### Appending data to a CSV file

To append data to a CSV file, you can use the csv.writer function with the a mode. This mode will open the file in append mode, which allows you to add new rows to the end of the file. Here is an example:

```python
import csv

with open('my_file.csv', 'a', newline='') as csv_file:
    csv_writer = csv.writer(csv_file)

    # Append rows to the CSV file
    csv_writer.writerow(['1984', 'George Orwell', 'Signet Classic'])
```

This code opens the my_file.csv file in append mode and creates a csv.writer object using the csv.writer function. Then, it appends a new row to the end of the CSV file.

#### Removing data from a CSV file

To remove data from a CSV file, you will need to read in the entire file, filter out the rows that you don't want, and then write the filtered rows back to the CSV file. Here is an example:

```python
import csv

# Read in the CSV file and filter out the rows that match a specific criteria
with open('my_file.csv', 'r') as csv_file:
    csv_reader = csv.reader(csv_file)

    filtered_rows = []
    for row in csv_reader:
        if row[1] != 'Harper Lee':
            filtered_rows.append(row)

# Write the filtered rows back to the CSV file
with open('my_file.csv', 'w', newline='') as csv_file:
    csv_writer = csv.writer(csv_file)

    for row in filtered_rows:
        csv_writer.writerow(row)
```
This code reads in the my_file.csv file and filters out the rows where the author is "Harper Lee". Then, it opens the same file in write mode and writes the filtered rows back to the CSV file.
