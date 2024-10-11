# Melocoton

## Overview
Melocoton is a young company selling smartphones. The company is very dynamic, and in the space of a few years, business has been booming, with 3 stores launched in 3 different cities. Faced with this success, Sophia, CEO and founder of the company, decided to take on a new challenge. Launch her marketplace on Amazon and conquer the online market. To do this, Sophia, the young entrepreneur, decided to schedule a meeting with the heads of marketing and supply chain. The aim of this meeting is to understand how the Amazon marketplace works. She wants to maintain the same dynamic as the physical stores, and to do this it's important to know the rules and trends of online sales. At the end of the meeting, I'm tasked with analyzing Amazon sales to identify trends in the best-selling products.

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

It is important to point out that there is little information on the period of validity of this data, and that it has not been taken. 
For the purposes of analysis, we will consider the previous month, since we have sales data for the previous month. 

**Now the cleaning**

The cleaning is done in GoogleSheet. 

  - Remove duplicates: select all rows; data; clean data; remove duplicates.
21 rows removed, 320 rows remaining.
Now, using conditional formatting, I'm going to look for duplicates in the asin column (A). The reference is supposed to be unique for a product. The rule is as follows
```
 "=COUNTIF(A:A;A1)>1"
```
4 products have duplicates: B09T3MQSVP, B09T3MQSVP, B0CMDRFVTL, B0CRVWPWKP.
They have not been removed before due to slight stock differences, or discounts (9% instead of 10% but prices are still the same).

  - Removing empty cells: here we'll search for all columns except B; D; H; I; T; O; U; V
To highlight empty cells, we'll use conditional formatting.
On all these columns A1:A320, C1:C320, K1:K320, L1:L320, N1:N320, Q1:Q320, R1:R320, S1:S320, E1:E320, F1:F320, G1:G320, J1:J320, M1:M320, P1:P320
the rule is to display the cell in red if it's empty
    - A: No empty cells
    - C: 4 empty cells B0CMDLJR6K, B07ZHPCJW3, B07Z6Q9NCZ, B09R6FJWWS. As I don't really have any information on the date of data extraction, instead of using the current price indicated on the website, I delete the rows.
    - E: No empty cells
    - F: 3 empty cellsB0DFB9SNBR, B0D79Z15XR, B0DCQX683H. These cells are empty because the products have never been evaluated, as shown in columns G. The value of the cells cannot be 0 as this would be incorrect. I'll put “/” instead.
    - G: No empty cells
    - J: No empty cells
    - K: No empty cells
    - L: No empty cells
    - M: No empty cells
    - N: No empty cells
    - P: No empty cells
    - Q: 17 empty cells. Filtering the columns, I notice that there are other values that don't indicate sales volume in the past month, such as: “List:”, “More buying choices”, “Typical: ” , “Typical price: ” ,Buy branded products from small businesses sold in the Amazon store. Learn more about Amazon's small business partners and Amazon's commitment to empowering them.” 26 in all
After checking the link for each product, we realize that there is no value, or that the wrong value has been imported because Amazon does not display the previous month's sales volume if it is not equal to or greater than 50.
As these products were sold between 0 and 49 times last month, we'll use “25+ purchased last month” as the new value.
25 is the average value.
    - R: No empty cells
    - S: No empty cells

Now that I've checked that there are no duplicates, blank spaces or empty cells, I'll make sure that the values have the best format so that they can be analyzed later. 
I'll start with the format of all prices. 
As the database is extracted from the US Amazon market, we can assume that all this data is indeed in USD and I can then proceed to remove the dollar sign from columns C, D and K. 
To remove the dollar sign: create a new column between columns C and D. Then go to data -> split text -> use “$” as a custom separator. The new column now displays the price without the dollar sign. 
I deleted the empty column and pasted the title into the new column. 
I repeat the operation for D and L. 

As the only currecny used is the USD, we can also delete E. 

I realize that column P “sales_columns” contains data that may be difficult to use in SQL. 
I want to keep only the numbers in this column. 
To do this, I split the text again, using “+” as a separator. 
I now have the number on the one hand, and the text “bought in the last month” on the other. 
I'm deleting this second column as I won't be using it. 
In my column, I now have the numbers, except that the thousands are written as “K”, 2000 = 2K. 
I replace all the Ks with 000s.  To do this: edit -> search and replace -> search K, replace by 000, search'!P:P, replace all. 
I'm now changing the title to “approx_past_month_sales_volume” as the values are not exact (4K+ for example). 

I'm going to do the same for the delivery column. 
I'll divide it into 2 with delivery as separator, and replace FREE by 0.
There was only one item without free delivery.  I manually removed the dollar sign. 

I manually remove the dollar sign in value 2 of the unit_price column.

I now have a clean dataset ready to use. 
It contains 311 rows

# Exploring the data. 

Now I export the file in a CSV and then import it in Bigquerry


I start with checking once again the number of distinct item. 

