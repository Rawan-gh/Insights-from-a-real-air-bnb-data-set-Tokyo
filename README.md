# Insights-from-a-real-air-bnb-data-set-Tokyo.
I have done all the calculations based on data from Tokyo.
![image](https://github.com/user-attachments/assets/11324a34-abd4-4608-814c-ecfc6396a261)

# Importing the Compressed CSV for Tokyo with Pandas

```python
import pandas as pd

# Reading a compressed CSV file for Tokyo
calendar = pd.read_csv('calendar.csv.gz')
```

# Questions that you might want to address about the given data
Let`s dive into exciting insights!

## 1 Want to know the number of available and unavailable rooms
```
calendar.available.value_counts()

```

<img src="https://github.com/user-attachments/assets/f6a52d03-eb3e-48c9-9408-7028703ddcec" alt="Value Counts Output" width="400"/>

## 2 Purpose: Calculates the percentage of available (t) and unavailable (f) dates.
Explanation: value_counts(normalize=True) gives proportions. Multiplying by 100 converts the proportions into percentages

```
availability_percentage = calendar['available'].value_counts(normalize=True) * 100
availability_percentage
```

<img src="https://github.com/user-attachments/assets/592de42d-2969-4ad4-bb87-294d9c29d190" alt="Value Counts Output" width="600"/>

## 3 Let`s Count the busiest day! 🚩
We will be counting the most unavailable days (given by f)

```
busiest_dates = calendar[calendar['available'] == 'f']['date'].value_counts()
print("Busiest Dates:")
print(busiest_dates.head())
```

<img src="https://github.com/user-attachments/assets/344e73bb-1687-4875-8c99-680e62b8835a" alt="Value Counts Output" width="600"/>

## 4 Plot a bar graph to show availability percentage
```
import matplotlib.pyplot as plt
availability_percentage.plot(kind='bar', color=['green', 'red'])
plt.title('Availability Percentages')
plt.ylabel('Percentage')
plt.xlabel('Available (t/f)')
plt.show()
```

<img src="https://github.com/user-attachments/assets/2001ba10-899e-4b94-9bc1-d922d8fef0d8" alt="Value Counts Output" width="600"/>

## 5 Plot the busiest day
```
busiest_dates.head(10).plot(kind='bar', color='orange')
plt.title('Top 10 Busiest Dates')
plt.ylabel('Number of Listings Unavailable')
plt.xlabel('Date')
plt.show()
```
<img src="https://github.com/user-attachments/assets/b5c5870f-f0ec-4ddb-99a9-382ff2b4abd6" alt="Value Counts Output" width="600"/>

# 📄 Download listings dataset of Tokyo from
https://data.insideairbnb.com/japan/kantō/tokyo/2024-09-27/visualisations/listings.csv

```
import pandas as pd
listings = pd.read_csv('listings.csv')
```

```
listings.columns
```
<img src="https://github.com/user-attachments/assets/a8ab2248-e73c-483e-ad9e-688118916c6c" alt="Value Counts Output" width="600"/>

# ✅ Tokyo type and price is given seperately
```
import matplotlib.pyplot as plt
price_by_room = listings.groupby('room_type')['price'].mean()
print(price_by_room)

# Plot price by room type
price_by_room.plot(kind='bar', color='saddlebrown')
plt.title('Average Price by Room Type')
plt.ylabel('Average Price')
plt.xlabel('Room Type')

plt.show()
```
<img src="https://github.com/user-attachments/assets/edb6780e-1048-428e-832a-8e2f7f4864b3" alt="Value Counts Output" width="600"/>

# ✅ Which are the top 10 neighborhoods with the most listings?

