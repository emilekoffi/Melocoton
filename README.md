# Melocoton

## Overview
Melocoton, is a young smartphone retailer company.
The company is very dynamic, and in a space of a few year, business has been going pretty well, 3 shops have been launched in 3 different city.. 
Facing such success, Sophia CEO and founder of the company, decided to face a new challenge. 
Launch its marketplace on Amazon and conquer the online market.
In order to do so, Sophia, the young entrepreneur, decided to plan a meeting with marketting and supply. 
The purpose of this meeting is to understand how the market on amazon works.
She wants to keep the same dynamic as the physical shops, and to do so it's important to know the rules. 
At the end of this meeting, I have been task to analyse amazon sales in order to identify trends among the most sold product
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
MAX(product_price),
MIN(product_price)
FROM `phone_search.phone_search_cleaned` 
WHERE approx_past_month_sales_volume >=2000
```

<img width="1165" alt="image" src="https://github.com/user-attachments/assets/7e65ab56-9ec8-4680-89d8-6f2742fa7dca">

For the time being, it is difficult to establish a relationship between sales volume and product price, as the best-selling products range in price from 18.45 to 287.3 USD.  

Sn, in order to establish this relationship, I'll start by analyzing whether products with discounts sell more than products without.

### Does dicounts attract more sales ? 

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

<img width="724" alt="image" src="https://github.com/user-attachments/assets/6d2bc64a-5c78-4a86-8b06-2c35d2ca2e0e">


I notice here that the products with the highest discounts are not the ones that sell the most.
But this is not enough to come to the conclusion that discount products are more sold. 


To take this a step further, I'm going to compare the average sales of products with discounts to the average sales of products without discounts.
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

Last month, products with discounts sold better than those without.
However, I'm not sure that the discounted products are the cheapest. 
So I'll take a closer look.


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

Products with discounts sold better than those without, but paradoxically the latter were on average more expensive despite the discount. 



**In order to determine whether the price of a product is very important in the consumer's choice of purchase, I'm going to check whether the best-selling products are in fact those whose price offer was the lowest.**
```sql
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_price = product_minimum_offer_price

```

Products priced at the lowest offer sold an average of 194 units.

```
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_price = product_minimum_offer_price
AND product_num_offers > 1
```

Il n'y a pas de résultat

```sql
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_num_offers > 1
```

Products priced at the lowest offer sold an average of 514 units. 
This means that when the consumer has had the choice of the same product at different prices, in the majority of cases, he hasn't chosen the cheapest offer and that the consumer only buys the products with the cheapest offer when he has no choice.


Price analysis has shown that price cannot be the only criterion to be taken into consideration. 
While we know that discounted products sell better, they are not necessarily the cheapest.
And when customers have the opportunity to choose the same product with different price options, they don't choose the cheapest option.
They only do when they have no choice
So there are other options besides price to consider and analyze . 


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

How many products meet these criteria? 
```sql
SELECT 
DISTINCT asin
FROM `phone_search.phone_search_cleaned` 
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

Among products considered good and excellent, I don't really observe the same trends. 
In addition to being the best-selling products on average, discounted products are also on average the least expensive.

### But does that mean that the price has become the mostimportant cireterion ? 

Let's dive deeper within the products that haven't been choose beacause they were cheaper.

**products that respect the quality requierment of the compny, and that have the lowest price offer.**

```sql
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_price = product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000

```
The result is still null


**How about the products that are chosen on another criteria than the fact that they have the lower price ?**

What about the products sold despite the fact that they don't offer the cheapest deal? 

All products not offering the lowest price? 
```sql
SELECT
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_num_offers > 1

```
Those products solds in average 514

All products not offering the lowest price with at least 3.5 stars? 
```sql
SELECT
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
```
average 534 units solds 

All products not offering the lowest price with at least 3.5 stars and 1000 reviews.? 
```sql
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
```
673 sales in average. 

I'm realizing that this trend is still valid, and even more so: the more qualitative criteria I add, the higher the average number of sales.


**Let's see how many of the 116 products complying with the company's quality standards are priced above the cheapest offer available.**
```sql
SELECT 
COUNT (DISTINCT asin)
FROM `phone_search.phone_search_cleaned` 
WHERE product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND product_price > product_minimum_offer_price
AND product_num_offers > 1
```
106 produits, 91% of them.
 


On this basis, we can establish that price is not the main criterion chosen by consumers when buying a phone related item on Amazon, Consummer seems to care a lot about the quelity of the product. 
To take this a step further, let's see if the Amazon labels also have an impact on sales volume. 


## Label analysis and more. 

### Does the labels have any impact on the sales of a product ? 

**Does best seller label has an impact on the sales?**

```sql
SELECT
COUNT (DISTINCT asin)
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
COUNT (DISTINCT asin) AS best_seller
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND is_best_seller IS TRUE
```
There is also only one result also, this is not significative enough. 

**Does is_prime has an impact on the sales ?**
```sql
SELECT
COUNT (DISTINCT asin) AS is_prime
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND is_prime IS TRUE
```
 There is 73 of these products that are PRME product. 

 ```sql
SELECT
COUNT (DISTINCT asin) AS is_not_prime
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND is_prime IS NOT TRUE
```
 
34 products doesn't have the prime label.
 
**Which have more sales ?** 

```sql
SELECT
ROUND(AVG(approx_past_month_sales_volume),0) AS average_prime_sales
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND is_prime IS TRUE
```

629 units for prime products. 

```sql
SELECT
ROUND(AVG(approx_past_month_sales_volume),0) AS average_not_prime_sales
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND is_prime IS NOT TRUE
```
771 units for products that are not Prime


Does the climate_pledge label has an impact on the sales ? 
```sql
SELECT
COUNT (DISTINCT asin),
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND climate_pledge_friendly IS TRUE
```
35 products. 

```sql
SELECT
COUNT (DISTINCT asin) AS not_climate_pledge_friendly,
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND climate_pledge_friendly IS NOT TRUE
```

71 products doesn't have


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

```sql
SELECT
ROUND(AVG(approx_past_month_sales_volume),0)
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND climate_pledge_friendly IS NOT TRUE
```

An average of 688 units of products that don't have the climate pledge friendly label ws sold.

**Conclusion** : 
I can see that labels have no significant influence on sales. 
In fact, for prime and Climate Pledge Friendly labels, products without them record higher average sales. 
The Best Seller and Amazon`Choice labels, on the other hand, only appear on one product each. 
They therefore don't seem to play a decisive role in consumers' purchasing decisions. 



## Do products with variations sell best? 

**How many products have variations?**
```sql
SELECT
COUNT (DISTINCT asin),
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND has_variations IS TRUE
```

42 units

**And how many units sold in average ?**
```sql
SELECT
ROUND(AVG(approx_past_month_sales_volume),0)
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND has_variations IS TRUE
```

618 unts sold in average


**How many products don't have variations?**
```sql
SELECT
COUNT (DISTINCT asin),
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND has_variations IS NOT TRUE
```

64 products without variation. 

**And how many units sold in average ?**
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

**Conclusion** : 
Consumers don't seem to be influenced by the fact that products have variations either.  










 








































 