```sql 
SELECT 
DISTINCT asin AS unique_asin
 FROM `phone_search.phone_search_cleaned`
```

I have 311 lines, meaning that they're no more duplicates.


### A few questions. 

Before going deeper in th analysis there's a few questrions that needs to be asked to get a better understanding of the project. 
Marketing team provided me the answers.

**What is Prime**
The Amazon Prime badge is a highly recognizable icon that signifies a product's eligibility for Amazon Prime's fast, free two-day delivery. This exclusive service has revolutionized the way customers shop online, offering unrivalled convenience and speed.
Having the Prime badge not only improves visibility in search results, but also increases your chances of obtaining the coveted Buy Box. The Buy Box is the section of Amazon's product page where customers can directly add items to their shopping cart. Winning the Buy Box is crucial for sellers, as it significantly increases the likelihood of making a sale.

**What does Best Seller Amazon means ?** 
It is represented by the orange ribbon emblem in the upper-left corner of a product page and is given to top sellers. Customers may make an informed purchase choice by knowing which products have a higher product rating in terms of sales thanks to this emblem.

**What does Amazon's Choice means ?** 
Amazon's Choice makes it easy to discover products that other customers frequently choose for similar purchasing needs. Featured products like Amazon's Choice are rated highly by our customers, are available for immediate shipment and are well-priced. 

**What is Climate Pledge Friendly**
Climate Pledge Friendly highlights products that are recognized by at least one sustainability certification.


**What are the reviews standards ? What is the thereshold ?** 
Reviews on Amazon are classified with stars, 5 stars being the most positive review possible, 0 the least.
More than 4,2 stars is considered an execvellent review. 
In beetwen 4.2 and 3.5 is a good review
From 3,5 to 2.5 is medium 
Below is considered as a bad review.
Melocoton is highly interested in products whith at least 3.5 stats.

**How many reviews is enough to be significative ?** 
A minimum of 1,000 reviews is required for a rating to be considered significant. 

**How many sales per product are we targeting ?** 
At least 500

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

[Link](https://public.tableau.com/views/MelocotonRA1bis/Feuille1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)


I notice here that the products with the highest discounts are not the ones that sell the most.
But this is not enough to come to the conclusion that discount products are more sold. 


To take this a step further, I'm going to compare the average sales of products with discounts to the average sales of products without discounts.
```sql
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0) AS avg_discounted_sales_volume
FROM `phone_search.phone_search_cleaned` 
WHERE product_original_price IS NOT NULL
```

Last month, the products with a discount sold an average 540 units. 

```sql
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0) AS avg_without_discount_sales_volume
FROM `phone_search.phone_search_cleaned` 
WHERE product_original_price IS NULL
```

The products without discount sold an average 350. 

Last month, products with discounts sold more than those without.
However, I'm not sure that the discounted products are the cheapest. 
So I'll take a closer look.



### I am now going to compare the average price of products with discount with the average sales af of product without discounts
```sql
SELECT 
ROUND (AVG(product_price),2) AS avg_dicounted_products_price
FROM `phone_search.phone_search_cleaned` 
WHERE product_original_price IS NOT NULL
```

Discounted products cost an acerage $183.3 

```sql
SELECT 
ROUND (AVG(product_price),2) avg_price_product_without_discount
FROM `phone_search.phone_search_cleaned` 
WHERE product_original_price IS NULL
```

Non-discounted products average price is 165.93

Products with discounts sold more units than those without, but paradoxically the latter were on average more expensive despite the discount. 


### Is price a determining factor in consumer purchasing decisions? 

In order to determine whether the price of a product is very important in the consumer's choice of purchase, I'm going to check whether the best-selling products are indeed those whose price offer was the lowest.

```sql
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0) AS avg_sales_at_lower_offer
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

They're no results for this.

```sql
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_num_offers > 1
```
Products priced at an higher price than the lowest offer sold an average of 514 units. 
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

As a reminder, th compagny is highly interested in products whith at least 3.5 stats. 
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

**how many discounted products with a rating of at least 3.5 stars from at least 1,000 reviews have been sold and at what price ?**
```SQL
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0),
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
An average 719 units with an average price of 140.35 USD.

