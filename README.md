

```python
import pandas as pd
```


```python
df = pd.read_csv("./heroes_information.csv")
```


```python
import swagger_client
```

# Project - Data Cleaning

## Introduction
In this lab, we'll make use of everything we've learned about pandas, data cleaning, and Exploratory Data Analysis. In order to complete this lab, you'll have to make import, clean, combine, reshape, and visualize data to answer questions provided, as well as your own questions!

## Objectives
You will be able to:
* Show mastery of the content covered in this section


## Objectives (SG):
YWBAT
* clean pandas dataframe
    * handle null values
    * handle place marker values
    * handle categorical data (data conditioning)
    * feature engineering (data conditioning)
    * normalization (data conditioning)
* In lieu of building Linear Regression Models

## The Dataset
In this lab, we'll work with the comprehensive [Super Heroes Dataset](https://www.kaggle.com/claudiodavi/superhero-set/data), which can be found on Kaggle!

## Goals
* Use all available pandas knowledge to clean the dataset and deal with null values
* Use Queries and aggregations to group the data into interesting subsets as needed
* Use descriptive statistics and data visualization to find answers to questions we may have about the data. 

## Getting Started

In the cell below:

* Import and alias pandas as `pd`
* Import and alias numpy as `np`
* Import and alias seaborn as `sns`
* Import and alias matplotlib.pyplot as `plt`
* Set matplotlib visualizations to display inline in the notebook


```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline
```

For this lab, our dataset is split among two different sources--`heroes_information.csv` and `super_hero_powers.csv`.

Use pandas to read in each file and store them in DataFrames in the appropriate variables below. Then, display the head of each to ensure that everything loaded correctly.  


```python
heroes_df = pd.read_csv("heroes_information.csv")
powers_df = pd.read_csv("super_hero_powers.csv")
```

It looks as if the heroes information dataset contained an index column.  We did not specify that this dataset contained an index column, because we hadn't seen it yet. Pandas does not know how to tell apart an index column from any other data, so it stored it with the column name `Unnamed: 0`.  

Our DataFrame provided row indices by default, so this column is not needed.  Drop it from the DataFrame in place in the cell below, and then display the head of `heroes_df` to ensure that it worked properly. 


```python
heroes_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>name</th>
      <th>Gender</th>
      <th>Eye color</th>
      <th>Race</th>
      <th>Hair color</th>
      <th>Height</th>
      <th>Publisher</th>
      <th>Skin color</th>
      <th>Alignment</th>
      <th>Weight</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>A-Bomb</td>
      <td>Male</td>
      <td>yellow</td>
      <td>Human</td>
      <td>No Hair</td>
      <td>203.0</td>
      <td>Marvel Comics</td>
      <td>-</td>
      <td>good</td>
      <td>441.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Abe Sapien</td>
      <td>Male</td>
      <td>blue</td>
      <td>Icthyo Sapien</td>
      <td>No Hair</td>
      <td>191.0</td>
      <td>Dark Horse Comics</td>
      <td>blue</td>
      <td>good</td>
      <td>65.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Abin Sur</td>
      <td>Male</td>
      <td>blue</td>
      <td>Ungaran</td>
      <td>No Hair</td>
      <td>185.0</td>
      <td>DC Comics</td>
      <td>red</td>
      <td>good</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Abomination</td>
      <td>Male</td>
      <td>green</td>
      <td>Human / Radiation</td>
      <td>No Hair</td>
      <td>203.0</td>
      <td>Marvel Comics</td>
      <td>-</td>
      <td>bad</td>
      <td>441.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Abraxas</td>
      <td>Male</td>
      <td>blue</td>
      <td>Cosmic Entity</td>
      <td>Black</td>
      <td>-99.0</td>
      <td>Marvel Comics</td>
      <td>-</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
powers_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>hero_names</th>
      <th>Agility</th>
      <th>Accelerated Healing</th>
      <th>Lantern Power Ring</th>
      <th>Dimensional Awareness</th>
      <th>Cold Resistance</th>
      <th>Durability</th>
      <th>Stealth</th>
      <th>Energy Absorption</th>
      <th>Flight</th>
      <th>...</th>
      <th>Web Creation</th>
      <th>Reality Warping</th>
      <th>Odin Force</th>
      <th>Symbiote Costume</th>
      <th>Speed Force</th>
      <th>Phoenix Force</th>
      <th>Molecular Dissipation</th>
      <th>Vision - Cryo</th>
      <th>Omnipresent</th>
      <th>Omniscient</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3-D Man</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>A-Bomb</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Abe Sapien</td>
      <td>True</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Abin Sur</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Abomination</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>...</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 168 columns</p>
</div>



## Familiarize Yourself With the Dataset

The first step in our Exploratory Data Analysis will be to get familiar with the data.  This step includes:

* Understanding the dimensionality of your dataset
* Investigating what type of data it contains, and the data types used to store it
* Discovering how missing values are encoded, and how many there are
* Getting a feel for what information it does and doesnt contain

In the cell below, get the descriptive statistics of each DataFrame.  


```python
heroes_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>name</th>
      <th>Gender</th>
      <th>Eye color</th>
      <th>Race</th>
      <th>Hair color</th>
      <th>Height</th>
      <th>Publisher</th>
      <th>Skin color</th>
      <th>Alignment</th>
      <th>Weight</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>A-Bomb</td>
      <td>Male</td>
      <td>yellow</td>
      <td>Human</td>
      <td>No Hair</td>
      <td>203.0</td>
      <td>Marvel Comics</td>
      <td>-</td>
      <td>good</td>
      <td>441.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Abe Sapien</td>
      <td>Male</td>
      <td>blue</td>
      <td>Icthyo Sapien</td>
      <td>No Hair</td>
      <td>191.0</td>
      <td>Dark Horse Comics</td>
      <td>blue</td>
      <td>good</td>
      <td>65.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Abin Sur</td>
      <td>Male</td>
      <td>blue</td>
      <td>Ungaran</td>
      <td>No Hair</td>
      <td>185.0</td>
      <td>DC Comics</td>
      <td>red</td>
      <td>good</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Abomination</td>
      <td>Male</td>
      <td>green</td>
      <td>Human / Radiation</td>
      <td>No Hair</td>
      <td>203.0</td>
      <td>Marvel Comics</td>
      <td>-</td>
      <td>bad</td>
      <td>441.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Abraxas</td>
      <td>Male</td>
      <td>blue</td>
      <td>Cosmic Entity</td>
      <td>Black</td>
      <td>-99.0</td>
      <td>Marvel Comics</td>
      <td>-</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
