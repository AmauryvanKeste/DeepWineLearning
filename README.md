# DeepWineLearning

# Deep learning challenge - Wine dataset
![wien](https://user-images.githubusercontent.com/84380197/132534908-93568322-7fff-4935-8f3d-d30fefa0cbb4.jpg)
# Description
This was a project during our time at BeCode.  
A dataset composed of white and red wines that included 12 features on chemical properties of wines. The target variable is the quality of wine on a scale from 1 to 10.
[Link to wine dataset](https://archive.ics.uci.edu/ml/datasets/wine)
# Goal
   1.  Use a deep learning library
   2. Prepare a data set for a machine learning model
   3. Put together a simple neural network
   4. Tune parameters of a neural network
# Installation
### Python version
* Python 3.7.11

### Packages used
* keras==2.4.0
* tensorflow==2.3.0
* sklearn==0.24.2
* keras_tuner==1.0.3

### Implementation strategy

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
| 2        | quality | No        | 10(multiclass) | 0.3       | yes       | adam      | 1           | dense 50    | dense 10     | softmax    | 800    | cct  | 0.54      | 0.57     |


## 2. Binary classification

Here I use the keras_tuner library's RandomSearch to look for the ideal combination of number of layers, number of neurons and learning rate.
I also use different optimizers (Adam / SGD / RMSprop) and variations in number of epochs and splitting sizes.



## 3. Regression




# Usage
### Main folder
| Folder            | Description                                                 |
|-------------------|-------------------------------------------------------------|
| data_json         | Directory containing the original .json file on steam data. |
| old_versions      | Directory containing all files for early versions.<br>* version 1<br>* version 2<br>* version 3 |
| images            | Directory containing images used for readme.md              |
| final_version | Directory containing all files for final version            |




### final_version
| File                  | Description                                                |
|-----------------------|------------------------------------------------------------|
| data_files            | Directory containing all our .csv files.                   |
| database              | Database created using info from the .csv files.           |
| utils                 | Directory containing Python files:                         |
| ** create_database.py | Code used to create the database                           |
| ** json_to_df.py      | Code used to convert the .json file to a Pandas dataframe. |
| app.py                | Code used for deployment on a website                      |

# Sample of our app
![](images/steam_app_gif.gif)

# ERD of our SQLITE-database 
![](images/ERD.png)

# Author
| Name                   | Github                              |
|------------------------|-------------------------------------|  |
| Amaury van Kesteren    | https://github.com/AmauryvanKeste   |




# Timeline
07/09/2021 - 09/09/2021
