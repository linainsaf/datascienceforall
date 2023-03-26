# How to Evaluate a Model

## Evaluating a Regression Model

### Regression Coefficients

The equation of a simple linear regression can be written as:

```py
y = β0 + β1x + ε
```

where:

- y is the dependent variable,
- x is the independent variable,
- β0 is the intercept or constant,
- β1 is the regression coefficient or slope,
- ε is the residual error.

The equation indicates that the value of y depends on the value of x multiplied by the regression coefficient β1 and added to the intercept β0, with some residual error ε. Multiple linear regression can be extended to include several independent variables, which would give an equation of the form:

```py
y = β0 + β1x1 + β2x2 + ... + βpxp + ε
```
where p is the number of independent variables.

<br />

Regression coefficients are used to measure the relationship between independent variables and the dependent variable in a regression model. A positive coefficient indicates a positive relationship between the variables, while a negative coefficient indicates an inverse relationship. In simple linear regression, the slope coefficient measures the variation of the dependent variable for one unit of variation of the independent variable, while the intercept coefficient indicates the value of the dependent variable when the independent variable is equal to zero. However, the interpretation of coefficients depends on the type of model and the nature of the data.

### Mean Absolute Error (MAE)

MAE measures the average absolute error between predictions and actual observations. It is calculated by taking the sum of the absolute values of the differences between each prediction and the actual value, then dividing this sum by the total number of predictions. It is often used for simple regression models. The lower the MAE, the better the model's performance.

<br />

The equation for MAE is:

```py
MAE = (1/n) * Σ|i=1 à n|(|y_i - ŷ_i|)

```
Where:

- n: the number of observations in the test dataset
- y_i: the actual value of the target variable for the i-th observation
- ŷ_i: the predicted value of the target variable for the i-th observation

Python code example to calculate MAE:

```py
from sklearn.metrics import mean_absolute_error

y_true = [3, -0.5, 2, 7]
y_pred = [2.5, 0.0, 2, 8]

mae = mean_absolute_error(y_true, y_pred)

print("MAE: ", mae)
```
### Mean Squared Error (MSE)

MSE measures the average of the squares of the errors between predictions and actual observations. It is calculated by taking the sum of the squares of the differences between each prediction and the actual value, then dividing this sum by the total number of predictions. It is often used for more complex regression models. It is more sensitive to large errors than MAE. The lower the MSE, the better the model's performance.

<br />

The equation for MSE is:

```py
MSE = (1/n) * Σ|i=1 à n|(y_i - ŷ_i)^2
```
Where:

- n: the number of observations in the test dataset
- y_i: the actual value of the target variable for the i-th observation
- ŷ_i: the predicted value of the target variable for the i-th observation

Python code example to calculate MSE:

```py
from sklearn.metrics import mean_squared_error

y_true = [3, -0.5, 2, 7]
y_pred = [2.5, 0.0, 2, 8]

mse = mean_squared_error(y_true, y_pred)

print("MSE: ", mse)

```

### Root Mean Squared Error (RMSE)

RMSE is the square root of MSE and represents the average error between the model's predictions and the actual values of the target variable. It is often used for more complex regression models. The lower the RMSE, the better the model's performance.

<br />

The equation for RMSE is:

```py
RMSE = sqrt(MSE)
```
Python code example for calculating RMSE:

```py
from sklearn.metrics import mean_squared_error
import numpy as np

y_true = [3, -0.5, 2, 7]
y_pred = [2.5, 0.0, 2, 8]

mse = mean_squared_error(y_true, y_pred)
rmse = np.sqrt(mse)

print("RMSE: ", rmse)
```

### Coefficient of Determination R²

The coefficient of determination R² measures the proportion of variance explained by the model. It ranges from 0 to 1, where 0 means the model fails to explain the variance of the target variable and 1 means it explains all the variance. The higher the R², the better the model's performance. The equation is:

```py
R² = 1 - SSE / SST
```
where SSE is the sum of squared residuals (i.e., the sum of squared differences between predicted and actual values), and SST is the sum of squared differences between actual values and the mean of actual values.

- Sum of Squared Errors (SSE):

```py
SSE = Σ(yi - ŷi)²
```
where yi is the actual value of the dependent variable, and ŷi is the value predicted by the model.

- Total Sum of Squares (SST) :

```py
SST = Σ(yi - ȳ)²
```
where yi is the actual value of the dependent variable, and ȳ is the mean of the actual values.


```py
from sklearn.metrics import r2_score

y_true = [3, -0.5, 2, 7]
y_pred = [2.5, 0.0, 2, 8]

# Calculation of the R² coefficient of determination
r2 = r2_score(y_train, y_pred)

print("R² coefficient of determination:", r2)
```


### Interpretation of results

- MAE: An MAE of 0 means that the model predicts the values of the target variable perfectly. In general, a low MAE indicates good performance, but this depends on the context and the range of values of the target variable.