heroes_df.columns
```




    Index(['Unnamed: 0', 'name', 'Gender', 'Eye color', 'Race', 'Hair color',
           'Height', 'Publisher', 'Skin color', 'Alignment', 'Weight'],
          dtype='object')




```python
heroes_df.drop(labels=["Unnamed: 0"], axis=1, inplace=True)
heroes_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>Gender</th>
      <th>Eye color</th>
      <th>Race</th>
      <th>Hair color</th>
      <th>Height</th>
      <th>Publisher</th>
      <th>Skin color</th>
      <th>Alignment</th>
      <th>Weight</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A-Bomb</td>
      <td>Male</td>
      <td>yellow</td>
      <td>Human</td>
      <td>No Hair</td>
      <td>203.0</td>
      <td>Marvel Comics</td>
      <td>-</td>
      <td>good</td>
      <td>441.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Abe Sapien</td>
      <td>Male</td>
      <td>blue</td>
      <td>Icthyo Sapien</td>
      <td>No Hair</td>
      <td>191.0</td>
      <td>Dark Horse Comics</td>
      <td>blue</td>
      <td>good</td>
      <td>65.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Abin Sur</td>
      <td>Male</td>
      <td>blue</td>
      <td>Ungaran</td>
      <td>No Hair</td>
      <td>185.0</td>
      <td>DC Comics</td>
      <td>red</td>
      <td>good</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Abomination</td>
      <td>Male</td>
      <td>green</td>
      <td>Human / Radiation</td>
      <td>No Hair</td>
      <td>203.0</td>
      <td>Marvel Comics</td>
      <td>-</td>
      <td>bad</td>
      <td>441.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Abraxas</td>
      <td>Male</td>
      <td>blue</td>
      <td>Cosmic Entity</td>
      <td>Black</td>
      <td>-99.0</td>
      <td>Marvel Comics</td>
      <td>-</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
  </tbody>
</table>
</div>



## Dealing with Null Values

Starting in the cell below, detect and deal with any null values in either data frame.  Then, explain your methodology for detecting and dealing with outliers in the markdown section below.  Be sure to explain your strategy for dealing with null values in numeric columns, as well as your strategy for dealing with null values in non-numeric columns.  

Note that if you need to add more cells to write code in, you can do this by:

**1.** Highlighting a cell and then pressing `ESC` to enter command mode.  
**2.** Press `A` to add a cell above the highlighted cell, or `B` to add a cell below the highlighted cell. 

Describe your strategy below this line:
____________________________________________________________________________________________________________________________




# Notice Publisher has null values


```python
# How do we even spot null values
heroes_df.isna().sum()
```




    name           0
    Gender         0
    Eye color      0
    Race           0
    Hair color     0
    Height         0
    Publisher     15
    Skin color     0
    Alignment      0
    Weight         2
    dtype: int64




```python
# let's look at how many rows we have to measure the impact of 15 null rows
heroes_df.shape
```




    (734, 10)



### Dropping Publisher null values below


