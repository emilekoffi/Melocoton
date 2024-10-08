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
FROM `phone_search.phone_search_cleaned` 
 ORDER BY approx_past_month_sales_volume DESC
```

For now it's complicated to establish a link between the price and the sells, as the first 10 products goes from 31.99 USD to 225.65 USD. 

### But does dicounts attract more sales ? 

```sql
SELECT 
DISTINCT asin AS unique_asin,
product_price AS with_discount,
product_original_price AS without_discount,
((product_original_price - product_price) / product_original_price)*100 AS discount,
approx_past_month_sales_volume,
FROM `phone_search.phone_search_cleaned`
WHERE product_original_price IS NOT NULL
ORDER BY discount DESC
```

The results show that the products with the highest dicount are not the ones that are most sold. 
But this is not enough to come to the conclusion that discount products are more sold. 


To dow so, I'm going to compare the average sales of dicounted products and non discouted product. 
```sql
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_original_price IS NOT NULL
```

Last month, the products with a discount sold an average 540 units. 

```sql
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_original_price IS NULL
```

The products without discount sold an average 350. 

On this base, product with discount sell more units than the product without discount but;, this doesn't mean that the discounted product are less expensive than the product without discount. 

**I am now going to compare the average price of products with discount with the average sales af of product without discounts,**
```sql
SELECT 
ROUND (AVG(product_price),2) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_original_price IS NOT NULL
```

Discounted products cost an acerage $183.3 

```sql
SELECT 
ROUND (AVG(product_price),2) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_original_price IS NULL
```

Non-discounted products average price is 165.93

The discounted products, are the most sold products last month but, they are in average the most expeesives.

**Finally, I decide to check if the products for which the product minimum offer price is equal to the product price have a higher (or lower) price than the products with an higher price than the poduct minimum offer price.**
```sql
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_price = product_minimum_offer_price
```

The product that have as a final price the lower offer for the preoduct sold an verage 194 unite last month. 

```sql
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
```
The product that have as an higher final price than the lowest offer for the product sold an average 514 units. 

This means that when there is a choice to make, the price is not the main criteria selected by customers.

The price analysis showed that the price cannot be the only criteria to consider. 
If we realize that discounted products are more sold, these product are not necessary the cheapest.
And when the customer gets to chose the same product with differents price options, most of the time they don't chose the cheapest option. 
So, there are other option to consider and to analyze. 

## Review analysis. 

### Is it possible to establish a relation between the sales and the reviews ? 

First let's start with seeing the range of the ratings. 

```sql
SELECT 
MAX (product_star_rating),
MIN (product_star_rating)
FROM `phone_search.phone_search_cleaned` 
WHERE product_star_rating >= '0'
```

The range of rhe rantings goes from 1.6 to 5 

As a reminder, th eompagny is highly interested in products whith at least 3.5 stats. 
Also a minimum of 1,000 reviews is required for a rating to be considered significant. 

How many product do I have including these paramaters. 
```sql
SELECT 
DISTINCT asin
FROM `phone_search_cleaned` 
WHERE product_star_rating >= '3.5'
AND product_num_ratings >= 1000
```

116 products respects the standard quality set by the company.

### Let's see if the price analysis, still stands including the quality standard. 

**how many discounted products with a rating of at least 3.5 stars from at least 1,000 reviews have been sold**
```SQL
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM
(
SELECT *
FROM `phone_search.phone_search_cleaned` 
WHERE product_star_rating >= '3.5'
AND product_num_ratings >= 1000
)
WHERE product_original_price IS NOT NULL
```
An average 719 units.

**At what price ?**
```sql
SELECT 
ROUND (AVG(product_price),2) 
FROM
(
SELECT *
FROM `phone_search.phone_search_cleaned` 
WHERE product_star_rating >= '3.5'
AND product_num_ratings >= 1000
)
WHERE product_original_price IS NOT NULL
```

the average price discounted was 140.35

**And how many non discounted products with a rating of at least 3.5 stars from at least 1,000 reviews have been sold**
```sql
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM
(
SELECT *
FROM `phone_search_cleaned` 
WHERE product_star_rating >= '3.5'
AND product_num_ratings >= 1000
)
WHERE product_original_price IS NULL
```
An average of 513 units. 

**At what price ?**
```sql
SELECT 
ROUND (AVG(product_price),2) 
FROM
(
SELECT *
FROM `phone_search.phone_search_cleaned` 
WHERE product_star_rating >= '3.5'
AND product_num_ratings >= 1000
)
WHERE product_original_price IS NULL
```

average price for non discounted product is 157,3


Among good and excellent reviews, I don't have the same trends.
The dicrounted products are cheaper and more sold here.

### But does that mean that the price has become the mostimportant cireterion ? 

Let's dive deeper within the products that haven't been choose beacause they were cheaper.

**products that respect the quality requierment of the compny, and that have the lowest price offer.**
```sql
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_price = product_minimum_offer_price
```

194 units sold that only have the lower price offer for the product.

```sql
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_price = product_minimum_offer_price
AND product_star_rating >= '3.5'
```
214 units sold that have at leat 3.5 stars and the lower price offer for the product.



```sql
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_price = product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
```
160 units sold that have at leat 3.5 stars from at least a 1000 differents review and the lower  offer for the product.



**How about the products that are chosen on another criteria than the fact that they have the lower price ?**
```sql
SELECT
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
```
Those products solds in average 514

```sql
SELECT
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
```
average 534 units solds 


```sql
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
```
673 sales in average. 

Still the customer makes its choice based on other criteria than the price among thse type of products (with these criteria)


**Let's see how many of these 116 products that haven't been choosed because they had the lowest price are respecting the compagny standards.**
```sql
SELECT 
DISTINCT asin,
product_star_rating
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
```

There are a total of 233 prodcuts that are bought, without beeing the cheapest option. 
How many of them respect the company standards ? 

```sql 
SELECT 
DISTINCT asin,
product_star_rating
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating > '3.5'
```


224 of them have more at least 3.5 stars
So 94% of these products are not bought because they're cheaper.
Having more than 3.5 stars seems to be one of the main criteria 

And about the products that have more than 3.5 stars and 1000 reviews ? 


```sql
SELECT 
DISTINCT asin,
product_star_rating
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
```

106 results

So 91% of the products that respects the comapny standards are products that haven't been bought only based on the price.
48% of the products with more than 3.5 stars that haven't been chose becuase they were cheap have more than 1000 reviews.

On this basis, we can establish that price is not the main criterion chosen by consumers when buying a smartfone on Amazon, and that the products with the best review quality are those with the lowest price. 
To take this a step further, let's see if the Amazon labels also have an impact on sales volume. 


## Label analysis and more. 

### Does the labels have any impact on the sales of a product ? 

**Does best seller label has an impact on the sales?**

```sql
SELECT
DISTINCT asin
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND is_best_seller IS TRUE
```
There is only one product that does hae this label among the most sold products. 


**Does is_amazon_choice has an impact on the sales ?**
```sql
SELECT
DISTINCT asin
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND is_amazon_choice IS TRUE
```
There is also only one result also, this is not significative enough. 

**Does is_prime has an impact on the sales ?**
```sql
SELECT
DISTINCT asin
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND is_prime IS TRUE
```
 There is 73 of these products that are PRME product. 

 
## Are those products more sold ? 

```sql
SELECT
ROUND(AVG(approx_past_month_sales_volume),0)
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND is_prime IS TRUE
```

The result is 629 units


Does the climate_pledge label has an impact on the sales ? 
```sql
SELECT 
DISTINCT asin,
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND climate_pledge_friendly IS TRUE
```
35 results. 

**How many units did they sold on an average?** 
```sql
SELECT
ROUND(AVG(approx_past_month_sales_volume),0)
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND climate_pledge_friendly IS TRUE
```

642 units has been sold

The label climate_pledge_ratings is the labels that seems to have themore best selling proddcuts. 

## is it better to have variation ? 

**Products with variations and the Prime label**
```sql
SELECT
ROUND(AVG(approx_past_month_sales_volume),0)
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND is_prime IS TRUE
AND has_variations IS TRUE

