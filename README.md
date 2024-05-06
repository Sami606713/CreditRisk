# Credit Risk Model Building

- This dataset contains information about bank customers applying for loans.
- Our goal is to build a model that determines whether the customer is eligible for a loan.

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