```python
heroes_df.dropna(how='any', subset=['Publisher'], inplace=True)
print(heroes_df.isna().sum())
print(heroes_df.shape)
```

    name          0
    Gender        0
    Eye color     0
    Race          0
    Hair color    0
    Height        0
    Publisher     0
    Skin color    0
    Alignment     0
    Weight        0
    dtype: int64
    (719, 10)



```python
## Should we just fill the null values with 'Unknown' and make it a category 
## Unknown (Astro Boy - Marvel Comics)
```


```python
heroes_df.loc[heroes_df.Publisher.isna(), 'name'] # all null values are gone
```




    Series([], Name: name, dtype: object)



## Investigating Skin Color


```python
heroes_df["Skin color"].unique()
```




    array(['-', 'blue', 'red', 'black', 'grey', 'gold', 'green', 'white',
           'pink', 'silver', 'red / black', 'yellow', 'purple',
           'orange / white', 'gray', 'blue-white', 'orange'], dtype=object)




```python
heroes_df["Skin color"].value_counts() # how do we handle skin color place holders?
```




    -                 649
    green              21
    blue                9
    red                 8
    white               7
    silver              5
    grey                4
    purple              3
    gold                3
    pink                2
    yellow              2
    orange / white      1
    blue-white          1
    red / black         1
    black               1
    orange              1
    gray                1
    Name: Skin color, dtype: int64



### Drop skin color column, because so many unknown values


```python
heroes_df.drop(columns=['Skin color'], inplace=True)
```


