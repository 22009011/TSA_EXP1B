# Ex.No: 1B                     CONVERSION OF NON STATIONARY TO STATIONARY DATA

# Developed By: Thanjiyappan k

# Register No: 212222240108

# Date: 

### AIM:
To perform regular differncing,seasonal adjustment and log transformatio on Ev Sales dataset
### ALGORITHM:
1.Import Necessary Libraries: Import pandas, numpy, matplotlib.pyplot, and adfuller for data manipulation and time series analysis.

2.Load and Filter Dataset: Load the EV sales dataset, filter it for the desired parameter (e.g., "EV sales"), and convert the 'year' column to datetime format.

3.Set Datetime Index: Set the converted datetime column as the index of the DataFrame for time series operations.

4.Perform and Plot ADF Test: Conduct the Augmented Dickey-Fuller test to check for stationarity and plot the original EV sales data.

5.Apply Transformations: Perform regular differencing, seasonal differencing, and log transformation on the EV sales data, storing each result in new columns.

6.Visualize Transformed Data: Plot the original data, along with the differenced, seasonally adjusted, and log-transformed series to compare the effects of each transformation.
### PROGRAM:
#### IMPORTING PACKAGES:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.stattools import adfuller

file_path = 'EV.csv'  # Replace with your file path
data = pd.read_csv(file_path)

ev_sales = data[(data['parameter'] == 'EV sales') & (data['unit'] == 'Vehicles')].copy()

ev_sales['timestamp'] = pd.to_datetime(ev_sales['year'], format='%Y')

ev_sales.set_index('timestamp', inplace=True)

ev_sales.columns = ev_sales.columns.str.strip()

ev_sales['value'].plot(title='EV Sales Over Time', figsize=(10, 6))

def adf_test(timeseries):
    print("Results of Dickey-Fuller Test:")
    dftest = adfuller(timeseries, autolag="AIC")
    dfoutput = pd.Series(dftest[0:4], index=["Test Statistic", "p-value", "#Lags Used", "Number of Observations Used"])
    for key, value in dftest[4].items():
        dfoutput["Critical Value (%s)" % key] = value
    print(dfoutput)
adf_test(ev_sales['value'])

ev_sales['value_diff'] = ev_sales['value'] - ev_sales['value'].shift(1)
ev_sales['value_seasonal_diff'] = ev_sales['value'] - ev_sales['value'].shift(12)
ev_sales['value_log'] = np.log(ev_sales['value'])
ev_sales['value_log_diff'] = ev_sales['value_log'] - ev_sales['value_log'].shift(1)
fig, axes = plt.subplots(5, 1, figsize=(10, 12), sharex=True)


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
axes[0].plot(ev_sales.index, ev_sales['value'], label='Original Value')
axes[0].set_title('Original EV Sales')
axes[0].legend(loc='upper left')

axes[1].plot(ev_sales.index, ev_sales['value_diff'], label='Differenced (lag=1)', color='orange')
axes[1].set_title('Regular Differencing (lag=1)')
axes[1].legend(loc='upper left')
```
#### SEASONAL DIFFERENCING:
```
axes[2].plot(ev_sales.index, ev_sales['value_seasonal_diff'], label='Seasonal Differencing (lag=12)', color='green')
axes[2].set_title('Seasonal Differencing (lag=12)')
axes[2].legend(loc='upper left')
```
#### LOG TRANSFORMATION:
```
axes[3].plot(ev_sales.index, ev_sales['value_log'], label='Log Transformation', color='red')
axes[3].set_title('Log Transformation')
axes[3].legend(loc='upper left')

axes[4].plot(ev_sales.index, ev_sales['value_log_diff'], label='Log Differencing (lag=1)', color='purple')
axes[4].set_title('Log Differencing (lag=1)')
axes[4].legend(loc='upper left')

plt.tight_layout()
plt.show()
```


### OUTPUT:


REGULAR DIFFERENCING:
![image](https://github.com/user-attachments/assets/0c970376-8d71-405f-89f3-08ce5f4dba09)








SEASONAL ADJUSTMENT:

![2](https://github.com/user-attachments/assets/b140c8de-8ad6-4206-9c6d-4bb5e22716e0)








LOG TRANSFORMATION:
![1](https://github.com/user-attachments/assets/04ef3805-e574-46e2-bf00-1f430c1d3249)





### RESULT:
Thus program was successfully created the python code for the conversion of non stationary to stationary data of EV Sales dataset.
