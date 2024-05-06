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