```python
heroes_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>Gender</th>
      <th>Eye color</th>
      <th>Race</th>
      <th>Hair color</th>
      <th>Height</th>
      <th>Publisher</th>
      <th>Alignment</th>
      <th>Weight</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A-Bomb</td>
      <td>Male</td>
      <td>yellow</td>
      <td>Human</td>
      <td>No Hair</td>
      <td>203.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>441.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Abe Sapien</td>
      <td>Male</td>
      <td>blue</td>
      <td>Icthyo Sapien</td>
      <td>No Hair</td>
      <td>191.0</td>
      <td>Dark Horse Comics</td>
      <td>good</td>
      <td>65.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Abin Sur</td>
      <td>Male</td>
      <td>blue</td>
      <td>Ungaran</td>
      <td>No Hair</td>
      <td>185.0</td>
      <td>DC Comics</td>
      <td>good</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Abomination</td>
      <td>Male</td>
      <td>green</td>
      <td>Human / Radiation</td>
      <td>No Hair</td>
      <td>203.0</td>
      <td>Marvel Comics</td>
      <td>bad</td>
      <td>441.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Abraxas</td>
      <td>Male</td>
      <td>blue</td>
      <td>Cosmic Entity</td>
      <td>Black</td>
      <td>-99.0</td>
      <td>Marvel Comics</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
  </tbody>
</table>
</div>



## Investigating Gender


```python
heroes_df.columns
```




    Index(['name', 'Gender', 'Eye color', 'Race', 'Hair color', 'Height',
           'Publisher', 'Alignment', 'Weight'],
          dtype='object')




```python
heroes_df.Gender.value_counts()
```




    Male      494
    Female    198
    -          27
    Name: Gender, dtype: int64




```python
# slice the dataframe for male/female gender values
heroes_df = heroes_df.loc[(heroes_df.Gender == 'Male') | (heroes_df.Gender == 'Female')]
heroes_df.Gender.value_counts()
```




    Male      494
    Female    198
    Name: Gender, dtype: int64




```python
plt.figure(figsize=(8, 6))
plt.hist(heroes_df.Height, bins=20)
plt.vlines(ymin=0, ymax=400, x=heroes_df.Height.mean(), colors='r', label='mean')
plt.vlines(ymin=0, ymax=400, x=heroes_df.Height.median(), colors='g', label='median')
plt.legend()
plt.show()
```


![png](index_files/index_32_0.png)



```python
heroes_df.Height.describe() # descriptive statistics (mean, median) - mean, std, 5 point statistics (quartiles)
```




    count    692.000000
    mean     106.689306
    std      138.547728
    min      -99.000000
    25%      -99.000000
    50%      175.000000
    75%      188.000000
    max      975.000000
    Name: Height, dtype: float64




```python
heroes_df.Height.value_counts()
```




    -99.0     195
     183.0     57
     188.0     50
     180.0     38
     178.0     36
     185.0     34
     175.0     32
     168.0     28
     165.0     26
     170.0     25
     191.0     20
     193.0     18
     173.0     17
     198.0     17
     201.0     11
     196.0     10
     163.0      8
     213.0      7
     203.0      5
     211.0      5
     157.0      5
     244.0      4
     229.0      3
     155.0      3
     218.0      3
     226.0      3
     206.0      2
     279.0      2
     366.0      2
     137.0      2
     305.0      2
     122.0      2
     61.0       1
     30.5       1
     975.0      1
     142.0      1
     297.0      1
     267.0      1
     304.8      1
     701.0      1
     876.0      1
     259.0      1
     15.2       1
     287.0      1
     62.5       1
     257.0      1
     66.0       1
     160.0      1
     234.0      1
     71.0       1
     79.0       1
     64.0       1
    Name: Height, dtype: int64



## set all of the -99 values to the median


```python
height_median = heroes_df.Height.median()
```


```python
heroes_df.Height[heroes_df['Height'] == -99] = height_median
```


```python
heroes_df[heroes_df['Height'] == height_median]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>Gender</th>
      <th>Eye color</th>
      <th>Race</th>
      <th>Hair color</th>
      <th>Height</th>
      <th>Publisher</th>
      <th>Alignment</th>
      <th>Weight</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>Abraxas</td>
      <td>Male</td>
      <td>blue</td>
      <td>Cosmic Entity</td>
      <td>Black</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Adam Monroe</td>
      <td>Male</td>
      <td>blue</td>
      <td>-</td>
      <td>Blond</td>
      <td>175.0</td>
      <td>NBC - Heroes</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Alex Mercer</td>
      <td>Male</td>
      <td>-</td>
      <td>Human</td>
      <td>-</td>
      <td>175.0</td>
      <td>Wildstorm</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Alex Woolsly</td>
      <td>Male</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>NBC - Heroes</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Allan Quatermain</td>
      <td>Male</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>Wildstorm</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Ando Masahashi</td>
      <td>Male</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>NBC - Heroes</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Angel</td>
      <td>Male</td>
      <td>-</td>
      <td>Vampire</td>
      <td>-</td>
      <td>175.0</td>
      <td>Dark Horse Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Angela</td>
      <td>Female</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>Image Comics</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Anti-Spawn</td>
      <td>Male</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>Image Comics</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Aquababy</td>
      <td>Male</td>
      <td>blue</td>
      <td>-</td>
      <td>Blond</td>
      <td>175.0</td>
      <td>DC Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Arachne</td>
      <td>Female</td>
      <td>blue</td>
      <td>Human</td>
      <td>Blond</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>63.0</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Arsenal</td>
      <td>Male</td>
      <td>-</td>
      <td>Human</td>
      <td>-</td>
      <td>175.0</td>
      <td>DC Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>50</th>
      <td>Atom</td>
      <td>Male</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>DC Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>53</th>
      <td>Atom III</td>
      <td>Male</td>
      <td>-</td>
      <td>-</td>
      <td>Red</td>
      <td>175.0</td>
      <td>DC Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>54</th>
      <td>Atom IV</td>
      <td>Male</td>
      <td>brown</td>
      <td>-</td>
      <td>Black</td>
      <td>175.0</td>
      <td>DC Comics</td>
      <td>good</td>
      <td>72.0</td>
    </tr>
    <tr>
      <th>57</th>
      <td>Azrael</td>
      <td>Male</td>
      <td>brown</td>
      <td>Human</td>
      <td>Black</td>
      <td>175.0</td>
      <td>DC Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>58</th>
      <td>Aztar</td>
      <td>Male</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>DC Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>62</th>
      <td>Batgirl</td>
      <td>Female</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>DC Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>64</th>
      <td>Batgirl III</td>
      <td>Female</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>DC Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>66</th>
      <td>Batgirl V</td>
      <td>Female</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>DC Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>73</th>
      <td>Beak</td>
      <td>Male</td>
      <td>black</td>
      <td>-</td>
      <td>White</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>63.0</td>
    </tr>
    <tr>
      <th>76</th>
      <td>Beetle</td>
      <td>Male</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>77</th>
      <td>Ben 10</td>
      <td>Male</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>DC Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>79</th>
      <td>Beyonder</td>
      <td>Male</td>
      <td>-</td>
      <td>God / Eternal</td>
      <td>-</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>81</th>
      <td>Big Daddy</td>
      <td>Male</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>Icon Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>83</th>
      <td>Bill Harken</td>
      <td>Male</td>
      <td>-</td>
      <td>Alpha</td>
      <td>-</td>
      <td>175.0</td>
      <td>SyFy</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>84</th>
      <td>Billy Kincaid</td>
      <td>Male</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>Image Comics</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>88</th>
      <td>Bird-Man</td>
      <td>Male</td>
      <td>-</td>
      <td>Human</td>
      <td>-</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>89</th>
      <td>Bird-Man II</td>
      <td>Male</td>
      <td>-</td>
      <td>Human</td>
      <td>-</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>90</th>
      <td>Birdman</td>
      <td>Male</td>
      <td>-</td>
      <td>God / Eternal</td>
      <td>-</td>
      <td>175.0</td>
      <td>Hanna-Barbera</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>619</th>
      <td>Spider-Carnage</td>
      <td>Male</td>
      <td>-</td>
      <td>Symbiote</td>
      <td>-</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>626</th>
      <td>Spider-Woman II</td>
      <td>Female</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>631</th>
      <td>Stacy X</td>
      <td>Female</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>633</th>
      <td>Stardust</td>
      <td>Male</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>638</th>
      <td>Stephanie Powell</td>
      <td>Female</td>
      <td>-</td>
      <td>-</td>
      <td>Blond</td>
      <td>175.0</td>
      <td>ABC Studios</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>647</th>
      <td>Swamp Thing</td>
      <td>Male</td>
      <td>red</td>
      <td>God / Eternal</td>
      <td>No Hair</td>
      <td>175.0</td>
      <td>DC Comics</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>649</th>
      <td>Sylar</td>
      <td>Male</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>NBC - Heroes</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>652</th>
      <td>T-800</td>
      <td>Male</td>
      <td>red</td>
      <td>Cyborg</td>
      <td>-</td>
      <td>175.0</td>
      <td>Dark Horse Comics</td>
      <td>bad</td>
      <td>176.0</td>
    </tr>
    <tr>
      <th>653</th>
      <td>T-850</td>
      <td>Male</td>
      <td>red</td>
      <td>Cyborg</td>
      <td>-</td>
      <td>175.0</td>
      <td>Dark Horse Comics</td>
      <td>bad</td>
      <td>198.0</td>
    </tr>
    <tr>
      <th>654</th>
      <td>T-X</td>
      <td>Female</td>
      <td>-</td>
      <td>Cyborg</td>
      <td>-</td>
      <td>175.0</td>
      <td>Dark Horse Comics</td>
      <td>bad</td>
      <td>149.0</td>
    </tr>
    <tr>
      <th>662</th>
      <td>Thor Girl</td>
      <td>Female</td>
      <td>blue</td>
      <td>Asgardian</td>
      <td>Blond</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>143.0</td>
    </tr>
    <tr>
      <th>664</th>
      <td>Thunderbird II</td>
      <td>Male</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>665</th>
      <td>Thunderbird III</td>
      <td>Male</td>
      <td>brown</td>
      <td>-</td>
      <td>Black</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>74.0</td>
    </tr>
    <tr>
      <th>671</th>
      <td>Titan</td>
      <td>Male</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>HarperCollins</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>672</th>
      <td>Toad</td>
      <td>Male</td>
      <td>black</td>
      <td>Mutant</td>
      <td>Brown</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>neutral</td>
      <td>76.0</td>
    </tr>
    <tr>
      <th>675</th>
      <td>Tracy Strauss</td>
      <td>Female</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>NBC - Heroes</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>677</th>
      <td>Trigon</td>
      <td>Male</td>
      <td>yellow</td>
      <td>God / Eternal</td>
      <td>Black</td>
      <td>175.0</td>
      <td>DC Comics</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>685</th>
      <td>Valerie Hart</td>
      <td>Female</td>
      <td>hazel</td>
      <td>-</td>
      <td>Black</td>
      <td>175.0</td>
      <td>Team Epic TV</td>
      <td>good</td>
      <td>56.0</td>
    </tr>
    <tr>
      <th>690</th>
      <td>Venom II</td>
      <td>Male</td>
      <td>brown</td>
      <td>-</td>
      <td>Black</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>bad</td>
      <td>50.0</td>
    </tr>
    <tr>
      <th>696</th>
      <td>Vindicator</td>
      <td>Male</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>697</th>
      <td>Violator</td>
      <td>Male</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>Image Comics</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>701</th>
      <td>Vixen</td>
      <td>Female</td>
      <td>amber</td>
      <td>Human</td>
      <td>Black</td>
      <td>175.0</td>
      <td>DC Comics</td>
      <td>good</td>
      <td>63.0</td>
    </tr>
    <tr>
      <th>702</th>
      <td>Vulcan</td>
      <td>Male</td>
      <td>black</td>
      <td>-</td>
      <td>Black</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>711</th>
      <td>Watcher</td>
      <td>Male</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>712</th>
      <td>Weapon XI</td>
      <td>Male</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>713</th>
      <td>White Canary</td>
      <td>Female</td>
      <td>brown</td>
      <td>Human</td>
      <td>Black</td>
      <td>175.0</td>
      <td>DC Comics</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>715</th>
      <td>Wildfire</td>
      <td>Male</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>DC Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>716</th>
      <td>Winter Soldier</td>
      <td>Male</td>
      <td>brown</td>
      <td>Human</td>
      <td>Brown</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>117.0</td>
    </tr>
    <tr>
      <th>723</th>
      <td>Wondra</td>
      <td>Female</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>726</th>
      <td>X-Man</td>
      <td>Male</td>
      <td>blue</td>
      <td>-</td>
      <td>Brown</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>61.0</td>
    </tr>
  </tbody>
