**EXP 3 - Delhi Air Quality Analysis**

**Aim**


To compare air quality parameters in Delhi across different stations and analyze the relationship between pollutants (e.g., PM2.5 and NO₂) using scatter plots and correlation analysis.


**Procedure / Algorithm**

1)Load the dataset using pandas.

2)Preprocess the data:

3)Convert the date column (period.datetimeFrom.utc) to datetime format.

4)Drop missing or invalid values.

5)Pivot the dataset so each pollutant (parameter) becomes a separate column.

6)Plot scatter plot between PM2.5 and NO₂ to study their relationship.

7)Plot correlation heatmap between all pollutants to identify relationships.

8)Interpret the results — identify which pollutants are correlated and which stations are most polluted.


**Program**

**Name : yokesh v **

**Reg No:212225040501**
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("del-sirifort-cpcb-2024-25 (1).csv")

print("Shape:", df.shape)
print("\nColumns:")
print(df.columns)

df["Timestamp"] = pd.to_datetime(df["Timestamp"], errors="coerce")

pollutants = [
    "PM2.5 (µg/m³)",
    "PM10 (µg/m³)",
    "NO2 (µg/m³)",
    "Ozone (µg/m³)"
]

for col in pollutants:
    df[col] = pd.to_numeric(df[col], errors="coerce")

print("\nMissing Values:")
print(df[pollutants].isnull().sum())

data = df[pollutants].dropna()

print("\nStatistical Summary:")
print(data.describe())

print("\nMean:")
print(data.mean())

print("\nMedian:")
print(data.median())

print("\nStandard Deviation:")
print(data.std())

print("\nVariance:")
print(data.var())

data.hist(figsize=(12, 8), bins=30)
plt.suptitle("Distribution of Air Quality Parameters")
plt.tight_layout()
plt.show()

plt.figure(figsize=(10, 6))
sns.boxplot(data=data)
plt.title("Boxplot of Air Quality Parameters")
plt.xticks(rotation=20)
plt.show()

print("\nOutlier Count:")
for col in pollutants:
    Q1 = data[col].quantile(0.25)
    Q3 = data[col].quantile(0.75)
    IQR = Q3 - Q1

    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR

    outliers = data[(data[col] < lower) | (data[col] > upper)]

    print(col, ":", len(outliers))

scaled_data = (data - data.min()) / (data.max() - data.min())

print("\nMin-Max Scaled Data:")
print(scaled_data.head())

standardized_data = (data - data.mean()) / data.std()

print("\nStandardized Data:")
print(standardized_data.head())

correlation = data.corr()

print("\nCorrelation Matrix:")
print(correlation)

plt.figure(figsize=(8, 6))
sns.heatmap(correlation, annot=True, cmap="coolwarm", fmt=".2f")
plt.title("Correlation Heatmap of Air Pollutants")
plt.show()

plt.figure(figsize=(8, 6))
plt.scatter(data["PM2.5 (µg/m³)"], data["NO2 (µg/m³)"], alpha=0.4)
plt.xlabel("PM2.5 (µg/m³)")
plt.ylabel("NO2 (µg/m³)")
plt.title("PM2.5 vs NO2")
plt.show()

def gini_coefficient(values):
    values = np.sort(values)
    n = len(values)
    return (
        2 * np.sum((np.arange(1, n + 1)) * values)
        - (n + 1) * np.sum(values)
    ) / (n * np.sum(values))

print("\nGini Coefficient:")
for col in pollutants:
    print(col, ":", round(gini_coefficient(data[col]), 3))
```

**Output**

<img width="817" height="792" alt="image" src="https://github.com/user-attachments/assets/ef802ef7-dc9f-4a60-a167-f7a8da28da12" />

<img width="537" height="436" alt="image" src="https://github.com/user-attachments/assets/f1d89eb7-3f64-430e-b6ae-03b25aee85e9" />

<img width="1183" height="787" alt="image" src="https://github.com/user-attachments/assets/8c269d53-c921-4062-bd25-2d2406a4abac" />

<img width="1048" height="692" alt="image" src="https://github.com/user-attachments/assets/9e2b33cc-e214-4bd6-96ff-f7b513c822ec" />

<img width="741" height="610" alt="image" src="https://github.com/user-attachments/assets/62380707-e13b-45ad-b63a-39ae0f1b0331" />

<img width="780" height="656" alt="image" src="https://github.com/user-attachments/assets/8c0dcbb3-de52-4873-a4ef-2f2e603ff01a" />

<img width="870" height="676" alt="image" src="https://github.com/user-attachments/assets/35794f67-6472-4b95-9f60-10b04794ae95" />

<img width="308" height="117" alt="image" src="https://github.com/user-attachments/assets/3b39fe40-96d3-4282-8363-a3c7207d95ec" />


**Result**

The dataset was successfully loaded and processed to extract pollutant-wise and station-wise air quality data for Delhi.


