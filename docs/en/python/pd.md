## Pandas
					
### Introduction to Pandas

Pandas is a Python library used for working with data sets. It has functions for analyzing, cleaning, exploring, and manipulating data. 

<br />
						
Pandas allows us to :

- Analyze big data and make conclusions based on statistical theories.
- Clean messy data sets, and make them readable and relevant. Relevant data is very important in data science. 
- Pandas gives you answers about the data Such as : Is there a correlation between two or more columns? What is the average value? Max value? Min value?
- Manage diffrent data sets merging them filtering them etc...

<br />
						
Pandas is also able to delete rows that are not relevant, or contain wrong values, like empty or NULL values. This is called cleaning the data.

<br />

To start using Pandas, you first need to install it. You can install it using the following command:

```python
!pip install pandas
```

After installing pandas, you can import it as follows: 

```python
import pandas as pd
```
### Pandas Data Structures 

Pandas provides two main data structures: Series and DataFrame. A Series is a one-dimensional array-like object that can hold any data type. A DataFrame is a two-dimensional table that can hold data of different types in columns.


#### Series

A Pandas Series is like a column in a table. It is a one-dimensional array holding data of any type

```python					
import pandas as pd
dataset1=[1, 3, 5, np.nan, 6, 8]
df1=pd.Series(dataset1)
print (df1)
```

If nothing else is specified, the values are labeled with their index number. The first value has index 0, etc. This label can be used to access a specified value. With the index argument, you can name your own labels. When using labels, you can access an item by using the label. Code example : 

```python
import pandas as pd
dataset1=[1, 3, 5, np.nan, 6, 8]
df1=pd.Series(dataset1, index=["a","b","c","e","f","g"])
print (df1)						
print (df1["a"])
```

We can also use key/value pair objects like dictionaries when creating a Series

<br />

Note: The keys of the dictionary become the labels

<br />
						
Code example :
```python
import pandas as pd
dataset1={"Vehicle number":1, "Wheels":4, "Doors":4}
df1=pd.Series(dataset1)
print (df1)
```			

#### DataFrame

Data sets in Pandas are usually multi-dimensional tables, called DataFrames. Series can be considered to be like a column in a table, whereas a DataFrame can be considered to be the table. 

<br />

A DataFrame can be created in several ways. One way is to pass a dictionary of lists to the pd.DataFrame() function. Each key in the dictionary represents a column name, and each list represents the data in that column.

```python
data = {'name': ['Alice', 'Bob', 'Charlie', 'David'],
        'age': [25, 32, 18, 47],
        'gender': ['F', 'M', 'M', 'M']}
df = pd.DataFrame(data)
print(df)
```					
We can use the loc attribute to locate one or more rows. Just as with series, we can name indexes of data frames and locate them using the loc attribute

<br />

Code example :

```python
import pandas as pd
dataset1={"Vehicle number":[1,2,3], "Wheels":[4,2,4], "Doors":[4,0,5]}
df1=pd.DataFrame(dataset1, index=["Car","Motorcycle","Van"])
print(df1)
print (df1.loc["Car"])				
```

### Data Import and Export

Pandas supports reading and writing data from and to various file formats, such as CSV, Excel, SQL databases, and more.

- **CSV** : A simple way to store big data sets is to use CSV (Comma Separated Value) files. CSV files contain plain text and are a well know format that can be read by almost all software including Pandas. To read a CSV file, you can use the pd.read_csv() function. The function takes the path to the CSV file as an argument and returns a DataFrame. To write a DataFrame to a CSV file, you can use the to_csv() method. Code example :

```python
import pandas as pd

## Importing a CSV file
df = pd.read_csv('data.csv')
print(df)

## Export a CSV file
df.to_csv('data.csv', index=False)#The index=False argument tells Pandas not to write the row index to the CSV file.
```


- **Excel** : To read an Excel file, you can use the pd.read_excel() function. The function takes the path to the Excel file as an argument and returns a DataFrame. To write a DataFrame to an Excel file, you can use the to_excel() method.

```python
## Importing a Excel file
df = pd.read_excel('data.xlsx', sheet_name='Sheet1')
print(df)

## Export a Excel file
df.to_excel('data.xlsx', sheet_name='Sheet1', index=False)
```
- **SQL Databases** : To read data from a SQL database, you can use the pd.read_sql() function. The function takes a SQL query and a connection object as arguments and returns a DataFrame. To write a DataFrame to a SQL database, you can use the to_sql() method.



