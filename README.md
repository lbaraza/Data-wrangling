## Data Cleaning and Normalization

This repository contains a Python script for cleaning and normalizing a dataset using pandas.
The steps performed include identifying and removing duplicate values,identifying and imputing missing values,and normalizing compensation data.

## Objectives

1. Identify duplicate values in the dataset.
2. Remove duplicate values from the dataset.
3. Identify missing values in the dataset.
4. Impute the missing values in the dataset.
5. Normalize data in the dataset.

## Steps

### 1. Import pandas module

```python
import pandas as pd
```

### 2. Load the dataset into a DataFrame

```python
df = pd.read_csv("https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DA0321EN-SkillsNetwork/LargeData/m1_survey_data.csv")
df.shape
# Output: (11552, 85)
```

### 3. Finding duplicates

Identify duplicate values in the dataset:

```python
# Find how many duplicate rows exist
len(df) - len(df.drop_duplicates())
# Output: 154
```

### 4. Removing duplicates

Remove duplicate rows from the DataFrame:

```python
df = df.drop_duplicates()
# Verify if duplicates were actually dropped
df.shape
# Output: (11398, 85)
```

### 5. Finding Missing values

Identify missing values for all columns:

```python
df.isnull().sum()
```

Find out how many rows are missing in the column `WorkLoc`:

```python
df['WorkLoc'].isnull().sum()
# Output: 32
```

### 6. Imputing missing values

Find the value counts for the column `WorkLoc`:

```python
df['WorkLoc'].value_counts()
```

Identify the value that is most frequent (majority) in the `WorkLoc` column.

### 7. Normalizing data

The dataset contains two columns related to compensation:

- `CompFreq`: How often a developer is paid (Yearly, Monthly, Weekly).
- `CompTotal`: Total compensation per Year, Month, or Week depending on `CompFreq`.

Create a new column `NormalizedAnnualCompensation` for annual compensation.

List out the various categories in the column `CompFreq`:

```python
df.CompFreq.value_counts().index
# Output: Index(['Yearly', 'Monthly', 'Weekly'], dtype='object')
```

Create the `NormalizedAnnualCompensation` column:

```python
anncomp = []

def NAC():
    for x, y in zip(df['CompFreq'], df['CompTotal']):
        if x == 'Monthly':
            anncomp.append(y * 12)
        elif x == 'Weekly':
            anncomp.append(y * 52)
        else:
            anncomp.append(y)

NAC()

df['NormalizedAnnualCompensation'] = anncomp
df[['NormalizedAnnualCompensation']]
```

## Results

The final DataFrame will include a new column `NormalizedAnnualCompensation` which makes it easier to compare the total compensation of developers regardless of the payment frequency.

```python
df[['NormalizedAnnualCompensation']]
```

## Conclusion

This repository demonstrates how to clean and normalize a dataset using pandas. By following the steps outlined,you can ensure that your data is free of duplicates, missing values are appropriately handled, and compensation data is normalized for easy comparison.


