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
For this analysis, I will be using the datasets: phone_search



# Data cleanning

First off, let's start with the dataset and its integrity. 

The data is form Kaggle. There are few information about its authors, nor its provenance. 
We only jnow that the data was collected using the "Real-Time Amazon Data" API from RapidAPI.
The dataset was generated using an automated query to retrieve real-time data on phones available on Amazon. For each query, phone-related product data (including prices, ratings, and availability) was fetched using the API. Data collection was done iteratively over 100 pages to cover a broad range of phones, ensuring the inclusion of various price ranges, product types, and customer reviews. The process utilized the following query parameters: Query: "Phone" (ensures data is filtered for phone products) Page: Each iteration requested a new page to retrieve different sets of products. Country: US (data was specific to the US market). Sorting: Data was sorted by relevance, ensuring a diverse selection of phone products.

It is important to underlight that there are few information about the period of time in which this data is valid, nor hase been taken. 
Fotr the sake of the analysis, we're gonna consider as it was from the prior month as we have sales data from the past month. 

**Now the cleaning**

The cleaning is done in GoogleSheet. 

  - rermoving duplicates : select all rows; data ; cleaning data ; remove duplicates.
21 rows eliminated, 320 rows left. 
  - removing blank spaces : selact all rows; data; claening data ; remove blank space.
none found
  - Looking for empty cells : her we are gonna look for all colomns except for B; D ; H; I; T; O;  U; V
In order to underlight the empty cells, we are gonna use conditionnal formartting.
On all these colomns A1:A320, C1:C320, K1:K320, L1:L320, N1:N320, Q1:Q320, R1:R320, S1:S320, E1:E320, F1:F320, G1:G320, J1:J320, M1:M320, P1:P320
the rule is to show the cell in red if it's empty
    - A : No empty cells
    - C : 4 empty cells B0CMDLJR6K, B07ZHPCJW3, B07Z6Q9NCZ, B09R6FJWWS. As I don't really have information about when the data has been extracted, instead of using the        current price shown on the website, I delete the lines.
    - E : No empty cells
    - F : 3 empty cellsB0DFB9SNBR, B0D79Z15XR, B0DCQX683H. These cells are empty beccause the have never been rated as show the colomns G. The ceels value cannot be 0 as it will be incorrect. I'll put "/" instead.
    - G : No empty cells
    - J : No empty cells
    - K : No empty cells
    - L : No empty cells
    - M : No empty cells
    - N : No empty cells
    - P  : No empty cells
    - Q : 17 empty cells. As I filter the colomns, I notice that there are other values that don't indicate the sales volume on the past month such as : "List:", "More Buying Choices", "Typical:" , "Typical price:" ,Shop products from small business brands sold in Amazon’s store. Discover more about the small businesses partnering with Amazon and Amazon’s commitment to empowering them.". 26 in total
After verifying th elink for each product, we realize that, there a no values, or not the right value has been imported because amazon doesn't show the past month sales volume if there are not equal or higher to 50.
As those prodcut has been sold in beetween 0 and 49 times last month, we are going to use "25+ bought in past month" as a new value.
25 as it is the mean value.
    -R  : No empty cells
    -S  : No empty cells










