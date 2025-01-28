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

## ✅ Tokyo type and price is given seperately
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

## ✅ Which are the top 10 neighborhoods with the most listings?
```
neighborhood_counts = listings['neighbourhood'].value_counts().head(10)
print("Top 10 Neighborhoods by Listings:")
print(neighborhood_counts)

# Plot neighborhoods with most listings
neighborhood_counts.plot(kind='bar', color='lightcoral')
plt.title('Top 10 Neighborhoods by Listings')
plt.ylabel('Number of Listings')
plt.xlabel('Neighborhood')
plt.xticks(rotation=90)
plt.show()
```
<img src="https://github.com/user-attachments/assets/34664f07-1459-495e-bc01-a35b29db99c6" alt="Value Counts Output" width="600"/>

## ✅ Geographical Distribution of Listings (Price Colored)

```
import matplotlib.pyplot as plt
import seaborn as sns
```
```
plt.figure(figsize=(10, 6))
sns.scatterplot(data=listings, x='longitude', y='latitude', hue='price', palette='viridis', size='price', sizes=(10, 200))
plt.title('Geographical Distribution of Listings (Price Colored)')
plt.xlabel('Longitude')
plt.ylabel('Latitude')
plt.show()
```
<img src="https://github.com/user-attachments/assets/354054b1-4ddc-4269-81f9-38d9838dda27" alt="Value Counts Output" width="600"/>

## What is the most expensive neighborhood in Tokyo
```
most_expensive_neighborhoods = listings.groupby('neighbourhood')['price'].mean().sort_values(ascending=False).head(10)
print("Most Expensive Neighborhoods:")
print(most_expensive_neighborhoods)

# Plot the most expensive neighborhoods
most_expensive_neighborhoods.plot(kind='bar', color='gold')
plt.title('Top 10 Most Expensive Neighborhoods')
plt.ylabel('Average Price')
plt.xlabel('Neighborhood')
plt.xticks(rotation=90)
plt.show()
```
<img src="https://github.com/user-attachments/assets/a0559f36-0650-45f9-967c-f90168e79576" alt="Value Counts Output" width="600"/>

## What is the distribution of room types across Tokyo
```
# Count the number of each room type
room_type_counts = listings['room_type'].value_counts()
print("Room Type Distribution:")
print(room_type_counts)

# Plot room type distribution as a pie chart
room_type_counts.plot(kind='pie', autopct='%1.1f%%', startangle=90, figsize=(8, 8), colors=['skyblue', 'lightgreen', 'orange', 'pink'])
plt.title('Distribution of Room Types in Tokyo')
plt.ylabel('')  # Hides the y-label
plt.show()
```
<img src="https://github.com/user-attachments/assets/56043893-5018-4404-8eb5-1aea5f283000" alt="Value Counts Output" width="600"/>

## Let us see the listings on a real map
* Hotter Areas (Red/Yellow): High Density: The areas that appear in red or yellow (the "hot" colors) indicate higher density or concentration of listings. This means there are more listings in these areas. Popular Locations: These regions might be more popular or in high demand. It could be near tourist attractions, popular neighborhoods, or central areas in Tokyo where people tend to stay more often. Colder Areas (Green/Blue):

* Low Density: Areas with blue or green (the "cold" colors) indicate a lower concentration of listings. These regions have fewer listings available. Less Popular Locations: These areas might be less popular or further from key attractions. If you're looking at pricing or other factors, lower density could imply less competition in these regions, which might indicate more affordable areas or less tourist traffic. Density Patterns:

* Clustered Areas: If you notice clusters of heatmap intensity, they represent hotspots. These might correspond to high-traffic areas like resorts, beaches, or urban centers. Spread-Out Listings: If the heatmap shows a more uniform distribution, it could suggest that listings are more evenly spread across the region, which may reflect a more balanced demand for rentals across different areas of Tokyo.

<img src="https://github.com/user-attachments/assets/b0301a59-ce84-4eda-a15f-c0f3be96b153" alt="Value Counts Output" width="600"/>


