## Matplotlib

Matplotlib is a popular data visualization library in Python that provides a wide range of tools for creating static, animated, and interactive visualizations. It is widely used in scientific computing, data analysis, and machine learning.

<br />

Matplotlib allows you to create a variety of plots, including line plots, scatter plots, bar plots, histogram plots, and more. It provides a high level of customization to create professional-looking plots with minimal coding.

<br />

In this course, we will cover the basics of Matplotlib and walk through several examples to illustrate how to create different types of plots using Matplotlib.

### Installing and importing Matplotlib

From the terminal type: 

```python
pip install matplotlib
```
For Jupyter in the command prompt type: 

```python
!pip install matplotlib
```

Matplotlib is available preinstalled on Anaconda3 Jupyter Notebook.

<br />

Once Matplotlib is installed, import it in your applications by adding the import module statement. You can also check which version of Matplotlib you have installed. Code example :

```python
import matplotlib #importing matplotlib into your code 

print(matplotlib.__version__) #checking installed version Matplotlib
```

Most of Matplotlib’s utility is in the pyplot sub-module, and its usually imported under the plt alias. Code Example :

```python
import matplotlib.pyplot as plt
```

Now let's create a simple plot using Matplotlib. We will plot a line chart of the sine function between 0 and 2π.

```python
import matplotlib.pyplot as plt
import numpy as np

# Generate data
x = np.linspace(0, 2*np.pi, 100)
y = np.sin(x)

# Plot data
plt.plot(x, y)

# Show plot
plt.show()
```

In this example, we first generated the data by creating an array x using the linspace function of NumPy, which creates 100 evenly spaced numbers between 0 and 2π. We then computed the sine function of x using the NumPy sin function and stored it in an array y.

<br />

To create the plot, we called the plot function of Matplotlib and passed x and y as arguments. This created a line chart of the sine function. Finally, we called the show function to display the plot.

### Basic Plotting with Matplotlib

#### Line Plot

The most basic type of plot in Matplotlib is a line plot, which displays data as a series of points connected by straight lines. To create a line plot in Matplotlib, we use the plot function, which takes two arrays of data as arguments: x and y

```python
import matplotlib.pyplot as plt

# Data
x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]

# Plot data
plt.plot(x, y)

# Show plot
plt.show()
```
#### Scatter Plot

A scatter plot is a type of plot that displays data as a collection of points. To create a scatter plot in Matplotlib, we use the scatter function, which takes two arrays of data as arguments: x and y.

```python
import matplotlib.pyplot as plt

# Data
x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]

# Plot data
plt.scatter(x, y)

# Show plot
plt.show()
```

In this example, we used the `scatter` function of Matplotlib to create a scatter plot of the data. We passed `x` and `y` as arguments to the function, which created a scatter plot with five points.


#### Bar Plot 

A bar plot is a type of plot that displays data as a series of bars. To create a bar plot in Matplotlib, we use the `bar` function, which takes two arrays of data as arguments: `x` and `y`.

```python
import matplotlib.pyplot as plt

# Data
x = ['A', 'B', 'C', 'D', 'E']
y = [2, 4, 6, 8, 10]

# Plot data
plt.bar(x, y)

# Show plot
plt.show()
```

In this example, we used the bar function of Matplotlib to create a bar plot of the data. We passed x and y as arguments to the function, which created a bar plot with five bars. If you want the bars to be displayed horizontally instead of vertically, use the barh() function. 

#### Histogram Plot

A histogram plot is a type of plot that displays the distribution of a dataset. To create a histogram plot in Matplotlib, we use the hist function, which takes an array of data as argument: x.

```python
import matplotlib.pyplot as plt
import numpy as np

# Data
x = np.random.randn(1000)

# Plot data
plt.hist(x, bins=20)

# Show plot
plt.show()
```

In this example, we first generated random data using the NumPy randn function. We then used the hist function of Matplotlib to create a histogram plot of the data. We passed x as an argument to the function and set the number of bins to 20 using the bins parameter.


#### Pie Charts Plot

With Pyplot, you can use the pie() function to draw pie charts. Code Example :

```python
import matplotlib.pyplot as plt
import numpy as np

# Data
y = np.array([35, 25, 25, 15])
plt.pie(y) 
plt.show()
```
In this example, we created two lists x and y containing five numbers each. We then called the plot function and passed x and y as arguments. This created a line plot of the data.

### Customizing Plots with Matplotlib

