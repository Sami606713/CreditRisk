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


# ANOVA
- It is a hypothesis test in this test we can check the relationship b/w numerical col to target col.
- Target  col should be categorical and must contain 3 or greater then 3 categories.
- In our case our target col is `Approved_Flag` which contain `4 classes`.
- We can set the threshold `0.05` if the p_value is greater then `0.05` we can drop these columns.
- `0.05` threshold are generaly industry standard.
```python
col_to_remaing=[]
for i in col_to_kept:
    current_col=final_df[i]
    targte_col=final_df[cat_col[-1]]
    
    # Saperat each group for preforming ANOVA 
    group1=[value for value,group in zip(current_col,targte_col) if group=='P1']
    group2=[value for value,group in zip(current_col,targte_col) if group=='P2']
    group3=[value for value,group in zip(current_col,targte_col) if group=='P3']
    group4=[value for value,group in zip(current_col,targte_col) if group=='P4']
    
    # Perform ANOVA
    f_stats,p_value=f_oneway(group1,group2,group3,group4)
    print(i,"------>",p_value)
    
    if(p_value<0.05):
        col_to_remaing.append(i)
```
# Conclussion
- After `ANOVA` we can get only 38 columns.

# Chie-Square
- These test are generally perform on categorical column.
- Note that target column category should be `>=3`.
- Now using `chie-square` test we can check the relationship b/w target col.
```python
drop_col=[]
for i in cat_col[:-1]:
    # Perform chie-square
    chie,p_value,_,_=chi2_contingency(pd.crosstab(final_df[i],final_df[cat_col[-1]]))
    
    print(i,"----->",p_value)
    if(p_value>0.05):
        drop_col.append(i)
```
# Conclussion
- See that all the columns `p_value` is less the `0.05` so we can accept all the columns.

# Model Building
- First we can saperate the input and output columns.
- Second we can saperate numerical and categorical columns.
- Third we can build a pipeline for some transformation.

# Saperate Input and output col
```python
feature=final_df.drop(columns=['Approved_Flag'])
label=final_df['Approved_Flag']
```

# Encode target Colummn
```python
from sklearn.preprocessing import LabelEncoder,OneHotEncoder,StandardScaler
lb=LabelEncoder()
label=lb.fit_transform(label)
```
# Saperate `Numerical` and `Categorical` column
```python 
num_col=feature.select_dtypes('number').columns
cat_col=feature.select_dtypes('object').columns
```
# Train Test Split
```python
from sklearn.model_selection import train_test_split
x_train,x_test,y_train,y_test=train_test_split(feature,label,test_size=0.2,random_state=43)
```

# Building Pipeline
- we can build a pipeline for numercial column.
    - In numerical pipeline we can scale the feature.
- we can build a pipeline for categorical column.
    - In categorical pipeline we can encode the categorical column.

# Numerical Pipeline
```python
num_pipe=Pipeline(steps=[
    ("impute",SimpleImputer(strategy='median')),
    ('scale',StandardScaler())
])
num_pipe
```

# Categorical Pipeline
```python
cat_pipe=Pipeline(steps=[
    ("impute",SimpleImputer(strategy='most_frequent')),
    ('Encode',OneHotEncoder(drop='first',sparse=False,handle_unknown='ignore'))
])
cat_pipe
```

# Colum Transformer
- In column transformer we can combine al the steps.

```python
transformer=ColumnTransformer(transformers=[
    ('num_transformer',num_pipe,num_col),
    ("Encode",cat_pipe,cat_col)
],remainder='passthrough')
transformer
```
# Build Final Pipeline for model
```python
final=Pipeline(steps=[
    ("process",transformer),
    ("model",LogisticRegression())
])
```
# Fit the model
```python
final.fit(x_train,y_train)
```