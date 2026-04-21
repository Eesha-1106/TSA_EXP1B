# Ex.No: 1B                     CONVERSION OF NON STATIONARY TO STATIONARY DATA
# Date:21-04-2026

### AIM:
To perform regular differncing,seasonal adjustment and log transformatio on international airline passenger data
### ALGORITHM:
1. Import the required packages like pandas and numpy
2. Read the data using the pandas
3. Perform the data preprocessing if needed and apply regular differncing,seasonal adjustment,log transformation.
4. Plot the data according to need, before and after regular differncing,seasonal adjustment,log transformation.
5. Display the overall results.
### PROGRAM:
```py
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

from statsmodels.tsa.seasonal import seasonal_decompose

df=pd.read_csv("/content/Chocolate Sales (2).csv")
df.head()
df['Date']=pd.to_datetime(df['Date'],dayfirst=True)

df.set_index('Date',inplace=True)
df['Box_Shipped_diff']=df['Boxes Shipped']-df['Boxes Shipped'].shift(1)

result=seasonal_decompose(df['Boxes Shipped'],model='additive',period=12)
df['box_shipped_sea_diff']=result.resid
df['box_shipped_log']=np.log(df['Boxes Shipped'])
df['box_shipped_log_diff']=df['box_shipped_log']-df['box_shipped_log'].shift(1)
series_for_decompose = df['box_shipped_log_diff'].dropna()
if not series_for_decompose.index.is_unique:
    series_for_decompose = series_for_decompose.groupby(level=0).mean() 
result = seasonal_decompose(series_for_decompose, model='additive', period=12)
df['box_shipped_log_sea_diff'] = df.index.map(result.resid)

plt.figure(figsize=(16,16))
plt.plot(df['Boxes Shipped'],label='Original')
plt.legend(loc='best')
plt.title('Original Data')
plt.xlabel('Date')
plt.ylabel('Boxes Shipped')

plt.subplot(6, 1, 2)
plt.plot(df['Box_Shipped_diff'], label='Regular Difference')
plt.legend(loc='best')
plt.title('Regular Differencing')
plt.xlabel('Date')
plt.ylabel('Boxes shipped')

plt.subplot(6, 1, 3)
plt.plot(df['box_shipped_sea_diff'], label='Seasonal Adjustment')
plt.legend(loc='best')
plt.title('Seasonal Adjustment')
plt.xlabel('Date')
plt.ylabel('Seasonally adjusted No of box shipped')

plt.subplot(6, 1, 4)
plt.plot(df['box_shipped_log'], label='Log Transformation')
plt.legend(loc='best')
plt.title('Log Transformation')
plt.xlabel('Date')
plt.ylabel('Log(Box shipped)')

plt.subplot(6, 1, 5)
plt.plot(df['box_shipped_log_diff'], label='Log Transformation and Regular Differencing')
plt.legend(loc='best')
plt.title('Log Transformation and Regular Differencing')
plt.xlabel('Date')
plt.ylabel('RDiff(Log(box shipped))')

plt.subplot(6, 1, 6)
plt.plot(df['box_shipped_log_sea_diff'], label='Log Transformation and regular Differencing and Seasonal Differencing')
plt.legend(loc='best')
plt.title('Log Transformation and Regular Differencing and Seasonal Differencing')
plt.xlabel('Date')
plt.ylabel('SDiff(RDiff(Log(Box shipped)))')

plt.tight_layout()
plt.show()

df.plot(kind='line')
```

### OUTPUT:

<img width="563" height="416" alt="download" src="https://github.com/user-attachments/assets/a956d936-5daa-4171-bbab-310f27df3c7a" />



REGULAR DIFFERENCING:
<img width="578" height="138" alt="download" src="https://github.com/user-attachments/assets/9b70e88e-cdcd-44af-804b-f72ae65592f3" />


SEASONAL ADJUSTMENT:

<img width="560" height="138" alt="download" src="https://github.com/user-attachments/assets/e034b3e9-ab23-41b7-8384-e3b824736d82" />

LOG TRANSFORMATION:

<img width="560" height="138" alt="download" src="https://github.com/user-attachments/assets/a0643bc5-e01a-4bed-a6d9-028d1a798e13" />


### RESULT:
Thus we have created the python code for the conversion of non stationary to stationary data on international airline passenger
data.