```

439 units

**Products with variations and the Climate Pledge Friendly label**
```sql
SELECT
ROUND(AVG(approx_past_month_sales_volume),0)
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND climate_pledge_friendly IS TRUE
AND has_variations IS TRUE

```
En average 718 units sold



**Which of these products having the Pime and Climate Pledge Friendly sold more than 500 units last month?** 
```sql
SELECT
DISTINCT asin,
approx_past_month_sales_volume,
product_star_rating,
product_num_ratings
FROM `phone_search.phone_search_cleaned` LIMIT 10
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND climate_pledge_friendly IS TRUE
AND has_variations IS TRUE
AND approx_past_month_sales_volume >= 500
order by approx_past_month_sales_volume DESC
```
Most sold products with the Climate Pledge Friendly label

- 1	B07P6Y7954 2000 4.4 64977
- 2	B088NQXD8T 2000 4.3 18826
- 3	B08PPDJWC8 2000 4.1 13191
- 4	B08L34JQ9C 2000 3.9 7272
- 5	B096T6Y623 2000 3.9 2362
- 6	B0BZ9XNBRB 1000 4.3 2482
- 7	B08PMYLKVF 1000 4.2 10317
- 8	B0991J62ZY 1000 3.8 1684
- 9	B09LG4PSB6 500 4.1 1184
- 10 B09LKXHWCF 500 4.1 4265




```sql
SELECT
DISTINCT asin,
approx_past_month_sales_volume,
product_star_rating,
product_num_ratings
FROM `phone_search.phone_search_cleaned` LIMIT 10
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND is_prime IS TRUE
AND has_variations IS TRUE
AND approx_past_month_sales_volume >= 500
order by approx_past_month_sales_volume DESC, product_star_rating DESC
```
Most sold products with the Climate Pledge Friendly label

- 1	B0C2SWQBMB 2000 4.2 2637
- 2	B08L34JQ9C 2000 3.9 7272
- 3	B096T6Y623 2000 3.9 2362
- 4	B08H8VZ6PV 500 4.3 2927










 








































 