</table>
<p>227 rows × 9 columns</p>
</div>



## Investigating Eye Color


```python
heroes_df["Eye color"].value_counts()
```




    blue                       218
    -                          155
    brown                      119
    green                       73
    red                         41
    black                       21
    yellow                      17
    white                       16
    grey                         6
    hazel                        6
    purple                       4
    gold                         3
    amber                        2
    violet                       2
    yellow / blue                1
    green / blue                 1
    blue / white                 1
    yellow / red                 1
    yellow (without irises)      1
    indigo                       1
    white / red                  1
    silver                       1
    bown                         1
    Name: Eye color, dtype: int64




```python
## Many '-' values so set them to their own category
heroes_df.loc[heroes_df["Eye color"]=='-', "Eye color"] = 'unknown'
heroes_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>Gender</th>
      <th>Eye color</th>
      <th>Race</th>
      <th>Hair color</th>
      <th>Height</th>
      <th>Publisher</th>
      <th>Alignment</th>
      <th>Weight</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A-Bomb</td>
      <td>Male</td>
      <td>yellow</td>
      <td>Human</td>
      <td>No Hair</td>
      <td>203.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>441.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Abe Sapien</td>
      <td>Male</td>
      <td>blue</td>
      <td>Icthyo Sapien</td>
      <td>No Hair</td>
      <td>191.0</td>
      <td>Dark Horse Comics</td>
      <td>good</td>
      <td>65.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Abin Sur</td>
      <td>Male</td>
      <td>blue</td>
      <td>Ungaran</td>
      <td>No Hair</td>
      <td>185.0</td>
      <td>DC Comics</td>
      <td>good</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Abomination</td>
      <td>Male</td>
      <td>green</td>
      <td>Human / Radiation</td>
      <td>No Hair</td>
      <td>203.0</td>
      <td>Marvel Comics</td>
      <td>bad</td>
      <td>441.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Abraxas</td>
      <td>Male</td>
      <td>blue</td>
      <td>Cosmic Entity</td>
      <td>Black</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
  </tbody>
</table>
</div>



## Investigating Race and Hair color


```python
heroes_df.drop(columns=["Race"], inplace=True)
heroes_df[:10]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>Gender</th>
      <th>Eye color</th>
      <th>Hair color</th>
      <th>Height</th>
      <th>Publisher</th>
      <th>Alignment</th>
      <th>Weight</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A-Bomb</td>
      <td>Male</td>
      <td>yellow</td>
      <td>No Hair</td>
      <td>203.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>441.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Abe Sapien</td>
      <td>Male</td>
      <td>blue</td>
      <td>No Hair</td>
      <td>191.0</td>
      <td>Dark Horse Comics</td>
      <td>good</td>
      <td>65.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Abin Sur</td>
      <td>Male</td>
      <td>blue</td>
      <td>No Hair</td>
      <td>185.0</td>
      <td>DC Comics</td>
      <td>good</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Abomination</td>
      <td>Male</td>
      <td>green</td>
      <td>No Hair</td>
      <td>203.0</td>
      <td>Marvel Comics</td>
      <td>bad</td>
      <td>441.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Abraxas</td>
      <td>Male</td>
      <td>blue</td>
      <td>Black</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Absorbing Man</td>
      <td>Male</td>
      <td>blue</td>
      <td>No Hair</td>
      <td>193.0</td>
      <td>Marvel Comics</td>
      <td>bad</td>
      <td>122.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Adam Monroe</td>
      <td>Male</td>
      <td>blue</td>
      <td>Blond</td>
      <td>175.0</td>
      <td>NBC - Heroes</td>
      <td>good</td>
      <td>-99.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Adam Strange</td>
      <td>Male</td>
      <td>blue</td>
      <td>Blond</td>
      <td>185.0</td>
      <td>DC Comics</td>
      <td>good</td>
      <td>88.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Agent 13</td>
      <td>Female</td>
      <td>blue</td>
      <td>Blond</td>
      <td>173.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>61.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Agent Bob</td>
      <td>Male</td>
      <td>brown</td>
      <td>Brown</td>
      <td>178.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>81.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
