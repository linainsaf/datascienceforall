## Seaborn 

Seaborn is a Python data visualization library based on matplotlib. It provides a high-level interface for drawing attractive and informative statistical graphics. Seaborn helps you explore and understand your data. Its plotting functions operate on dataframes and arrays containing whole datasets and internally perform the necessary semantic mapping and statistical aggregation to produce informative plots. 

<br />

Its dataset-oriented, declarative API lets you focus on what the different elements of your plots mean, rather than on the details of how to draw them.



### Installing and importing Seaborn

- In your terminal type

```python
pip install seaborn
```

- If using jupyter type : 

```python
!pip install seaborn
```

- To import Seaborn in your code : 

```python
import seaborn as sns
```

### Loading Data from Seaborn 

First, we need to import Seaborn and load some data to work with. In this course, we will use the tips dataset, which is included in Seaborn. The tips dataset contains information about the bills and tips at a restaurant.

``` python
import seaborn as sns
import matplotlib.pyplot as plt

tips = sns.load_dataset('tips')
```

The tips dataset has several columns:

- total_bill: the total bill amount (including tip)
- tip: the tip amount
- sex: the gender of the person who paid the bill (male or female)
- smoker: whether the person was a smoker or not (yes or no)
- day: the day of the week
- time: whether the meal was lunch or dinner
- size: the number of people in the party

### Creating Plots with Seaborn

#### Scatter Plot

One of the most basic types of plot is a scatter plot, which shows the relationship between two variables. We can create a scatter plot using Seaborn's scatterplot function.

``` python
sns.scatterplot(x='total_bill', y='tip', data=tips)
plt.show()
```
This will create a scatter plot of the total bill versus the tip amount.

#### Line Plot

Another type of plot is a line plot, which shows the relationship between two variables over time. We can create a line plot using Seaborn's lineplot function.

``` python
sns.lineplot(x='size', y='total_bill', data=tips)
plt.show()
```

This will create a line plot of the size versus the total bill amount over time.

#### Bar Plot

A bar plot is useful for showing comparisons between different categories. We can create a bar plot using Seaborn's barplot function.

``` python
sns.barplot(x='day', y='total_bill', data=tips)
plt.show()
```

This will create a bar plot of the total bill for each day of the week.

#### Histogram

A histogram is useful for showing the distribution of a single variable. We can create a histogram using Seaborn's histplot function.

```python
sns.histplot(x='total_bill', data=tips)
plt.show()
```
This will create a histogram of the total bill.

#### Box Plot

A box plot is useful for showing the distribution of a variable across different categories. We can create a box plot using Seaborn's boxplot function.

```python
sns.boxplot(x='day', y='total_bill', data=tips)
plt.show()
```

This will create a box plot of the total bill for each day of the week.

#### Violin Plot 

A violin plot is similar to a box plot, but it also shows the distribution of the variable. We can create a violin plot using Seaborn's violinplot function.

```python
sns.violinplot(x='day', y='total_bill', data=tips)
plt.show()
```

This will create a violin plot of the total bill for each day of the week.

#### Pair Plots 

Seaborn's pairplot function can be used to create scatter plots of pairs of variables in a dataset. By default, it plots the scatter plot between each pair of numerical variables, and a histogram of each variable on the diagonal. It is a useful tool to visualize the relationships between multiple variables at once. Example :

```python
import seaborn as sns
import pandas as pd

iris = sns.load_dataset('iris')
sns.pairplot(iris, hue='species')
```

#### Heatmaps

Seaborn's heatmap function allows you to create a heatmap of a matrix of values, with each cell colored according to its value. Heatmaps are useful for visualizing large amounts of data and identifying patterns within the data. Example : 


```python
import seaborn as sns
import numpy as np

# Create a matrix of random values
data = np.random.rand(10, 10)

# Create a heatmap
sns.heatmap(data)
```

#### FacetGrid

Seaborn's FacetGrid function allows you to create a grid of plots based on the levels of one or more categorical variables. This is useful when you want to visualize relationships between variables for different groups or categories. Example:

```python
import seaborn as sns
import pandas as pd

# Load the tips dataset
tips = sns.load_dataset("tips")

# Create a FacetGrid
g = sns.FacetGrid(tips, col="time", row="sex")

# Map the plot to the FacetGrid
g.map(sns.scatterplot, "total_bill", "tip")
```


### Styling Plots

Seaborn provides several functions for styling your plots, including setting the plot background, changing the color palette, and adjusting the font size and style. Example:

```python
import seaborn as sns
import matplotlib.pyplot as plt

# Set the plot background color
sns.set_style("darkgrid")

# Load the tips dataset
tips = sns.load_dataset("tips")

# Set the color palette
sns.set_palette("husl")

# Create a scatter plot
sns.scatterplot(data=tips, x="total_bill", y="tip", hue="sex", size="size")

# Set the title and axis labels
plt.title("Tips by Total Bill")
plt.xlabel("Total Bill")
plt.ylabel("Tip")

# Set the font size and style
sns.set(font_scale=1.5, font="Arial")
```
