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
4. **What is Revenue?:** to address the research question, I formulated a calculation method for revenue based on the dataset. The following calculation was employed:
#### Annual Revenue = Daily Price * Yearly Occupancy

  - Note: No expences were taken into consideration!

5. **Analysis Assumptions:** prior and during the analysis, I've made a few hypotheses to provide a clear direction and framework for investigation and creating reports:
  - Used only data with Price and Occupancy (the only relevant data to answer the research question)
  - Categories with a correlation of revenue under 2% were excluded.(see below - Data Analysis Step)
  - Filtered Property Type to a minimum of 29 units, which is 1% of properties (to make the "Property Types - AVG Revenue" bubble chart more readable - see presentation)
  - Focus on positive correlation between categorical variables (in order to drop irrelevant categories that don't have a meaningful impact on revenue) 

6. **Data Analysis:** 
7. 