heroes_df["Hair color"].value_counts()
```




    -                   154
    Black               151
    Blond                97
    Brown                82
    No Hair              70
    Red                  51
    White                22
    Auburn               13
    Green                 8
    Strawberry Blond      7
    Purple                5
    Grey                  4
    Brown / White         4
    Silver                3
    black                 3
    blond                 3
    Blue                  3
    Orange                2
    Yellow                1
    Indigo                1
    Pink                  1
    Red / Grey            1
    Black / Blue          1
    Gold                  1
    Red / Orange          1
    Magenta               1
    Brownn                1
    Red / White           1
    Name: Hair color, dtype: int64




```python
# pandas stuff
heroes_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>Gender</th>
      <th>Eye color</th>
      <th>Hair color</th>
      <th>Height</th>
      <th>Publisher</th>
      <th>Alignment</th>
      <th>Weight</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A-Bomb</td>
      <td>Male</td>
      <td>yellow</td>
      <td>No Hair</td>
      <td>203.0</td>
      <td>Marvel Comics</td>
      <td>good</td>
      <td>441.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Abe Sapien</td>
      <td>Male</td>
      <td>blue</td>
      <td>No Hair</td>
      <td>191.0</td>
      <td>Dark Horse Comics</td>
      <td>good</td>
      <td>65.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Abin Sur</td>
      <td>Male</td>
      <td>blue</td>
      <td>No Hair</td>
      <td>185.0</td>
      <td>DC Comics</td>
      <td>good</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Abomination</td>
      <td>Male</td>
      <td>green</td>
      <td>No Hair</td>
      <td>203.0</td>
      <td>Marvel Comics</td>
      <td>bad</td>
      <td>441.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Abraxas</td>
      <td>Male</td>
      <td>blue</td>
      <td>Black</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>bad</td>
      <td>-99.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# zscore and how we can use it
