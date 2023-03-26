# NumPy 
NumPy is short for Numerical Python. It is a Python library/package used for working with arrays which contains classes, functions, variables , a large library of mathematical functions etc for working with scientific calculation. It can be used to create an “n” dimensional array where “n” is any integer.				
## Why NumPy
In Python we have lists that serve the purpose of arrays, but they are slow. NumPy aims to provide an array object that is up to 50x faster than a traditional Python list.

<br />

The array object in NumPy is called ndarray, it provides a lot of supporting functions that make working with ndarray very easy. Arrays are very frequently used in data science, where speed and resources are very important.		

<br />

What makes NumPy arrays faster than lists: NumPy arrays are stored at one continuous place in memory unlike lists, so processes can access and manipulate them very efficiently. This behavior is called locality of reference. This is the main reason why NumPy is faster than lists. It is also optimized to work with the latest CPU architectures.

## Installing NumPy
To install NumPy, you can use pip, the Python package installer. Open your terminal or command prompt and enter the following command:

```python
pip3 install numpy
```	

## Importing NumPy

There are two ways to import NumPy. Code Example : 

```python
# this will import the entire NumPy module.
import numpy as np
# this will import all class, objects, variables etc from the NumPy package   
from numpy import* 	
```
NumPy is usually imported under the np alias. 

<br />

alias: In Python aliases are an alternate name for referring to the same thing. 

<br />

## Creating NumPy Arrays
 
The array object in NumPy is called ndarray. We can create a NumPy ndarray object by using the array() function. NumPy arrays can be created in a number of ways. Here are some of the most common methods:

- Using the numpy.array() function to create an array from a list/tuple:

```python
a = np.array([1, 2, 3])
```
- Using the numpy.zeros() function to create an array filled with zeros:

```python
b = np.zeros((2, 3))
```
- Using the numpy.ones() function to create an array filled with ones:

```python
c = np.ones((2, 3))
```

- Using the numpy.random.randint(): Returns an array of random integers between the two given numbers

```python
d = np.random.randint(0, 10)
```
- Using the numpy.random.rand() function to create an array of random values:

```python
e = np.random.rand(2, 3)
```

## NumPy Array Dimensions

A dimension in arrays is one level of array depth (nested arrays). nested array: are arrays that have arrays as their elements. 0-D Arrays 1-D Arrays 2-D Arrays 3-D Arrays.

<br />

0-D arrays, or Scalars, are the elements in an array. Each value in an array is a 0-D array. 

<br />

Code Example:

```python
arr=np.array(30) 
print (arr)
```

An array that has 0-D arrays as its elements is called a uni-dimensional or 1-D array. These are the most common types of arrays. 

<br />

Code Example :

```python
 arr=np.array([30,35,38,40,46,52])
 print (arr)
```

An array that has 1-D arrays as its elements is called a 2-D array. These are often used to represent matrix or 2nd order tensors. 

<br />

Code Example :

```python
arr=np.array([[30,35,38,40,46,52],[22,28,39,42,49,52]])
 print (arr)
```

An array that has 2-D arrays (matrices) as its elements is called 3-D array. These are often used to represent a 3rd order tensor. 

<br />

Code Example : 

```python
arr=np.array([[[30,35,38,40,46,52],[22,28,39,42,49,52], [71,24,88,64,31,94]]])
print (arr)
```

NumPy Arrays provides the ndim attribute that returns an integer that tells us how many dimensions the array has. 

<br />

Code Example :

```python
arr=np.array([[[30,35,38,40,46,52],[22,28,39,42,49,52], [71,24,88,64,31,94]]])
print (arr.ndim)
```						
An array can have any number of dimensions. When the array is created, you can define the number of dimensions by using the ndim argument. 

<br />

Code Example : 

```python
arr=np.array([1,2,3,4],ndmin=6)
print (arr.ndim)
print (“Number of dimensions:”, arr.ndim)
```
## Arrays type and shape 

**NumPy Array Data Type** :The NumPy array object has a property called dtype that returns the data type of the array.

<br />

Code Example :

```python
arr=np.array([1,2,3,4,5]) 
print (arr.dtype)
```
**NumPy Array Shape** : The shape of an array is the number of elements in each dimension. NumPy arrays have an attribute called shape that returns a tuple with each index having the number of corresponding elements

<br />

Code example :

```python
arr=np.array([1,2,3,4,5,6])
print (arr.shape)
```		
## NumPy Array Indexing

Array indexing is the same as accessing an array element. You can access an array element by referring to its index number. The indexes in NumPy arrays start with 0, meaning that the first element has index 0, and the second has index 1 etc.