```python
import sqlite3

## Importing a SQL file
conn = sqlite3.connect('mydatabase.db')
query = 'SELECT * FROM mytable'
df = pd.read_sql(query, conn)
print(df)

## Export a SQL file
df.to_sql('mytable', conn, if_exists='replace', index=False) #The if_exists='replace' argument tells Pandas to replace the table if it already exists in the database.
```

### Getting a quick overview of the Dataframes content
						
- One of the most used method for getting a quick overview of the DataFrame, is the head() method. The head() method returns the headers and a specified number of rows, starting from the top. 

- The tail() method returns the last rows of the DataFrame

						
- The DataFrame object has a method called info(), that gives you more information about the data set.

Code Example :

```python
import pandas as pd 
df=pd.read_csv("data.csv") 
print (df.head())
print (df.tail()) 
df.info()
```						
### Data Cleaning with pandas

Data cleaning means fixing bad data in your data set it is an essential step in data analysis. Pandas provides several functions and methods for cleaning and preparing data. Examples of bad data include : 

#### Empty cells 

Empty cells can potentially give you a wrong result when you analyze data. One way to deal with empty cells is to remove rows that contain empty cells. This usually works since data sets can be very large, and removing a few rows will not have a significant impact on the results. 

<br />

To detect empty cells we can use : 

```python
df.isnull().value_counts()
```						
To remove empty cells, we can use the dropna() method. By default, the dropna() method returns a new DataFrame, and will not change the original. If you want to change the original DataFrame, use the inplace = True argument. Code Example :

```python
import pandas as pd						
df=pd.read_csv("data.csv")
print (df)
df.dropna(inplace=True)
print (df)
```															
Another way of dealing with empty cells is to insert a new value to replace the empty cell. This way you do not have to delete entire rows just because of some empty cells. The fillna() method allows us to replace empty cells with a value. To only replace empty values for one column, specify the column name for the DataFrame. Code Example :

```python
import pandas as pd
df=pd.read_csv("data.csv")
df.fillna(130, inplace=True)
df["Calories"].fillna(130, inplace=True)
print(df.to_string())
```

You can also fill in empty cells with the mean, median or mode of the column. Pandas uses the mean() median() and mode() methods to calculate the respective values for a specified column
						
- Mean : the mean is the average value (the sum of all values divided by number of values)
- Median : the median is the value in the middle, after you have sorted all values ascending
- Mode : the mode is the value that appears most frequently
				
Code Example : 
								
```python
import pandas as pd

df=pd.read_csv("data.csv")

meancal=df["Calories"].mean()
mediancal=df["Calories"].median()
modecal=df["Calories"].mode()

print ("The mean of calories is" + str(meancal) + " The median of calories is " + str(mediancal) + " The mode of calories is " + str(modecal)) 

meandf=df["Calories"].fillna(meancal)
mediandf=df["Calories"].fillna(mediancal)
modedf=df["Calories"].fillna(modecal)
						
print(meandf.to_string())
print(mediandf.to_string())
print(modedf.to_string())
```
#### Data in wrong format 

Cells with data of incorrect format can make it difficult, or even impossible, to analyze data.

<br />

To remedy this, you can either remove the rows, or convert all cells in the columns into the same format
				
#### Incorrect Data
						
Incorrect data does not have to be empty cells or incorrect format, it can just be incorrect, like if someone entered 199 instead of 1.99. Sometimes you can spot incorrect data by looking at the data set because you have an expectation of what it should be.

<br />
						
If you take a look at our data set you can see that in row 7 the duration is 450, but for all the other rows the duration is between 30 and 60. It doesn't have to be incorrect, but taking in consideration that this is the data set of someone's workout sessions, we conclude this person did not work out for 450 minutes

<br />


One way to fix wrong values is to replace them with something else. For small data sets you might be able to replace the wrong data one by one, but not for large data sets. To replace wrong data for larger data sets you can create some rules and set some boundaries for legal values, and replace any values that are outside of the boundaries

<br />
						
Another way of handling incorrect data is to remove the rows that contains incorrect data. This way you do not have to find out what to replace them with, and there is a good chance you do not need them for analysis.

