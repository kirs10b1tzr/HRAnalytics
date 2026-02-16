# Data Preparation  
**Tools: Python and Jupyter Notebook.**  
*Trying jupyter notebook for the first time! Enjoying it so far :)*

### 1. Import Packages  
*These are toolboxes for coding that help make our work easier i.e. code shorter/ more elegant.*  
Import them all at the top of your code so people reading later won't have to look for them throughout the script

```python
import numpy as np # nicknames help shorten the code
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```
#### TROUBLESHOOT!  
**If you encounter an error, make sure you have the packages installed first. Otherwise, ignore this cell.**  
Open your terminal/powershell on your laptop/pc and run any of the below lines depending on which package you're missing.  
Depending on your version of python, you might need to run 'python' instead of 'python3' above  
then run the previous cell's code again to import successfully (no message should appear).  

```python
python3 -m pip install numpy
python3 -m pip install pandas
python3 -m pip install matplotlib
python3 -m pip install seaborn
```
### 2. Import the dataset  
*Make sure you can know the filepath (exact location) of the folder where you saved your csv file.*  
You can find the filepath by right clicking the file and clicking 'properties'(on Windows) or 'get info'(on Mac)  

```python
hr = pd.read_csv(r"/Users/username/Desktop/Folder1/Folder2/HR-Employee-Attrition.csv")
```

'r' above just means reverse all the backslashes  
'hr' is the name of the dataframe in our python environment  
'pd.read_csv' opens our pandas toolbox and instructs python to read/import the csv file  
Pay attention to the notation "" ()  

### 3. Form First Impressions  
*You can do this very quickly in PowerQuery on PowerBI if you'd like* 

