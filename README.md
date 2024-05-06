# Credit Risk Model Building

# Problem Statement
- This Dataset Contain the information about the bank customer that will take loan.
- Our goal is build a model that will detect weather the customer is eligible for loan or not.
- Data should be present in 2 different files.
- Both conatin same data about bank.
- Both files also contain `-99999` values.
- `-99999` are the missing values.
- We can specific `-99999` as a `Null value` while reading the data.

## Reading Data

Data is present in another folder. Now we can read the data using `os`.

```python
import os

file_paths = []
data_folder = "Data"
for filename in os.listdir(data_folder):
    file_paths.append(os.path.join(os.getcwd(), data_folder, filename))
    print(os.path.join(os.getcwd(), data_folder, filename))
```
# Data Preprocessing
- Top records
- Shape of data
- Check columns
- Merge based on `common column`
- Check Datatyes
- Null values
- Duplicates
- Unique Values
- Statistical Summary

# Shape of Data 
- Data1 contain `51336` rows and `26` Column
- Data2 contain `51336` rows and `62` Column

# Missing Values
- we can see that in `df2` most of the columns contain `-99999` values.
- These `-99999` are basically `Missing value`.

# Merge `df1` and `df2`
```python
final_df=df1.merge(df2,how='inner',on='PROSPECTID')
```