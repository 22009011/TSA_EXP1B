# Ex.No: 1B                     CONVERSION OF NON STATIONARY TO STATIONARY DATA

# Developed By: Thanjiyappan k

# Register No: 212222240108

# Date: 

### AIM:
To perform regular differncing,seasonal adjustment and log transformatio on Ev Sales dataset
### ALGORITHM:
1. Import the required packages like pandas and numpy
2. Read the data using the pandas
3. Perform the data preprocessing if needed and apply regular differncing,seasonal adjustment,log transformation.
4. Plot the data according to need, before and after regular differncing,seasonal adjustment,log transformation.
5. Display the overall results.
### PROGRAM:
#### IMPORTING PACKAGES:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
```
#### Preprocessing:
```
train=pd.read_csv('EV.csv')
train.timestamp=pd.to_datetime(train.year,format='%Y')
train.index=train.timestamp
train.drop("year",axis=1,inplace=True)
train.head()
print(train.columns)
train.columns = train.columns.str.strip()
train['value'].plot()
```
#### REGULAR DIFFERENCING:
```
from statsmodels.tsa.stattools import adfuller
def adf_test(timeseries):
    print("Results of Dickey-Fuller Test:")
    dftest = adfuller(timeseries, autolag="AIC")
    dfoutput = pd.Series(dftest[0:4], index=["Test Statistic", "p-value", "#Lags Used", "Number of Observations Used"])
    for key, value in dftest[4].items():
        dfoutput["Critical Value (%s)" % key] = value
    print(dfoutput)
adf_test(train['value'])
train['value_diff']=train['value']-train['value'].shift(1)
train['value_diff'].dropna().plot()
```
#### SEASONAL DIFFERENCING:
```
n=7
train['value_diff']=train['value']-train['value'].shift(n)
train['value_diff'].dropna().plot()
```
#### LOG TRANSFORMATION:
```
train['value_log']=np.log(train['value'])
train['value_log_diff']=train['value_log']-train['value_log'].shift(1)
train['value_log_diff'].dropna().plot()
```


### OUTPUT:


REGULAR DIFFERENCING:
![image](https://github.com/user-attachments/assets/45dca2b7-6ba3-4faa-b29f-f769d7a517b2)



SEASONAL ADJUSTMENT:
![image](https://github.com/user-attachments/assets/c6d246c9-6423-4fee-8fd9-eaa9c43ad10e)



LOG TRANSFORMATION:
![image](https://github.com/user-attachments/assets/0efd8b1a-4a6a-4920-b015-90783ed53176)




### RESULT:
Thus we have successfully created the python code for the conversion of non stationary to stationary data of EV Sales dataset.