**BE CAREFUL!**  
This is not enough to tell us what columns are relevant or if there is data missing (we'll do this in the following steps)  
  
```python
print(hr.info())
```
This tells us the headings/columns (they successfully imported!), rows and datatypes among other info.  
*This is a preview of the output*
| #  | Column                   | Non-Null Count | Dtype |
|----|--------------------------|----------------|-------|
| 0  | Age                      | 1470 non-null  | int64 |
| 1  | Attrition                | 1470 non-null  | str   |
| 2  | BusinessTravel           | 1470 non-null  | str   |
| 3  | DailyRate                | 1470 non-null  | int64 |
| 4  | Department               | 1470 non-null  | str   |
| 5  | DistanceFromHome         | 1470 non-null  | int64 |
| 30 | WorkLifeBalance          | 1470 non-null  | int64 |
| 31 | YearsAtCompany           | 1470 non-null  | int64 |
| 32 | YearsInCurrentRole       | 1470 non-null  | int64 |
| 33 | YearsSinceLastPromotion  | 1470 non-null  | int64 |
| 34 | YearsWithCurrManager     | 1470 non-null  | int64 |

Check that none of our rows are duplicated exactly i.e. none of our records have copies  
```python
print(hr.duplicated().sum()) 
```
Output: 0.
#### 3.1. Numeric Variables
```python
print(hr.describe()) # this only shows the descriptive statistics for numeric variables!
```
*This is a preview of the output*
|       | Age         | DailyRate   | DistanceFromHome | Education   | EmployeeCount | EmployeeNumber | EnvironmentSatisfaction | HourlyRate  | JobInvolvement | JobLevel    | RelationshipSatisfaction | StandardHours |
|-------|-------------|-------------|------------------|-------------|---------------|----------------|-------------------------|-------------|----------------|-------------|--------------------------|---------------|
| count | 1470.000000 | 1470.000000 | 1470.000000      | 1470.000000 | 1470.0        | 1470.000000    | 1470.000000             | 1470.000000 | 1470.000000    | 1470.000000 | 1470.000000              | 1470.0        |
| mean  | 36.923810   | 802.485714  | 9.192517         | 2.912925    | 1.0           | 1024.865306    | 2.721769                | 65.891156   | 2.729932       | 2.063946    | 2.712245                 | 80.0          |
| std   | 9.135373    | 403.509100  | 8.106864         | 1.024165    | 0.0           | 602.024335     | 1.093082                | 20.329428   | 0.711561       | 1.106940    | 1.081209                 | 0.0           |
| min   | 18.000000   | 102.000000  | 1.000000         | 1.000000    | 1.0           | 1.000000       | 1.000000                | 30.000000   | 1.000000       | 1.000000    | 1.000000                 | 80.0          |
| 25%   | 30.000000   | 465.000000  | 2.000000         | 2.000000    | 1.0           | 491.250000     | 2.000000                | 48.000000   | 2.000000       | 1.000000    | 2.000000                 | 80.0          |
| 50%   | 36.000000   | 802.000000  | 7.000000         | 3.000000    | 1.0           | 1020.500000    | 3.000000                | 66.000000   | 3.000000       | 2.000000    | 3.000000                 | 80.0          |
| 75%   | 43.000000   | 1157.000000 | 14.000000        | 4.000000    | 1.0           | 1555.750000    | 4.000000                | 83.750000   | 3.000000       | 3.000000    | 4.000000                 | 80.0          |
| max   | 60.000000   | 1499.000000 | 29.000000        | 5.000000    | 1.0           | 2068.000000    | 4.000000                | 100.000000  | 4.000000       | 5.000000    | 4.000000                 | 80.0          |

Some things have a scale of 1-4 in when you look at range, likely representing Likert Scales in a recent survey.  

Based off the mean, standard deviation and range... the columns 'EmployeeCount' and 'StandardHours' are the exact same for all employees.  
Let's delete them using .drop because they don't give us any information.  
```python
hr1 = hr.drop(columns=['EmployeeCount','StandardHours'])

# TIP: give a new name to dataframe so we can backtrack if we make a mistake (good habit!)
```

We also could rename 'Education' to reflect highest educational level attained instead of numbers  
but the Kaggle datacard explains its been masked, possibly for anonymity or to prevent discrimination.  
  
For my convenience, I will rename the column 'DistanceFromHome' to 'KmHome2Work' for clarity (this is explained in the Kaggle notebook)  

```python
hr2 = hr1.rename(columns={'DistanceFromHome':'KmHome2Work'})
```

#### 3.2. Non-numeric Variables
Let's check the amount of categories/levels in our non-numeric variables/columns.  
This will also help us:  
(i) understand what the column headers mean if for whatever reason we can't just ask  
(ii) identify any hidden null values (NaN, Null, None)  

```python

str_cols = hr2.select_dtypes(include=["str"]).columns # we are picking all the non-numeric columns ('str'/string is their datatype)
for col in str_cols: # this loop means that for each column that we picked out
    print(f"Frequency of {col}:", hr2[col].value_counts(), "\n") # print the categories and how many records fall under them
# Look into loops and string concatenation for more breakdown of the above lines.
```
The column 'Over18' is not useful because all our employees are over 18, let's drop them  
```python
hr3 = hr2.drop(columns=['Over18'])
```
While there is no missingness, this is VERY RARE in real-life scenarios.  
I recommend looking into missiness (MCAR, MNAR, MAR) but I will not cover this.  
I am assuming that listwise deletion was performed (rows with ANY columns missing were deleted).  
The consequence of this is weaker statistical power-- something to note in predictive modelling later.  

We could also rename any categories at this stage but this dataset is pretty clear.  
This is veering into EDA but let's just doublecheck that JobRole matches for each Department  
```python
deps = pd.crosstab(hr['JobRole'],hr['Department']) 
print(deps) 
```
*Full output below*
| JobRole                   | Human Resources | Research & Development | Sales |
|---------------------------|-----------------|------------------------|-------|
| Healthcare Representative | 0               | 131                    | 0     |
| Human Resources           | 52              | 0                      | 0     |
| Laboratory Technician     | 0               | 259                    | 0     |
| Manager                   | 11              | 54                     | 37    |
| Manufacturing Director    | 0               | 145                    | 0     |
| Research Director         | 0               | 80                     | 0     |
| Research Scientist        | 0               | 292                    | 0     |
| Sales Executive           | 0               | 0                      | 326   |
| Sales Representative      | 0               | 0                      | 83    |

Everything looks sound here, we won't rename anything.  

#### 4. Optional: Export
*If you want to upload onto PowerBI*
```python
export = hr3.to_csv(r"Users/username/Desktop/Folder1/Folder2/HRAnalytics.csv")
```

## Let's move to the analytics!
