# Credit Risk Model Building
![Credit Risk Management](Credit_img.jpeg)

# Problem Statement
- This Dataset Contain the information about the bank customer that will take loan.
- Our goal is build a model that will detect weather the customer is eligible for loan or not.
- Data should be present in 2 different files.
- Both conatin same data about bank.
- Both files also contain `-99999` values.
- `-99999` are the missing values.

## Reading Data

Data is present in another folder. Now we can read the data using `os`.

```python
import os

file_paths = []
data_folder = "Data"
for filename in os.listdir(data_folder):
    file_paths.append(os.path.join(os.getcwd(), data_folder, filename))
    print(os.path.join(os.getcwd(), data_folder, filename))

df1=pd.read_excel(file_path[0])
df2=pd.read_excel(file_path[1])
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
# Datatypes
```python
final_df.dtypes
```
# Handling Missing Values
- As mentioned earlier, both files contain `-99999` values, which represent missing values.
- Check if the ratio of `-99999` values is greater in a specific column and consider removing those columns.

# Conclusion
- After removing columns containing more than `10000` `-99999` values, the dataframe has `51336` rows and `79` columns.
- We will remove only those columns with more than `10000` `-99999` values, but keep in mind that `-99999` values are also present in columns with fewer than `10000` occurrences.

# Points to Remember
- There are two options for handling the remaining missing values:
    ## Remove Missing Values
    - We can remove the rows with missing values if the data balance allows it.
    ## Fill Missing Values
    - Filling missing values is critical, especially if removing them results in significant data loss.

# Conclusion (Handling -99999 Values)
- After removing rows containing `-99999` values, we can retain `80%` of the data.
- We have decided to remove the missing values.

# Duplicates
```python
final_df.duplicated().sum()
```

# Unique Values
- There are `2` unique values in  `MARITALSTATUS` i-e  `['Married' 'Single']`
- There are `7` unique values in  `EDUCATION` i-e `['12TH' 'GRADUATE' 'SSC' 'POST-GRADUATE' 'UNDER GRADUATE' 'OTHERS'
 'PROFESSIONAL']`
- There are `2` unique values in  `GENDER` i-e `['M' 'F']`
- There are `6` unique values in  `last_prod_enq2` i-e `['PL' 'ConsumerLoan' 'AL' 'CC' 'others' 'HL']`
- There are `6` unique values in  `first_prod_enq2` i-e `['PL' 'ConsumerLoan' 'others' 'AL' 'HL' 'CC']`
- There are `4` unique values in  `Approved_Flag` i-e `['P2' 'P1' 'P3' 'P4']`

# Feature Engnering
## Hypothesis Testing

## Variance Inflence factor
- In statistics, the variance inflation factor (VIF) is a ratio that measures the severity of multicollinearity in regression analysis.
- Multicollinearity occurs when two or more independent variables in a regression model have a linear relationship, which can negatively impact regression results. The VIF measures the number of inflated variances caused by multicollinearity.
- We can apply the `VIF` on numerical columns to check the relationship of each column to every other col.
- If the 2 or more columns are highly corelated so we can take only one column so that i will reduce the `multicolarnarty`.
- If we can reduce multicolarty the column will automaticlally reduce.
- we can set the threshold ratio if the variance is greater then treshold we can drop the columns.
- if the variance is less then treshold we can keep the columns.
```python
col_to_kepto=[]
vif_data=final_df[num_col]
column_index=0
for i in range(num_col.shape[0]):
    # variance_inflation_factor(dataframe,index)
    vif_value=variance_inflation_factor(vif_data,column_index)
    print(i,"------->",vif_value)
    
    if(vif_value<=6):
        col_to_kept.append(num_col[i])
        column_index+=1
    else:
        vif_data=vif_data.drop(columns=num_col[i])
```

# Conclussion
- After applying `variance inflence factor` we can reduce some column now the remaing `numerical column is 40`