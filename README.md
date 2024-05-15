# Hello and welcome to my first project as Data Analyst!
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

Subsequently, I sought to identify the strongest correlation between numerical categories only and revenue. A decision I initially made in error, overlooking the direct impact of Price and Occupancy on Revenue, as well as leaving a lot of useful information behind: (see 'revenue_corr_to_other.ipynb')
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

I then decided to look for correlation between revenue and all of the remaining data that might interest me, dropping some of the columns that I was pretty sure won't make any difference in my research: (see data_anlysis_step.ipynb)
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

import seaborn as sns
import matplotlib.pyplot as plt

# Assuming correlation_matrix is already calculated as shown in your snippet
plt.figure(figsize=(12, 10))
sns.heatmap(correlation_matrix, annot=True, fmt=".2f", cmap='coolwarm', linewidths=.5)
plt.title('Heatmap of Correlation Matrix')
plt.show()

# Assuming 'filtered_correlation_with_revenue' contains the filtered correlations
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
7. 



