## Pandas
						
Pandas is a Python library used for working with data sets. It has functions for analyzing, cleaning, exploring, and manipulating data. The name "Pandas" has a reference to both "Panel Data", and "Python Data Analysis" and was created by Wes McKinney in 2008

<br />
						
Pandas allows us to :

- Analyze big data and make conclusions based on statistical theories.
- Clean messy data sets, and make them readable and relevant. Relevant data is very important in data science. 
- Pandas gives you answers about the data Such as : Is there a correlation between two or more columns? What is the average value?Max value? Min value?
- Manage diffrent data sets merging them filtering them etc...

<br />
						
Pandas is also able to delete rows that are not relevant, or contain wrong values, like empty or NULL values. This is called cleaning the data.

<br />
				
There are two ways to import pandas : 

```
import pandas #This will import the entire pandas module.
						
from pandas import* # This will import all class, objects, variables etc. from pandas package
```
### Pandas Series

A Pandas Series is like a column in a table. It is a one-dimensional array holding data of any type

```						
import pandas as pd
dataset1=[1,2,3]
df1=pd.Series(dataset1)
print (df1)
```						
### Pandas Labels

If nothing else is specified, the values are labeled with their index number. The first value has index 0, etc. This label can be used to access a specified value. With the index argument, you can name your own labels. When using labels, you can access an item by using the label. Code example : 

```
import pandas as pd
dataset1=[1,2,3]
df1=pd.Series(dataset1, index=["a","b","c"])
print (df1)						
print (df1["a"])
```

### Pandas Key Value objects

We can also use key/value pair objects like dictionaries when creating a Series

<br />

Note: The keys of the dictionary become the labels

<br />
						
Code example :
```
import pandas as pd
dataset1={"Vehicle number":1, "Wheels":4, "Doors":4}
df1=pd.Series(dataset1)
print (df1)
```			
### Pandas Data Frames

Data sets in Pandas are usually multi-dimensional tables, called DataFrames. Series can be considered to be like a column in a table, whereas a DataFrame can be considered to be the table. 

<br />
						
We can use the loc attribute to locate one or more rows. Just as with series, we can name indexes of data frames and locate them using the loc attribute

<br />

Code example :

```
import pandas as pd
dataset1={"Vehicle number":[1,2,3], "Wheels":[4,2,4], "Doors":[4,0,5]}
df1=pd.DataFrame(dataset1, index=["Car","Motorcycle","Van"])
print(df1)
print (df1.loc["Car"])				
```
### Reading CSV Files with Pandas
									
A simple way to store big data sets is to use CSV (Comma Separated Value) files. CSV files contain plain text and are a well know format that can be read by almost all software including Pandas

<br />
						
If your data sets are stored in a file, Pandas can load them into a DataFrame. Code example :

```
import pandas as pd 
dataframe1=pd.read_csv(‘data.csv’)
print(dataframe1.to_string())
```					
### Getting a quick overview of the Dataframes content
						
- One of the most used method for getting a quick overview of the DataFrame, is the head() method. The head() method returns the headers and a specified number of rows, starting from the top. 

- The tail() method returns the last rows of the DataFrame

						
- The DataFrame object has a method called info(), that gives you more information about the data set.

Code Example :

```
import pandas as pd 
dataframe1=pd.read_csv("data.csv") 
print (dataframe1.head())
print (dataframe1.tail()) 
dataframe1.info()
```						
### Data Cleaning With pandas

Data cleaning means fixing bad data in your data set Examples of bad data include : 

#### Empty cells 

Empty cells can potentially give you a wrong result when you analyze data. One way to deal with empty cells is to remove rows that contain empty cells. This usually works since data sets can be very large, and removing a few rows will not have a significant impact on the results.

<br />
						
To remove empty cells, we can use the dropna() method. By default, the dropna() method returns a new DataFrame, and will not change the original. If you want to change the original DataFrame, use the inplace = True argument. Code Example :

```
import pandas as pd						
dataframe1=pd.read_csv("data.csv")
print (dataframe1)
dataframe1.dropna(inplace=True)
print (dataframe1)
```															
Another way of dealing with empty cells is to insert a new value to replace the empty cell. This way you do not have to delete entire rows just because of some empty cells. The fillna() method allows us to replace empty cells with a value. To only replace empty values for one column, specify the column name for the DataFrame. Code Example :

```
import pandas as pd
dataframe1=pd.read_csv("data.csv")
dataframe1.fillna(130, inplace=True)
dataframe1["Calories"].fillna(130, inplace=True)
print(dataframe1.to_string())
```

You can also fill in empty cells with the mean, median or mode of the column. Pandas uses the mean() median() and mode() methods to calculate the respective values for a specified column
						
- Mean : the mean is the average value (the sum of all values divided by number of values)
- Median : the median is the value in the middle, after you have sorted all values ascending
- Mode : the mode is the value that appears most frequently
				
Code Example : 
								
```
import pandas as pd

dataframe1=pd.read_csv("data.csv")

meancal=dataframe1["Calories"].mean()
mediancal=dataframe1["Calories"].median()
modecal=dataframe1["Calories"].mode()

print ("The mean of calories is" + str(meancal) + " The median of calories is " + str(mediancal) + " The mode of calories is " + str(modecal)) 

meandf=dataframe1["Calories"].fillna(meancal)
mediandf=dataframe1["Calories"].fillna(mediancal)
modedf=dataframe1["Calories"].fillna(modecal)
						
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
						
```
import pandas as pd 

dataframe1=pd.read_csv("data.csv")
dataframe2=pd.read_csv("data.csv")

print(dataframe1.to_string())
print(dataframe2.to_string())
						
for x in dataframe1.index: #replace all values in duration above 120 with 120 
	if dataframe1.loc[x, "Duration"] > 120:
		dataframe1.loc[x,"Duration"] = 120

for y in dataframe2.index: #drop all values above 120
	if dataframe2.loc[y, "Duration"] >120:
		dataframe2.drop(y, inplace = True)
						
print(dataframe1.to_string())
print(dataframe2.to_string())
```											

#### Wrong data Duplicates
						
Duplicate rows are rows that have been entered more than once. By taking a look at our test data set, we can assume that row 11 and 12 are duplicates

<br />
						
To discover duplicates, we can use the duplicated() method The duplicated() method returns a Boolean values for each row To remove duplicates, use the drop_duplicates() method. Code Example :
						
```
import pandas as pd
dataframe1=pd.read_csv("data.csv")
						
print(dataframe1.duplicated()) #search for duplicates and output true when found 

dataframe1.drop_duplicates(inplace = True) #drop all duplicates

print(dataframe1.to_string()) #find and drop duplicates example
```
### Data Analysis in Pandas

#### Correlation in Pandas
The corr() method calculates the relationship between each column in your data set. The corr() method ignores "not numeric" columns. Code Example :	

```						
import pandas as pd
dataframe1=pd.read_csv("data.csv") 
dataframe1.corr()
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