- MSE and RMSE: As mentioned earlier, these two metrics are more sensitive to large errors than MAE. Therefore, a high MSE or RMSE may indicate outliers in the data or that the model is not accurately predicting extreme values of the target variable.

- R²: indicates the proportion of the total variance in the data that is explained by the model. An R² of 1 means the model perfectly explains the variance in the data, while an R² of 0 means the model explains none of the variance in the data. In practice, a high R² (typically greater than 0.7 or 0.8) is considered a good fit of the model to the data.

- SSE indicates the amount of residual error in the model. In general, lower SSE is better, as it means the model's predictions are closer to the actual values.

- SST indicates the amount of total variation in the data. It is often used as a measure of the variability of the reference data. A model that explains a large portion of the total variation in SST is considered a good fit to the data.




## Evaluating a Classification Model

### Accuracy

Accuracy is the simplest metric to evaluate classification models. It measures the proportion of correct predictions on the test dataset.

<br />

The equation for accuracy is:

```py
Accuracy = (TP + TN) / (TP + TN + FP + FN)
```
Where:

- TP: True Positives, the number of correct positive predictions
- TN: True Negatives, the number of correct negative predictions
- FP: False Positives, the number of incorrect positive predictions
- FN: False Negatives, the number of incorrect negative predictions

Python code example to calculate accuracy:


```py
from sklearn.metrics import accuracy_score

y_true = [0, 1, 2, 0, 1, 2]
y_pred = [0, 2, 1, 0, 0, 1]

accuracy = accuracy_score(y_true, y_pred)

print("Accuracy: ", accuracy) #Accuracy: 0.3333333333333333
```

### Precision, Recall et F1-score

Precision, recall, and F1-score are more advanced metrics for evaluating classification models. They are particularly useful for binary classification problems, where we want to predict the presence or absence of a class.

- **Precision** measures the proportion of positive predictions that are correct. It is calculated by dividing the number of true positives by the sum of true positives and false positives. The precision equation is as follows:
```py
Precision = TP / (TP + FP)
```
- **Recall** measures the proportion of true positives that are correctly identified. It is calculated by dividing the number of true positives by the sum of true positives and false negatives. The recall equation is as follows:

```py
Recall = TP / (TP + FN)
```
- **F1-score** is a harmonic mean of precision and recall. It is calculated by taking the weighted average of precision and recall, where the weighting is given by the harmonic mean of precision and recall. The F1-score equation is as follows:

```py
F1 = 2 * (Precision * Recall) / (Precision + Recall)
```
Example Python code for calculating precision, recall, and F1-score:

```py
from sklearn.metrics import precision_score, recall_score, f1_score

y_true = [0, 1, 2, 0, 1, 2]
y_pred = [0, 2, 1, 0, 0, 1]

precision = precision_score(y_true, y_pred, average='macro')
recall = recall_score(y_true, y_pred, average='macro')
f1 = f1_score(y_true, y_pred, average='macro')

print("Precision: ", precision)
print("Recall: ", recall)
print("F1-score: ", f1)

```
### Confusion Matrix

The confusion matrix is a visual tool for evaluating the performance of a classification model. It shows the number of correct and incorrect predictions for each class.

<br />

Example Python code for displaying the confusion matrix:

```py
from sklearn.metrics import confusion_matrix
import matplotlib.pyplot as plt
import seaborn as sns

y_true = [0, 1, 2, 0, 1, 2]
y_pred = [0, 2, 1, 0, 0, 1]

cm = confusion_matrix(y_true, y_pred)

sns.heatmap(cm, annot=True, cmap="Blues")
plt.xlabel('Predictions')
plt.ylabel('True values')
plt.show()

```

### Interpreting classification evaluation metrics:

It all depends on the specific context of each machine learning project. However, here are some general points to consider when interpreting these metrics:

- **Precision** : Precision measures the rate of correct positive predictions among all positive predictions. For example, a precision of 0.8 means that the model correctly predicted 80% of positive results. However, if precision is high but recall is low, this may indicate that the model is predicting too few positive results, thereby missing real positive results.

- **Recall**: Recall measures the rate of correct positive predictions among all actual positive results. For example, a recall of 0.7 means that the model correctly predicted 70% of positive results. However, if recall is high but precision is low, this may indicate that the model is predicting too many positive results, including false positives.

- **F1 score** : The F1 score is a harmonic mean of precision and recall. It is usually used when trying to find a balance between precision and recall.

- **Confusion matrix**: The confusion matrix shows the number of correct and incorrect predictions for each class. It allows you to see where the model is making mistakes. For example, if the model often predicts class 0 when it is actually class 1, there may be confusion between these two classes.

In general, it is important to evaluate the model with multiple metrics to get an overview of its performance. Depending on the context, certain metrics may be more important than others. For example, in fraud detection problems, high recall may be more important than precision because it is crucial not to miss any fraud cases, even if it means having more false positives.
