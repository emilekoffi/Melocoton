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

### Objective 1 - Cleanning the dataset. 
Organizing the data and make sure the dataset, and everypiece of data that we are going to use in the analyse is clean. 

### Objective 2 - Exploring the data. 

After importing data to the tables, performaing a Exploratory Data Analysis with SQL queries and to answer question to create insights about the data. This will include the following analysis:

Price analysis
Review Analysis
Label analysis. 

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
    - R  : No empty cells
    - S  : No empty cells
   

Now that I checked for duplicates, blank spaces,and empty cells, I'll proceed with making sure that the value have the best format to pêrfrom an alanysis later on. 
I'll start with the all the price format. 
As the the data base is extract form the USA amazon market, we can assume that all these datad are indeed in USD and I can then proceed to delete the dollar sign in colomn C, D, and K. 
To delete de dollar sign : create a new colomn in between colomn C ans D. Select all colomn C except the title. then go to data -> split text -> use "$" as the customized separator. the new column now shows the price without the dollar sign. 
I deleted the empty column and paste the title in the new column. 
I repeat for the operation on D and L. 

As the only currecny used is the USD, we can also delete E. 

I relaize that the column P "sales_columes" have data that can be difficult to use in SQL. 
I want to only keep nummers in this column. 
To do so i'll split the text agin, using "+" as a seperator. 
I now have for one part the number, and in the other part the text "bought in past month". 
I delete this second column as I won't use it. 
In my colomn i now have the numbers, only thing is that the thousands are written as "K", 2000 = 2K. 
I remplace all the K with 000.  To do so : edit -> search and replace -> search K, replace by 000, search'!P:P, replace all. 
I now change the title to "approx_past_month_sales_volume" as the value were not exact (4K+ for exemple). 

I'll proceed for the same type of operation vfor the delivery column. 
I'll split it in 2 with delivery as the separator, and replace FREE by 0.
There was only one item without free delivery.  I manually deleted the dollar sign. 

I manually delete the dollar sign in the 2 value of the unit_price column.

I now have a clean dataset ready for use. 
It has 315 lines

# Exploring the data. 

Now I export the file in a CSV and then import it in Bigquerry


I start with checking once again the number of distinct item. 

```sql 
SELECT 
DISTINCT asin AS unique_asin
 FROM `phone_search.phone_search_cleaned`
```

I now have 311 lines, meaning that 4 items were in double


### A few questions. 
Before going deeper in th analysis there's a few questrions that needs to be asked to get a better understanding of the project. 
Marketing team provided me the answers.

**What does Best Seller Amazon means ?** 
It is represented by the orange ribbon emblem in the upper-left corner of a product page and is given to top sellers. Customers may make an informed purchase choice by knowing which products have a higher product rating in terms of sales thanks to this emblem.

**What does Amazon's Choice means ?** 
Amazon's Choice makes it easy to discover products that other customers frequently choose for similar purchasing needs. Featured products like Amazon's Choice are rated highly by our customers, are available for immediate shipment and are well-priced. 

**How many sales are enough to be considers thats a products sells well ?** 
The targeted sales volumes is at least 500 sales monthly. 

**What are the reviews standards ? What is the thereshold ?** 
Reviews on Amazon are classified with stars, 5 stars being the most positive review possible, 0 the least.
More than 4,2 stars is considered an execvellent review. 
In beetwen 4.2 and 3.5 is a good review
From 3,5 to 2.5 is medium 
Below is considered as a bad review.
Melocoton is highly interested in products whith at least 3.5 stats.

**How many reviews is enough to be significative ?** 
A minimum of 1,000 reviews is required for a rating to be considered significant. 

## Price analysis. 

Does the price of the products plays a role on the sale ? 

```sql
SELECT 
DISTINCT asin AS unique_asin,
product_price,
approx_past_month_sales_volume,
 FROM `phone_search_cleaned`
 ORDER BY approx_past_month_sales_volume DESC
```

For now it's complicated to establish a link between the price and the sells, as the first 10 products goes from 31.99 USD to 225.65 USD. 

But does the dicounts attract more sales ? 

```sql
SELECT 
DISTINCT asin AS unique_asin,
product_price AS with_discount,
product_original_price AS without_discount,
((product_original_price - product_price) / product_original_price)*100 AS discount,
approx_past_month_sales_volume,
FROM `my-project-coursera-certif-1.phone_search.phone_search_cleaned`
WHERE product_original_price IS NOT NULL
ORDER BY discount DESC
```





 









