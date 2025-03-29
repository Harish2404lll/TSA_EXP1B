# Ex.No: 1B   CONVERSION OF NON STATIONARY TO STATIONARY DATA
### NAME :HARISH G
### REGISTER NO:212222243001
# Date: 

### AIM:
To perform regular differncing,seasonal adjustment and log transformatio on electric production dataset.
### ALGORITHM:
1. Import the required packages like pandas and numpy
2. Read the data using the pandas
3. Apply regular differncing,seasonal adjustment,log transformation.
4. Plot the data according to need, before and after regular differncing,seasonal adjustment,log transformation.
5. Display the overall results.

### PROGRAM:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.seasonal import seasonal_decompose

# Load the dataset with error handling
file_path = "/content/AirPassengers (1).csv"
data = pd.read_csv(file_path)

# Display column names to verify correctness
print("Column Names in Dataset:", data.columns)

# Check for correct date column name and rename if needed
if 'Month' not in data.columns:
    date_column = data.columns[0]  # Assume first column is date if 'Month' is missing
    data.rename(columns={date_column: 'Month'}, inplace=True)

# Convert the 'Month' column to datetime format
data['Month'] = pd.to_datetime(data['Month'], errors='coerce')

# Drop rows with invalid dates
data = data.dropna(subset=['Month'])

# Set the 'Month' column as the index
data.set_index('Month', inplace=True)

# Check for correct data column name and rename if needed
if '#Passengers' not in data.columns:
    value_column = data.columns[0]  # Assume first column is value if '#Passengers' is missing
    data.rename(columns={value_column: '#Passengers'}, inplace=True)

# Convert the '#Passengers' column to numeric
data['#Passengers'] = pd.to_numeric(data['#Passengers'], errors='coerce')

# Drop any rows with NaN values that may have resulted from the conversion
data = data.dropna()

# Perform time series decomposition
result = seasonal_decompose(data['#Passengers'], model='additive', period=12)
data['passengers_sea_diff'] = result.resid

data['passengers_diff'] = data['#Passengers'] - data['#Passengers'].shift(1)
data['passengers_log'] = np.log(data['#Passengers'])
data['passengers_log_diff'] = data['passengers_log'] - data['passengers_log'].shift(1)

result = seasonal_decompose(data['passengers_log_diff'].dropna(), model='additive', period=12)
data['passengers_log_seasonal_diff'] = result.resid

# Function to save each plot separately
def save_plot(data, column, title, ylabel, filename):
    plt.figure(figsize=(12, 6))
    plt.plot(data[column], label=title)
    plt.legend(loc='best')
    plt.title(title)
    plt.xlabel('Year')
    plt.ylabel(ylabel)
    plt.savefig(filename)
    plt.close()

# Save each plot as an image
save_plot(data, '#Passengers', 'Original Data', 'No of passengers', 'original_data.png')
save_plot(data, 'passengers_diff', 'Regular Differencing', 'Differenced No of passengers', 'regular_diff.png')
save_plot(data, 'passengers_sea_diff', 'Seasonal Adjustment', 'Seasonally Adjusted No of passengers', 'seasonal_adjustment.png')
save_plot(data, 'passengers_log', 'Log Transformation', 'Log (No of passengers)', 'log_transformation.png')
save_plot(data, 'passengers_log_diff', 'Log Transformation and Regular Differencing', 'RDiff (Log (No of passengers))', 'log_reg_diff.png')
save_plot(data, 'passengers_log_seasonal_diff', 'Log Transformation and Regular Differencing and Seasonal Differencing', 'SDiff (RDiff (Log (No of passengers)))', 'log_reg_seasonal_diff.png')

data.plot(kind='line')
plt.savefig('line_plot.png')
plt.close()
```



### OUTPUT:

![image](https://github.com/user-attachments/assets/a5623a00-7255-4a58-87fe-7192744a7e02)


REGULAR DIFFERENCING:

![image](https://github.com/user-attachments/assets/e64dd9b4-7eb3-4092-a261-82fb74541b4b)



SEASONAL ADJUSTMENT:

![image](https://github.com/user-attachments/assets/af40e3ef-1f7a-4284-a0f6-21da020d3aa4)



LOG TRANSFORMATION:

![image](https://github.com/user-attachments/assets/0d0509db-85b3-40df-b848-032e48b17701)




### RESULT:
Thus The python code has been created for the conversion of non stationary to stationary data on electric production dataset.