<br />
			
Code Example
						
```python
import pandas as pd 

df1=pd.read_csv("data.csv")
df2=pd.read_csv("data.csv")

						
for x in df1.index: #replace all values in duration above 120 with 120 
	if df1.loc[x, "Duration"] > 120:
		df1.loc[x,"Duration"] = 120

for y in df2.index: #drop all values above 120
	if df2.loc[y, "Duration"] >120:
		df2.drop(y, inplace = True)	

print(dataframe1.to_string())
print(dataframe2.to_string())
```											

#### Wrong data Duplicates
						
Duplicate rows are rows that have been entered more than once. By taking a look at our test data set, we can assume that row 11 and 12 are duplicates

<br />
						
To discover duplicates, we can use the duplicated() method The duplicated() method returns a Boolean values for each row To remove duplicates, use the drop_duplicates() method. Code Example :
						
```python
import pandas as pd
df1=pd.read_csv("data.csv")
						
df1.duplicated().value_counts() #search for duplicates and output true when found 

df1.drop_duplicates(inplace = True) #drop all duplicates

df1
```

### Merging and Joining Data

Pandas provides several functions and methods for merging and joining DataFrames.

<br />

To merge two DataFrames based on a common column, you can use the merge() method.

```python
df1 = pd.DataFrame({'id': [1, 2, 3, 4], 'name': ['Alice', 'Bob', 'Charlie', 'David']})
df2 = pd.DataFrame({'id': [1, 2, 3, 4], 'age': [25, 32, 18, 47]})
merged_df = pd.merge(df1, df2, on='id')  # Merge DataFrames on 'id' column
print(merged_df)
```

The merge() method merges the two DataFrames based on the 'id' column. The result is a new DataFrame with columns from both DataFrames.

<br />

To join two DataFrames based on a common column, you can use the join() method.

```python
df1 = pd.DataFrame({'id': [1, 2, 3, 4], 'name': ['Alice', 'Bob', 'Charlie', 'David']})
df2 = pd.DataFrame({'id': [1, 2, 3, 4], 'age': [25, 32, 18, 47]})
joined_df = df1.set_index('id').join(df2.set_index('id'))  # Join DataFrames on 'id' column
print(joined_df)
```

The join() method joins the two DataFrames based on the 'id' column. The result is a new DataFrame with columns from both DataFrames.

### Data Analysis in Pandas

#### Describe in Pandas


The describe() method in Pandas provides summary statistics of a DataFrame. It calculates several common statistics for each numerical column in the DataFrame, such as count, mean, standard deviation, minimum, maximum, and quartiles. Code Example :

```python						
import pandas as pd
df1=pd.read_csv("data.csv") 
df1.describe()
```

#### Correlation in Pandas
The corr() method calculates the relationship between each column in your data set. The corr() method ignores "not numeric" columns. Code Example :	

```python						
import pandas as pd
df1=pd.read_csv("data.csv") 
df1.corr()
```
<br />

The Result of the corr() method is a table with a lot of numbers that represents how well the relationship is between two columns. The number varies from -1 to 1 such as :

- 1 means that there is a 1 to 1 relationship (a perfect correlation), and for this data set, each time a value went up in the first column, the other one went up as well.

- 0.9 is also a good relationship, and if you increase one value, the other will probably increase as well.
				
- -0.9 would be just as good relationship as 0.9, but if you increase one value, the other will probably go down
						
- 0.2 means NOT a good relationship, meaning that if one value goes up does not mean that the other will
						
What is a good correlation? It depends on the use, but I think it is safe to say you have to have at least 0.6 (or -0.6) to call it a good correlation

<br />
				
**Perfect Correlation**: We can see that "Duration" and "Duration" got the number 1.000000, which makes sense, each column always has a perfect relationship with itself.
				
**Good Correlation**: "Duration" and "Calories" got a 0.922721 correlation, which is a very good correlation, and we can predict that the longer you work out, the more calories you burn, and the other way around: if you burned a lot of calories, you probably had a long work out
						
**Bad Correlation**: "Duration" and "Maxpulse" got a 0.009403 correlation, which is a very bad correlation, meaning that we can not predict the max pulse by just looking at the duration of the work out, and vice versa