Matplotlib provides a wide range of customization options to create professional-looking plots. In this section, we will cover some of the most common customization options.

#### Adding Titles and Labels

To add a title to a plot in Matplotlib, we use the title function, which takes a string as argument: title. We can also add labels to the x and y axes using the xlabel and ylabel functions.

```python
import matplotlib.pyplot as plt

# Data
x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]

# Plot data
plt.plot(x, y)

# Add title and labels
plt.title('Line Plot')
plt.xlabel('X Axis')
plt.ylabel('Y Axis')

# Show plot
plt.show()
```

#### Changing Colors and Styles

Matplotlib allows you to customize the colors and styles of the plot elements, such as lines, markers, and bars.

**Colors** : By default, Matplotlib uses a default color cycle to distinguish between different lines in a plot. We can change the color of a line using the color parameter of the plotting function. For example:

```python
import matplotlib.pyplot as plt

# Data
x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]

# Plot data
plt.plot(x, y)

# Add title and labels
plt.title('Line Plot', color='red')
plt.xlabel('X Axis')
plt.ylabel('Y Axis')

# Show plot
plt.show()
```
This will plot the data using a red line. We can also use a variety of named colors such as blue, green, orange, purple, brown, and gray. We can also use color maps to map numerical values to colors. Matplotlib provides several built-in color maps, such as viridis, plasma, inferno, and magma. Here's an example:

```python
import matplotlib.pyplot as plt

# Data
x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]

# Get the weight of each point
z = np.random.rand(5)

# Plot using color map 
plt.scatter(x, y, c=z ,cmap='plasma')
plt.colorbar()
```

This will plot a scatter plot where the color of each point is determined by the corresponding value of z. The cmap parameter specifies the name of the color map to use, and the colorbar() function adds a color bar to the plot.

<br />

**Line Styles** : We can also change the style of lines in a plot using the linestyle parameter of the plotting function. Matplotlib provides several built-in line styles, such as solid lines ('-'), dashed lines ('--'), dotted lines (':'), and dash-dot lines ('--'). Here's an example:

```python
import matplotlib.pyplot as plt

# Data
x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]

# Plot data
plt.plot(x, y, linestyle='--')

# Add title and labels
plt.title('Line Plot', color='red')
plt.xlabel('X Axis')
plt.ylabel('Y Axis')

# Show plot
plt.show()
```

This will plot the data using a dashed line. We can also combine line styles with markers, as shown in the following example:

```python
import matplotlib.pyplot as plt

# Data
x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]

# Plot data
plt.plot(x, y, linestyle='--', marker='o', markersize=10)

# Add title and labels
plt.title('Line Plot', color='red')
plt.xlabel('X Axis')
plt.ylabel('Y Axis')

# Show plot
plt.show()
```

This will plot the data using a dashed line with circular markers. The markersize parameter controls the size of the markers.


#### Subplots

Subplots are useful when you want to display multiple plots in one figure. Matplotlib provides several ways to create subplots, and the simplest is the plt.subplots() function. Here is an example of creating a figure with two subplots, side by side:

```python
import matplotlib.pyplot as plt

# Data
x = [1, 2, 3, 4, 5]
y1 = [2, 4, 6, 8, 10]
y2 = [3, 5, 7, 9, 11]

# Plot data
fig, axs = plt.subplots(1, 2, figsize=(10, 5))

# first figure
axs[0].plot(x, y1)
# Add title to first figure
axs[0].set_title('Plot 1')

# second figure
axs[1].plot(x, y2)
# Add title to second figure
axs[1].set_title('Plot 2')

# Show plot
plt.show()
```

The first argument of plt.subplots() specifies the number of rows, and the second argument specifies the number of columns of subplots. In this example, we create one row and two columns of subplots. The figsize parameter specifies the size of the figure in inches.

<br />

The function returns two objects: the fig object that represents the whole figure and the axs object that represents the array of subplots. We can use indexing to access individual subplots. In this example, axs[0] represents the left subplot, and axs[1] represents the right subplot.

<br />

We can plot data on each subplot by calling plotting functions on the corresponding axs object. We can also set individual titles for each subplot using the set_title() method of each axs object.

### Saving Figures in Matplotlib

Matplotlib provides several ways to save figures to files, such as PNG, PDF, SVG, and EPS. The simplest way is to use the savefig() function:

```python

plt.savefig('figure.png')

```

This will save the current figure to a file named figure.png in the current directory. The file format is inferred from the file extension. 