```


```python
plt.scatter(heroes_df.Height, heroes_df.Weight)
```




    <matplotlib.collections.PathCollection at 0x1a12e0db00>




![png](index_files/index_47_1.png)



```python
# dummify the alignment column
```


```python
heroes_df2 = pd.get_dummies(heroes_df, columns=["Alignment"])
heroes_df2.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>Gender</th>
      <th>Eye color</th>
      <th>Hair color</th>
      <th>Height</th>
      <th>Publisher</th>
      <th>Weight</th>
      <th>Alignment_-</th>
      <th>Alignment_bad</th>
      <th>Alignment_good</th>
      <th>Alignment_neutral</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>A-Bomb</td>
      <td>Male</td>
      <td>yellow</td>
      <td>No Hair</td>
      <td>203.0</td>
      <td>Marvel Comics</td>
      <td>441.0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Abe Sapien</td>
      <td>Male</td>
      <td>blue</td>
      <td>No Hair</td>
      <td>191.0</td>
      <td>Dark Horse Comics</td>
      <td>65.0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Abin Sur</td>
      <td>Male</td>
      <td>blue</td>
      <td>No Hair</td>
      <td>185.0</td>
      <td>DC Comics</td>
      <td>90.0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Abomination</td>
      <td>Male</td>
      <td>green</td>
      <td>No Hair</td>
      <td>203.0</td>
      <td>Marvel Comics</td>
      <td>441.0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Abraxas</td>
      <td>Male</td>
      <td>blue</td>
      <td>Black</td>
      <td>175.0</td>
      <td>Marvel Comics</td>
      <td>-99.0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



## Joining, Grouping, and Aggregating

In the cell below, join the two DataFrames.  Think about which sort of join you should use, as well as which columns you should join on.  Rename columns and manipulate as needed.  

**_HINT:_** If the join throws an error message, consider settin the the column you want to join on as the index for each DataFrame.  


```python
from sklearn.datasets import load_boston
```


```python
boston = load_boston()
```


```python
df = pd.DataFrame(np.column_stack([boston.data, boston.target]), columns=list(boston.feature_names) + ["target"])
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CRIM</th>
      <th>ZN</th>
      <th>INDUS</th>
      <th>CHAS</th>
      <th>NOX</th>
      <th>RM</th>
      <th>AGE</th>
      <th>DIS</th>
      <th>RAD</th>
      <th>TAX</th>
      <th>PTRATIO</th>
      <th>B</th>
      <th>LSTAT</th>
      <th>target</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.00632</td>
      <td>18.0</td>
      <td>2.31</td>
      <td>0.0</td>
      <td>0.538</td>
      <td>6.575</td>
      <td>65.2</td>
      <td>4.0900</td>
      <td>1.0</td>
      <td>296.0</td>
      <td>15.3</td>
      <td>396.90</td>
      <td>4.98</td>
      <td>24.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.02731</td>
      <td>0.0</td>
      <td>7.07</td>
      <td>0.0</td>
      <td>0.469</td>
      <td>6.421</td>
      <td>78.9</td>
      <td>4.9671</td>
      <td>2.0</td>
      <td>242.0</td>
      <td>17.8</td>
      <td>396.90</td>
      <td>9.14</td>
      <td>21.6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.02729</td>
      <td>0.0</td>
      <td>7.07</td>
      <td>0.0</td>
      <td>0.469</td>
      <td>7.185</td>
      <td>61.1</td>
      <td>4.9671</td>
      <td>2.0</td>
      <td>242.0</td>
      <td>17.8</td>
      <td>392.83</td>
      <td>4.03</td>
      <td>34.7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.03237</td>
      <td>0.0</td>
      <td>2.18</td>
      <td>0.0</td>
      <td>0.458</td>
      <td>6.998</td>
      <td>45.8</td>
      <td>6.0622</td>
      <td>3.0</td>
      <td>222.0</td>
      <td>18.7</td>
      <td>394.63</td>
      <td>2.94</td>
      <td>33.4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.06905</td>
      <td>0.0</td>
      <td>2.18</td>
      <td>0.0</td>
      <td>0.458</td>
      <td>7.147</td>
      <td>54.2</td>
      <td>6.0622</td>
      <td>3.0</td>
      <td>222.0</td>
      <td>18.7</td>
      <td>396.90</td>
      <td>5.33</td>
      <td>36.2</td>
    </tr>
  </tbody>
</table>
</div>



In the cell below, subset male and female heroes into different dataframes.  Create a scatterplot of the height and weight of each hero, with weight as the y-axis.  Plot both the male and female heroes subset into each dataframe, and make the color for each point in the scatterplot correspond to the gender of the superhero.


```python
def zscores(arr):
    return (arr - arr.mean())/arr.std()
