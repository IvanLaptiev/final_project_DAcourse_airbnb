# Hello and welcome to my final project for the Data Analyst Course at Hebrew University Business School!

## Table of Contents
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Project Steps](#project-steps)
- [Conclusion and Recommendations](#conclusion-and-recommendations)

## Project Overview
As final part of my Data Analyst Program that I took in the Hebrew University, we've been tasked to imagine ourselves a data analysts working for a potential investor who’s considering investing in short-term rental properties in a selected state in Europe. I got the city of Bordeaux. We had to analyze the Airbnb dataset and to provide insights that will guide investor's investment strategy.

As data analysts we were tasked to clean a large Airbnb dataset, to find a research question, to analyze the remaining data, and to summarize the results accordingly. While conducting the analysis, I have mostly used Excel, Python, Tableau.

## Data Sources 
The datsets we've been given by our mentors were taken off of an Airbnb website.
- Listings data (listings.csv): Detailed information about each listing, including various attributes of the listing (e.g., location, property type, price, availability, number of reviews) and the host (e.g., host ID, host listing count).
- Calendar data (calendar.csv): Information about the availability and price of each listing for different dates.

## Tools
- Excel - Data Researching and Data Analysis
- Python - Data Cleaning and Data Analysis
- Tableau - Creating Reports
- PowerPoint - Presentation

## Project Steps 
1. **Data Loading and Cleaning:** this step included importing the datasets and conducting appropriate data cleaning processes, such as handling missing values and converting data types. In addition, I had to prepare the data for next stages and keep only the relevant columns to enhance computational time.
2. **Exploratory Data Analysis (EDA):** this step included conducting a preliminary examination of the data to understand the distributions of variables, relationships between variables, and any potential outliers or anomalies.
- The EDA was performed for each of the dataset files separately
- Throughout the EDA journey, I stumbled upon several intriguing research questions. However, I singled out one that would undoubtedly captivate investors:
#### What characteristics will yield high revenue for AIRBNB properties in Bordeaux?
3. **Combining the Datasets:** to answer the research question and to create a combined dataset for more complex analyses, I had to merge the "listings.csv" and "calendar.csv" using the key column - 'listing_id'. (The datasets were merged seamlessly using the 'VLOOKUP' function)
4. **What is Revenue:** to address the research question, I formulated a calculation method for revenue based on the dataset. The following calculation was employed:
#### Annual Revenue = Daily Price * Yearly Occupancy

  - Note: No expences were taken into consideration!

5. **Analysis Assumptions:** prior and during the analysis, I've made a few hypotheses to provide a clear direction and framework for investigation and creating reports:
  - Used only data with Price and Occupancy (the only relevant data to answer the research question)
  - Categories with a correlation of revenue under 2% were excluded (see below - Data Analysis Step)
  - Filtered Property Type to a minimum of 29 units, which is 1% of properties (to make the "Property Types - AVG Revenue" bubble chart more readable - see presentation)
  - Focus on positive correlation between categorical variables (in order to drop irrelevant categories that don't have a meaningful impact on revenue) 

6. **Data Analysis:** the whole data analysis process you can check out in the attached ipynb files
At first, I created a new Excel file (listing7.4), which contains only the relevant data rows (see step #5) and a few additional columns for my further analysis:
- F -> # of days in which the listing is occupied during the year
- T = 365 - F -> # of days in which the listing is not in demand 
- Revenue = F * Price -> revenue this listing brings to the host in a year

Subsequently, I sought to identify the strongest correlation between numerical categories only and revenue. A decision I initially made in error, overlooking the direct impact of Price and Occupancy on Revenue, as well as leaving a lot of useful information behind: (see 'revenue_corr_to_other.ipynb' notebook)
```python
import io
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

from google.colab import files
upload = files.upload()

# Load dataset
df = pd.read_excel(io.BytesIO(upload['listing7.4.xlsx']))

# Drop the non-numeric columns from the DataFrame
numeric_df_without_id = df.drop(columns=['id','T','longitude','latitude']).select_dtypes(include=[np.number])

# Calculate the correlation matrix for the remaining numeric columns
correlation_matrix = numeric_df_without_id.corr() 

plt.figure(figsize=(10, 8))
barplot = sns.barplot(x=correlation_with_revenue.values, y=correlation_with_revenue.index, palette='coolwarm')
plt.title('Correlation of Revenue with Other Properties, Excluding ID')
plt.xlabel('Correlation Coefficient')
plt.ylabel('Properties')
for index, value in enumerate(correlation_with_revenue.values):
    plt.text(value, index, f'{value:.2f}')
plt.show()
```

Then, I've decided to look for correlation between revenue and all of the remaining data that might interest me, dropping some of the columns that I was pretty sure won't make any difference in my research: (see 'data_anlysis_step.ipynb' notebook)
```python
import io
import pandas as pd
import numpy as np
import matplotlib.pylab as plt
import seaborn as sns
import missingno as msno
from datetime import datetime

from google.colab import files
upload = files.upload()

df = pd.read_excel(io.BytesIO(upload['listing7.4.xlsx']))

numeric_df = df.drop(columns=['id', 'T', 'longitude', 'latitude', 'price', 'F', 'host_since', 'accommodates',
    'minimum_nights', 'minimum_minimum_nights', 'maximum_minimum_nights',
    'minimum_nights_avg_ntm', 'number_of_reviews_ltm', 'number_of_reviews_l30d',
    'review_scores_cleanliness', 'review_scores_checkin', 'review_scores_value',
    'calculated_host_listings_count_entire_homes']).select_dtypes(include=[np.number])

correlation_matrix = numeric_df.corr()

correlation_with_revenue = correlation_matrix['revenue'].drop('revenue').sort_values(ascending=False)

#Creating a plot to see the correlation of revenue with other properties' categories 
# plt.figure(figsize=(10, 8))
# sns.barplot(x=correlation_with_revenue.values, y=correlation_with_revenue.index, palette='coolwarm')
plt.figure(figsize=(10, 8))
barplot = sns.barplot(x=correlation_with_revenue.values, y=correlation_with_revenue.index, palette='coolwarm')
plt.title('Correlation of Revenue with Other Properties Categories')
plt.xlabel('Correlation Coefficient')
plt.ylabel('Properties')
for index, value in enumerate(correlation_with_revenue.values):
    plt.text(value, index, f'{value:.4f}')
plt.show()

#Creating a heatmap of correlations

plt.figure(figsize=(12, 10))
sns.heatmap(correlation_matrix, annot=True, fmt=".2f", cmap='coolwarm', linewidths=.5)
plt.title('Heatmap of Correlation Matrix')
plt.show()
```
After creating a heatmap, I decided to focus on three main categories that, in my opinion, directrly affect Revenue: "Property Type", "Room Type", and "Superhost Status", filtering out the correlation between -0.02 and 0.02, to get a clearer results. (same 'data_anlysis_step.ipynb' notebook)

```python

import matplotlib.pyplot as plt
import seaborn as sns

# Filter out correlations between -0.02 and 0.02
filtered_correlation_with_revenue = correlation_with_revenue[(correlation_with_revenue > 0.02) | (correlation_with_revenue < -0.02)]

# Create a bar plot with the filtered correlations
plt.figure(figsize=(12, 10))
sns.barplot(x=filtered_correlation_with_revenue.values, y=filtered_correlation_with_revenue.index, palette = 'tab10')

# Set the title and labels of the plot
plt.title('Correlation of Revenue with Property Type, Room Type, and Superhost Status (Filtered)')
plt.xlabel('Correlation Coefficient')
plt.ylabel('Variables')

# Display the plot
plt.tight_layout()
plt.show()

#Creating a plot to see the correlation of Revenue with Property Type, Room Type, and Superhost Status

plt.figure(figsize=(15, 10))
barplot = sns.barplot(x=filtered_correlation_with_revenue.values, y=filtered_correlation_with_revenue.index, palette = 'tab10')

# Set the title and labels of the plot
plt.title('Correlation of Revenue with Property Type, Room Type, and Superhost Status (Filtered)')
plt.xlabel('Correlation Coefficient')
plt.ylabel('Variables')

# Add labels to each bar
for index, value in enumerate(filtered_correlation_with_revenue.values):
    # The parameters (x, y) determine the label's position
    # x is set to the value (correlation coefficient) plus a small offset to place it outside the bar
    # y is set to the index, which corresponds to the bar's position on the y-axis
    # The label itself is the value formatted to 4 decimal places
    plt.text(value, index, f'{value:.4f}', color='black', va='center')

# Display the plot
plt.tight_layout()
plt.show()
```

Analyzing the results allows us to evaluate each category individually and identify the top option in each. The highest correlation between Revenue and **Property Type** is found in "Private room in townhouse," indicating this property type generates the most revenue compared to others.

In the **Room Type** category, it is surprising to find that "Private Room" shows the highest correlation with revenue. This suggests that investors might consider purchasing a property and listing it as private rooms to maximize revenue.

Additionally, it appears that **Superhost Status** does not significantly impact higher revenue.

Next, once again I went on to look for a correlation of revenue with other properties' categories, but this time - all categories including non-numeric. As before, I'll attach here part of the code. (for the whole coding process - check out "final_corr.ipynb")

```python
# Define your numerical and categorical columns
numerical_cols = ['beds', 'latitude', 'longitude']
categorical_cols = ['neighbourhood', 'neighbourhood_cleansed', 'neighbourhood_group_cleansed', 'property_type', 'room_type']

#Creating a "heatmap" to see the highest correlation with revenue 

# Sort the correlations by their absolute values but keep the original sign
top_categorical_corrs = categorical_corrs.abs().sort_values(ascending=False).head(N)

# Use the original Series to keep the sign of the correlations
final_corrs = categorical_corrs[top_categorical_corrs.index]

# Convert to DataFrame for seaborn heatmap compatibility, this time directly
final_corrs_df = final_corrs.to_frame()  # Converts the filtered Series to a DataFrame
final_corrs_df.columns = ['Correlation with Revenue']  # Rename column for clarity

# Plotting the heatmap
plt.figure(figsize=(10, 8))  # Adjust size as necessary
sns.heatmap(final_corrs_df, annot=True, cmap='coolwarm', cbar=True, fmt=".2f", yticklabels=True)
plt.title('Top Categorical Correlations with Revenue')
plt.xticks(rotation=45, ha='right')  # Improve readability of x labels
plt.show()

#Creating a plot to show me the Top 10 Positive Correlations with Revenue

# Calculate the correlation of all variables with 'revenue'
correlation_with_revenue = combined_df.corr()['revenue'].sort_values(ascending=False)

# Exclude 'revenue' from itself to avoid redundancy
correlation_with_revenue = correlation_with_revenue.drop('revenue')

# Filter to keep only positive correlations
positive_correlations = correlation_with_revenue[correlation_with_revenue > 0]

# Sort the positive correlations in descending order to get the top 10
top_10_positive_correlations = positive_correlations.sort_values(ascending=False).head(10)

# Visualizing the top 10 positive correlations with 'revenue'
plt.figure(figsize=(8, 6))  # Adjust the figure size as needed
sns.heatmap(top_10_positive_correlations.to_frame(), annot=True, cmap='coolwarm', cbar=True, fmt=".2f")
plt.title('Top 10 Positive Correlations with Revenue')
plt.show()

```
Please feel free to check out the whole coding process in the attached files, as I noticed earlier. I am open for any suggestions at what I could have done better/ easier or smarter!

7. **Conclusion and Recommendations:**
- I found that **Saint-Médard-en-Jalles** is the neighborhood which, on average, returns the highest revenue with a price range of $18-$500 per night and an average price of $101 per night. Average annual revenue in this neighborhood = $27,590 and average occupancy = 75.78%. 
- Highest revenue room type, surprisingly, appeared to be **"Private Room"** with a price range of $15-$1600 per night (!) and an average price of $108.5 per night. So, if I was an investor looking for a property in Bordeaux, I would definitely consider buying a property and flipping it into a separate private room listings,
- And last but not least, **"Private room in townhouse"** is the highest revenue property type, which, on average, brings you $122.27 per night.

I have also attached a presentation which also includes some of the graphs that I made using Tableau. Feel free to check it out as well. 
Although I am quite proud of what I've achieved during my first full project as a data analyst, there is a looong way to make it even better and learn some new stuff for the upcoming projects and opportunities.

Thanks for your time!

Ivan



