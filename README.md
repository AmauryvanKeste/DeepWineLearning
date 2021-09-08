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

Link to [dataset](https://trello.com/b/cnnL0KJL/wine-tasting)

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


## 2. Binary classification

Here I use the keras_tuner library's RandomSearch to look for the ideal combination of number of layers, number of neurons and learning rate.
I also use different optimizers (Adam / SGD / RMSprop) and variations in number of epochs and splitting sizes.

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




# Usage

### Main folder
| Folder            | Description                                                 |
|-------------------|-------------------------------------------------------------|
| Old               | Directory containing irrelevant files                       |
| Datasets          | Directory containing the different datasets                 |
| Summaries         | Directory containing summaries of the models                |
| final_version | Directory containing all files for final version                |



# Author
| Name                   | Github                              |
|------------------------|-------------------------------------|  |
| Amaury van Kesteren    | https://github.com/AmauryvanKeste   |




# Timeline
07/09/2021 - 09/09/2021
