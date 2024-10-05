# Melocoton

## Overview
Melocoton, is a young smartphone retailer company.
The company is very dynamic, and in a space of a few year, business has been going pretty well, 3 shops have been launched in 3 different city.. 
Facing such success, Sophia CEO and founder of the company, decided to face a new challenge. 
Launch its marketplace on Amazon and conquer the online market.
In order to do so, Sophia, the young entrepreneur, decided to plan a meeting with marketting and supply. 
The purpose of this meeting is to understand how the market on amazon works.
She wants to keep the same dynamic as the physical shops, and to do so it's important to know the rules. 
At the end of this meeting, I have been task to analyse amazon sales in order to identify trends amongs de sales.
The compagny inteds to rely on this analyse to make up  sales strategy. 

The supply team gives you a selection of product that yoy will use for the analysis. 
[Here is the link for the database used for this project.](https://www.kaggle.com/datasets/shreyasur965/phone-search-dataset)

## Objective 1 - Cleanning the dataset. 
Organizing the data and make sure the dataset, and everypiece of data that we are going to use in the analyse is clean. 

## Objective 2 - Exploring the data. 

After importing data to the tables, performaing a Exploratory Data Analysis with SQL queries and to answer question to create insights about the data. This will include the following analysis:

Price analysis
Review Analysis
label analysis. 

# Process 
For this analysis, I will be using the datasets:



# Data cleanning

First off, let's start with the dataset and its integrity. 

The data is form Kaggle. There are few information about its authors, nor its provenance. 
We only jnow that the data was collected using the "Real-Time Amazon Data" API from RapidAPI.
The dataset was generated using an automated query to retrieve real-time data on phones available on Amazon. For each query, phone-related product data (including prices, ratings, and availability) was fetched using the API. Data collection was done iteratively over 100 pages to cover a broad range of phones, ensuring the inclusion of various price ranges, product types, and customer reviews. The process utilized the following query parameters: Query: "Phone" (ensures data is filtered for phone products) Page: Each iteration requested a new page to retrieve different sets of products. Country: US (data was specific to the US market). Sorting: Data was sorted by relevance, ensuring a diverse selection of phone products.

It is important to underlight that there are few information about the period of time in which this data is valid, nor hase been taken. 
Fotr the sake of the analysis, we're gonna consider as it was from the prior month as we have sales data from the past month. 




