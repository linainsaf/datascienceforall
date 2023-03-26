## What can we do with it?

Machine learning can be used for different tasks depending on the type of data you have and the problem you are trying to solve. Here is an overview of some of the most common tasks:

- **Regression** : regression involves predicting a numerical value based on one or more parameters. For example, you can use regression to predict the price of a house based on its location, size, and features.

- **Classification** : classification involves predicting a discrete label based on a set of features. For example, you can use classification to predict whether an email is spam or not based on its content and features.

- **Clustering** : clustering involves grouping similar data points into clusters without predefined class labels. For example, you can use clustering to group customers in your store based on their shopping habits.

- **Dimensionality Reduction** : dimensionality reduction involves reducing the complexity of data by reducing the number of variables or features. For example, you can use dimensionality reduction to visualize high-dimensional data in two or three dimensions.

## Types of ML

There are two main types:

### Supervised Learning

n this type of learning, the model learns to predict an output from an input using a training data set that contains examples of known inputs and outputs. The model then adjusts its parameters to minimize prediction error.

<br />
Here are some supervised machine learning models:

#### Linear Regression

Linear regression is a prediction method that models the relationship between a continuous target variable and one or more continuous or categorical predictor variables. The goal of linear regression is to find the best line (or hyperplane in higher dimensions) that represents the relationship between the variables.

<br />

The model can be used for prediction or to understand the relationship between variables. For example, in the case of predicting the selling price of a house, the predictor variables could be square footage, number of bedrooms, location, etc.

<br />

Example code:

```python
from sklearn.linear_model import LinearRegression

# Create a linear regression model
regression_model = LinearRegression()

# Train the model on the training data
regression_model.fit(X_train, y_train)

# Predict values for the test data
y_pred = regression_model.predict(X_test)
```

#### Logistic Regression

Logistic regression is a classification method that predicts the probability that an observation belongs to a particular class (binary or multi-class). The output is a probability, so the predicted value is always between 0 and 1.

<br />
The model estimates the probability of belonging to each class based on the predictor variables. The model is often used for the classification of potential customers, emails as spam or not, etc.

<br />
Example code:

```python
from sklearn.linear_model import LogisticRegression

# Create a logistic regression model
logistic_model = LogisticRegression()

# Train the model on the training data
logistic_model.fit(X_train, y_train)

# Predict classes for the test data
y_pred = logistic_model.predict(X_test)
```
#### Decision Tree


Decision trees are a classification and regression method that allows you to create a model from data in the form of a tree. Internal nodes of the tree represent a condition on the predictor variables, the leaves of the tree represent the predictions of the target variable. The model is easily interpretable and allows you to understand how variables are used for prediction.

<br />

Example code:

```python
from sklearn.tree import DecisionTreeClassifier

# Create a decision tree model
tree_model = DecisionTreeClassifier()

# Train the model on the training data
tree_model.fit(X_train, y_train)

# Predict classes for the test data
y_pred = tree_model.predict(X_test)
```

#### Random Forest

Random Forests are a classification and regression method that combines the results of several decision trees. Each tree in the forest is trained on a random sample of the training data. The model aggregates the predictions of the different trees to give a final prediction. Random Forests are often more accurate than individual decision trees.

<br />

Here's an example code using Scikit-learn to train a random forest model for classification:

```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.datasets import make_classification

X, y = make_classification(n_features=4, random_state=0)
clf = RandomForestClassifier(max_depth=2, random_state=0)
clf.fit(X, y)
```

#### Support Vector Machines (SVMs)


SVMs are a supervised learning algorithm used for classification and regression. SVMs find a decision boundary that maximizes the margin between the two classes. This decision boundary is called a hyperplane. SVMs can be used for classification tasks with two or more classes.

<br />

SVMs have high accuracy and are popular in classification tasks. They can also be used for regression.

<br />

Here's an example code using Scikit-learn to train an SVM model for classification:

```py
from sklearn import svm
from sklearn.datasets import make_classification

X, y = make_classification(n_features=4, random_state=0)
clf = svm.SVC(kernel='linear', C=1, random_state=0)
clf.fit(X, y)
```

### Unsupervised Learning

Unsupervised models are machine learning algorithms that do not require labeled data to function. They are used to explore data and find hidden structures, patterns, and correlations. Here are some examples of unsupervised models:


#### K-means clustering

K-means clustering is a data partitioning technique that divides a dataset into K distinct groups (clusters). Each data point is assigned to a cluster based on its proximity to the center of that cluster.

<br />
Here's an example Python code to apply K-means clustering:

```py
from sklearn.cluster import KMeans
import numpy as np

# Generate random data
X = np.random.rand(100, 2)

# Instantiate the clustering model
kmeans = KMeans(n_clusters=3)

# Fit the model to the data
kmeans.fit(X)

# Predict the clusters for new data
labels = kmeans.predict(X)
```

#### Principal Component Analysis (PCA)


Principal Component Analysis is a dimension reduction method that projects high-dimensional data onto a lower-dimensional space while retaining as much of the information in the original data as possible.

<br />

Here's an example Python code to apply PCA:


```py
from sklearn.decomposition import PCA
import numpy as np

# Generate random data
X = np.random.rand(100, 10)

# Instantiate the PCA model
pca = PCA(n_components=3)

# Fit the model to the data
pca.fit(X)

# Transform the data to get the new dimensions
X_pca = pca.transform(X)
```

#### Dimensionality Reduction with t-SNE

t-SNE is a dimensionality reduction method that projects high-dimensional data onto a lower-dimensional space while retaining as much of the proximity relationships between the data points as possible.

<br />

Here's an example Python code to apply dimensionality reduction with t-SNE:


```py
from sklearn.manifold import TSNE
import numpy as np

# Generate random data
X = np.random.rand(100, 10)

# Instantiate the t-SNE model
tsne = TSNE(n_components=2)

# Fit the model to the data
X_tsne = tsne.fit_transform(X)
```