```


```python
plt.hist(df.NOX)
plt.title("NOX")
plt.show()
plt.hist(df.AGE)
plt.title("AGE")
plt.show()
```


![png](index_files/index_56_0.png)



![png](index_files/index_56_1.png)



```python
# zscores can scale your data
z_age = zscores(df.AGE)
z_nox = zscores(df.NOX)
```


```python
plt.hist(z_nox)
plt.title("NOX")
plt.show()
plt.hist(z_age)
plt.title("AGE")
plt.show()
```


![png](index_files/index_58_0.png)



![png](index_files/index_58_1.png)



```python
# normalizing (taking skewed data and making it normal)
plt.hist(np.log(df.AGE))
plt.show()
```


![png](index_files/index_59_0.png)



```python
plt.hist(np.log(df.target))
```




    (array([  3.,   8.,  13.,  23.,  63., 100., 172.,  52.,  40.,  32.]),
     array([1.60943791, 1.83969642, 2.06995493, 2.30021344, 2.53047195,
            2.76073046, 2.99098897, 3.22124748, 3.45150599, 3.6817645 ,
            3.91202301]),
     <a list of 10 Patch objects>)




![png](index_files/index_60_1.png)


## Some Initial Investigation

Next, slice the DataFrame as needed and visualize the distribution of heights and weights by gender.  You should have 4 total plots.  

In the cell below:

* Slice the DataFrame into separate DataFrames by gender
* Complete the `show_distplot` function.  This helper function should take in a DataFrame, a string containing the gender we want to visualize, and and the column name we want to visualize by gender. The function should display a distplot visualization from seaborn of the column/gender combination.  

Hint: Don't forget to check the [seaborn documentation for distplot](https://seaborn.pydata.org/generated/seaborn.distplot.html) if you have questions about how to use it correctly! 


```python
male_heroes_df = None
female_heroes_df = None

def show_distplot(dataframe, gender, column_name):
    pass
```


```python
# Male Height

```


```python
# Male Weight

```


```python
# Female Height

```


```python
# Female Weight

```

Discuss your findings from the plots above, with respect to the distibution of height and weight by gender.  Your explanation should include discussion of any relevant summary statistics, including mean, median, mode, and the overall shape of each distribution.  

Wite your answer below this line:
____________________________________________________________________________________________________________________________



### Sample Question: Most Common Powers

The rest of this notebook will be left to you to investigate the dataset by formulating your own questions, and then seeking answers using pandas and numpy.  Every answer should include some sort of visualization, when appropriate. Before moving on to formulating your own questions, use the dataset to answer the following questions about superhero powers:

* What are the 5 most common powers overall?
* What are the 5 most common powers in the Marvel Universe?
* What are the 5 most common powers in the DC Universe?

Analyze the results you found above to answer the following question:

How do the top 5 powers in the Marvel and DC universes compare?  Are they similar, or are there significant differences? How do they compare to the overall trends in the entire Superheroes dataset?

Wite your answer below this line:
____________________________________________________________________________________________________________________________


### Your Own Investigation

For the remainder of this lab, you'll be focusing on coming up with and answering your own question, just like we did above.  Your question should not be overly simple, and should require both descriptive statistics and data visualization to answer.  In case you're unsure of what questions to ask, some sample questions have been provided below.

Pick one of the following questions to investigate and answer, or come up with one of your own!

* Which powers have the highest chance of co-occuring in a hero (e.g. super strength and flight), and does this differ by gender?
* Is there a relationship between a hero's height and weight and their powerset?
* What is the distribution of skin colors amongst alien heroes?

Explain your question below this line:
____________________________________________________________________________________________________________________________



Some sample cells have been provided to give you room to work. If you need to create more cells, you can do this easily by:

1. Highlighting a cell and then pressing `esc` to enter command mode.
1. Pressing `b` to add a cell below the currently highlighted cell, or `a` to add one above it.  

Be sure to include thoughtful, well-labeled visualizations to back up your analysis!

## Summary

In this lab, we demonstrated our mastery of:
* Using all of our Pandas knowledge to date to clean the dataset and deal with null values
* Using Queries and aggregations to group the data into interesting subsets as needed
* Using descriptive statistics and data visualization to find answers to questions we may have about the data