<img width="1440" alt="image" src="https://github.com/user-attachments/assets/463b30b3-3b53-4f17-9e9d-2d2b7a03eecc">
[Visualization link](https://public.tableau.com/shared/FYHZ42SQF?:display_count=n&:origin=viz_share_link)

<img width="1436" alt="image" src="https://github.com/user-attachments/assets/fc1c6d0b-0858-4c47-96a9-a9ee1057da0b">
[Visualisatoin link](https://public.tableau.com/views/MelocotonRA1bis/Feuille1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)


**And how many non discounted products with a rating of at least 3.5 stars from at least 1,000 reviews have been sold and at what price ?**
```sql
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0),
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
An average of 513 units for an average price of 157,3 USD.

<img width="1440" alt="image" src="https://github.com/user-attachments/assets/9faee5d7-1c2b-4ecf-ad82-262cf12dfbbf">  
[Visualization link ](https://public.tableau.com/views/MelocotonRA2/Feuille1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

<img width="1432" alt="image" src="https://github.com/user-attachments/assets/d246c7c8-7bf9-42a0-a2e1-306ad871294c">
[Visualization link](https://public.tableau.com/views/MelocotonRA2bis/Feuille1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)


Among products considered good and excellent, I don't really observe the same trends. 
In addition to being the best-selling products on average, discounted products are also on average the least expensive.

### But does that mean that the price has become the mostimportant cireterion ? 

Let's dive deeper within the products that haven't been choose beacause they were cheaper.

**products that respect the quality requierment of the compny, and that have the lowest price offer when there is choice between different offers**
```sql
SELECT 
ROUND (AVG(approx_past_month_sales_volume),0) 
FROM `phone_search.phone_search_cleaned` 
WHERE product_price = product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND product_num_offers > 1

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
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
```
Those products solds in average 673 units.

I realize that this trend is still valid


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
COUNT (DISTINCT asin) AS best_seller
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
COUNT (DISTINCT asin) AS is_amazon_choice
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
COUNT (DISTINCT asin) AS is_prime,
ROUND(AVG(approx_past_month_sales_volume),0) AS average_prime_sales
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND is_prime IS TRUE
```
There is 73 of these products that are PRME product. 
629 units for prime products. 


 ```sql
SELECT
COUNT (DISTINCT asin) AS is_not_prime,
ROUND(AVG(approx_past_month_sales_volume),0) AS average_not_prime_sales
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND is_prime IS NOT TRUE
``` 
34 products doesn't have the prime label. 
771 units for products that are not Prime

<img width="1438" alt="image" src="https://github.com/user-attachments/assets/56511f1d-215a-4d5a-8ab1-d8700a13960d">
[Visualization link](https://public.tableau.com/views/averagesaleslabel2/avg_sales_prime?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

**Does the climate_pledge label has an impact on the sales ?**
```sql
SELECT
COUNT (DISTINCT asin) AS is_climate_pledge_friendly, 
ROUND(AVG(approx_past_month_sales_volume),0) AS avg_climate_pledge_friendly_sales
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND climate_pledge_friendly IS TRUE
```
35 products. 
642 units has been sold


```sql
SELECT
COUNT (DISTINCT asin) AS not_climate_pledge_friendly,
ROUND(AVG(approx_past_month_sales_volume),0) AS avg_not_climate_pledge_friendly_sales
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND climate_pledge_friendly IS NOT TRUE
```

71 products doesn't have
An average of 688 units of products that don't have the climate pledge friendly label ws sold.

<img width="1438" alt="image" src="https://github.com/user-attachments/assets/1f98c912-a50f-4e3e-997f-54fb7f7fa662">
[Visualization link](https://public.tableau.com/views/labelshaverasales/avg_sales_climate_pledge_friedly?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)

**Conclusion** : 
I can see that labels have no significant influence on sales. 
In fact, for prime and Climate Pledge Friendly labels, products without them record higher average sales. 
The Best Seller and Amazon`Choice labels, on the other hand, only appear on one product each. 
They therefore don't seem to play a decisive role in consumers' purchasing decisions. 



## Do products with variations sell best? 

**How many products have variations?**
```sql
SELECT
COUNT (DISTINCT asin) AS has_variation,
ROUND(AVG(approx_past_month_sales_volume),0) AS avg_has_variation_sales,
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND has_variations IS TRUE
```

42 units
618 unts sold in average


**How many products don't have variations?**
```sql
SELECT
COUNT (DISTINCT asin), AS without_variation
ROUND(AVG(approx_past_month_sales_volume),0) AS avg_without_variation
FROM `phone_search.phone_search_cleaned` 
WHERE product_price > product_minimum_offer_price
AND product_star_rating >= '3.5'
AND product_num_ratings >= 1000
AND has_variations IS NOT TRUE
```
64 products without variation. 
En average 708 units sold


<img width="1439" alt="image" src="https://github.com/user-attachments/assets/4daed7cc-f8fd-4018-9e3f-9475c75ddc10">
[Visualization link](https://public.tableau.com/views/Averagessalesofproductswithvariation/Feuille1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)


**Conclusion** : 
Consumers don't seem to be influenced by the fact that products have variations either.  


## Here are some insights from the analysis: 

Price is not an important criterion in consumers' purchasing decisions. 
In fact, when given the choice between different offers, consumers do not, in the majority of cases, choose the most expensive one. 
On the contrary, they seem to go for products that have been reviewed and rated highly. 
Amazon's labels and certification don't have much impact either.
Only Prime sells better than other labels.
It's also the most widespread. 
Product variations aren't a determining factor either.

## Recommendations 
Melocoton should focus its sourcing strategy more on quality products. 
As for marketing, concentrate efforts on obtaining Prime certification, which is the most important of all. 










 








































 