<br />

Code Example : 

```python
arr=np.array([1,2,3,4])
print (arr[0])
```
To access elements from 2-D arrays we can use comma separated integers representing the dimension and the index of the element.

<br />

Code Example :

```python
arr=np.array([[1,2,3,4],[4,3,2,1]])
print (arr[0][1])

```				
To access elements from 3-D arrays we can use comma separated integers representing the dimensions and the index of the element. 

<br />

Code Example : 

```python
arr=np.array([1,2,3,4],[4,3,2,1],[8,6,7,9])
print (arr[0][1][0])
```				
Use negative indexing to access an array from the end
Code Example

```python
arr=np.array([1,2,3,4],[4,3,2,1],[8,6,7,9])
print (arr[0][-1][-2])
```

## NumPy Array Slicing 
						
Slicing in python means taking elements from one given index to another given index. We pass slice instead of index like this [start: end]. We can also define the step, like this [start:end:step].
						
- If we don't pass start its considered 0
- If we don't pass end its considered length of array in that dimension
- If we don't pass step its considered 1

Note: The result includes the start index, but excludes the end index. Use the minus operator to refer to an index from the end.

<br />

Code Example :

```python
arr=np.array([1,2,3,4,5])
print (arr[0:5]) #slice arrays from index 0 to 5 excluding 5 print (arr[:4]) #slice from beginning to 4 excluding 4
print (arr[2:]) #slice from index 2 onwards
print (arr[:-3]) #slice from index -3
```	
Code Example :

```python
arr=np.array([1,2,3,4,5])
print (arr[:4:1]) #slice from beginning to 4 step of 1 print (arr[0::3]) #slice from index 0 onwards step of 3 print (arr[:-1:2]) #slice from index -1 step of 2
```										
## Array Operations in NumPy

**Element-wise Operations** : NumPy allows you to perform element-wise operations on arrays. Here are some examples:

```python
a = np.array([1, 2, 3])
b = np.array([4, 5, 6])

# Element-wise addition
c = a + b
print(c)  # Output: [5 7 9]

# Element-wise multiplication
d = a * b
print(d)  # Output: [ 4 10 18]
```

**Matrix Multiplication** : NumPy allows you to perform matrix multiplication using the numpy.dot() function. Here is an example:

```python
a = np.array([[1, 2], [3, 4]])
b = np.array([[5, 6], [7, 8]])

# Matrix multiplication
c = np.dot(a, b)
print(c)
```

**NumPy Array Reshaping** : Reshaping means changing the shape of an array. The shape of an array is the number of elements in each dimension. By reshaping we can add or remove dimensions or change number of elements in each dimension.

<br />
						
Code Example :

```python					
# we can do it this way
arr=np.array([1,2,3,4,5,6,7,8,9,10])
arr2= arr.reshape(5,2)
print (arr)
print (arr2)
# or this way
a = np.array([1, 2, 3, 4, 5, 6])
b = np.reshape(a, (2, 3))
print (a)
print (b)
```

**Transposing Arrays** : NumPy provides a way to transpose arrays using the numpy.transpose() function. Here is an example:

```python
a = np.array([[1, 2], [3, 4], [5, 6]])
b = np.transpose(a)
print(b)
```
**Aggregation Functions** : NumPy provides several aggregation functions that can be used to compute statistics on arrays. Here are some examples:

```python
a = np.array([1, 2, 3, 4, 5])

# Computing the sum of the elements in the array
print(np.sum(a))  # Output: 15

# Computing the mean of the elements in the array
print(np.mean(a))  # Output: 3.0

# Computing the standard deviation of the elements in the array
print(np.std(a))  # Output: 1.41421356
```
## NumPy Array Copy and View

The main difference between a copy and a view of an array is that the copy is a new array, and the view is just a view, or link to, the original array.

<br />

The copy owns the data and any changes made to the copy will not affect original array, and any changes made to the original array will not affect the copy.

<br />

The view does not own the data and any changes made to the view will affect the original array, and any changes made to the original array will affect the view.

<br />

As mentioned, copies owns the data, and views does not own the data, so how can we check if it owns the data or not?

<br />

Every NumPy array has the attribute base that returns None if the array owns the data. Otherwise, the base attribute refers to the original object

<br />

Code example :

```python
import numpy as np
arr=np.array([1,2,3,4,5,6])
arrview=arr.view() #creates a view of the array arrcopy=arr.copy() #creates a copy of the array
print (arrview.base) #check if the array owns its data print (arrcopy.base) #check if the array owns its data (should output none)
```

[**Documentations NumPy**](https://numpy.org/doc/1.24/reference/index.html)