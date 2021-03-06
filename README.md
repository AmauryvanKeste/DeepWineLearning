# Deep learning challenge - Wine dataset
![wien](https://user-images.githubusercontent.com/84380197/132534908-93568322-7fff-4935-8f3d-d30fefa0cbb4.jpg)
# Description
This was a project during our time at BeCode.  
A dataset composed of white and red wines that included 12 features on chemical properties of wines. The target variable is the quality of wine on a scale from 1 to 10.

Link to wine [dataset](https://archive.ics.uci.edu/ml/datasets/wine).

# Goal
   1.  Use a deep learning library
   2. Prepare a data set for a machine learning model
   3. Put together a simple neural network
   4. Tune parameters of a neural network

# Project Management

Link to [trello](https://trello.com/b/cnnL0KJL/wine-tasting)

# Installation
## Python version
* Python 3.7.11

## Packages used
* keras==2.4.0
* tensorflow==2.3.0
* sklearn==0.24.2
* keras_tuner==1.0.3

# Implementation strategy

Looking at the data I observe that the quality ranking of the wine goes from 3 to 7, meaning 7 unique values, although the official documetation indicates a ranking from 1 to 10. 
So I first experiment with a classification into 10 classes (expecting no conclusive answer), then into 7 classes. The results are poor meaning that classification will require feature engineering and under-sampling.
I then perform classification according to a binary classification. This requires to regroup the wines into either "good" wines (quality >= 6)  or "bad" wines (quality <6). This results in slightly unbalanced datasets requiring stratification when further splitting the data in train sets and tests sets.
```X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.3, stratify=y, random_state = 10, shuffle=True)```

I add an additional explanatory variable: types of wines to further improve the models' explanatory power.

The learning rate is nearly always 0.001

## 1. Multi-class classification

| Model nr | Target  | Feat eng. | Class Num      | test_size | Normalise | optimizer | Num hidden  | input layer | output layer | activation | epochs | loss | acc train | acc test |
|----------|---------|-----------|----------------|-----------|-----------|-----------|-------------|-------------|--------------|------------|--------|------|-----------|----------|
| 1        | quality | No        | 10(multiclass) | 0.3       | yes       | adam      | 1           | dense 50    | dense 10     | softmax    | 100    | cct  | 0.5       | 0.5      |
| 2        | quality | No        | 7(multiclass)  | 0.3       | yes       | adam      | 1           | dense 50    | dense 10     | softmax    | 800    | cct  | 0.54      | 0.57     |

## 1.1 Multi-class with SMOTE

![smote_1](https://user-images.githubusercontent.com/84380197/132704762-b9c2f6ca-0e5e-4c43-8c63-c13494a9eb6b.png)

![smote2](https://user-images.githubusercontent.com/84380197/132704767-99b0a929-2b18-494a-8a8a-2e59594c63f7.png)


## 2. Binary classification

Here I use the keras_tuner library's RandomSearch to look for the ideal combination of number of layers, number of neurons and learning rate.
I also use different optimizers (Adam / SGD / RMSprop) and variations in number of epochs and splitting sizes.
In order to avoid overfitting and to avoid losing time I add an EarlyStopping setting the monitor val_score, direction max.

| Model nr | acc train | acc test | Target       | Feat eng     | Num classes | test_size    | Norm | Opt     | Num hidden | input layer | output layer | activation | epochs | loss     |
|----------|-----------|----------|--------------|--------------|-------------|--------------|------|---------|------------|-------------|--------------|------------|--------|----------|
| 3        |           | 0.7      | quality      | No           | 2(binary)   | 0.2          | yes  | adam    | 15         | dense       | dense 1      | sigmoid    | 15     | binary c |
| 4        | 0.79      |          | quality      | No           | 2(binary)   | No splitting | no   | adam    | 3          | dense       | dense 1      | sigmoid    | 5000   | binary c |
| 5        |           | 0.8      | quality      | No           | 2(binary)   | 0.2          | yes  | adam    | 17         | dense       | dense 1      | sigmoid    | 100    | binary c |
| 6        |           | 0.7      | quality      | No           | 2(binary)   | 0.2          | yes  | SGD     | 11         | dense       | dense 1      | sigmoid    | 100    | binary c |
| 7        |           | 0.7      | quality      | No           | 2(binary)   | 0.2          | yes  | RMSprop | 19         | dense       | dense 1      | sigmoid    | 100    | binary c |
| 8        |           | 0.7      | quality      | Added types  | 2(binary)   | 0.2          | yes  | adam    |            |             | dense 1      | sigmoid    | 100    | binary c |
| 9        |           | 0.797    | quality      | Added types  | 2(binary)   | No splitting | no   | adam    | 3          | dense       | dense 1      | sigmoid    | 5000   | binary c |
| 11       |           | 0.982    | type of wine | q is feature | 2(binary)   | 0.33         | no   | adam    | 3          | dense       | dense 1      | sigmoid    | 50     | binary c |

## 3. Regression
### 3.1 Using Kerasregressor

| Model nr     | y       | Feat eng    | Num clas   | split   | Norm | opt  | Num hidden | layers1  | activation | output layer | act  | epochs | loss               | Kfold mean score | Kfold |
|--------------|---------|-------------|------------|---------|------|------|------------|----------|------------|--------------|------|--------|--------------------|------------------|-------|
| 13(standard) | quality | Added types | regression | 10Kfold | no   | adam | 1          | dense 12 | relu       | dense 1      | none | 100    | mean_squared_error | 0.54             | Kfold |
| 14(larger)   | quality | Added types | regression | 10Kfold | no   | adam | 2          | dense 12 | relu       | dense 1      | none | 100    | mean_squared_error | 0.53             | Kfold |
| 15(wider)    | quality | Added types | regression | 10Kfold | no   | adam | 1          | dense 20 | relu       | dense 1      | none | 100    | mean_squared_error | 0.52             | Kfold |

### 3.2 Custom

| Model nr | y       | Feat eng    | Num clas   | split        | Norm | opt  | Num hidden | layers1   | activation | output layer | act    | epochs | loss                | Accuracy test | Loss |
|----------|---------|-------------|------------|--------------|------|------|------------|-----------|------------|--------------|--------|--------|---------------------|---------------|------|
| 16       | quality | Added types | regression | No splitting | yes  | adam | 3          | Dense 256 | relu       | dense 1      | linear | 500    | mean_absolute_error | 0.6           | 0.56 |

![cm_1](https://user-images.githubusercontent.com/84380197/132695143-aaadf7b2-2597-452e-a27b-ea4d1f558d46.png)
![linear_1](https://user-images.githubusercontent.com/84380197/132695145-be325cba-6f5b-48da-88fe-6d65d474f994.png)
Here above examples of overfitting.

![linear_2](https://user-images.githubusercontent.com/84380197/132697340-8d046d75-d68a-4872-8b95-48759f217df2.png)
![cm_2](https://user-images.githubusercontent.com/84380197/132697342-226af2a5-341e-486a-ba0f-9445c8bb23cb.png)
Here we can see that both the training and testing lines are not too separated indicating no issues of overfitting.


# Usage

### Main folder
| Folder            | Description                                                 |
|-------------------|-------------------------------------------------------------|
| Old               | Directory containing irrelevant files                       |
| Datasets          | Directory containing the different datasets                 |
| Summaries         | Directory containing summaries of the models                |
| Final_version     | Directory containing all files for final version            | 
| Graphs            | Directory containing all graphs                             |


# Author
| Name                   | Github                              |
|------------------------|-------------------------------------|
| Amaury van Kesteren    | https://github.com/AmauryvanKeste   |




# Timeline
07/09/2021 - 09/09/2021
