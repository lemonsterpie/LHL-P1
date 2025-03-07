# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
This project involved analyzing the ecommerce database for trends and patterns. The goal is to find any factors that correlate with transaction revenue, offering insights on future ways of optimization to maximize transaction revenue. 

## Process
### 1. Cleaning 
The first part of the process involved cleaning the tables in the dataset. The `all_sessions` table was used the most frequently in my queries, where I addressed the issues of unecessary columns, NULL values and insufficient string data, and scaling the monetary columns for better readibility.

### 2. Analysis 
The first analysis involved determining the most profitable countires their average products ordered, as well as their top selling products and respective categories. From there, I looked at the distribution of channel grouping and any effects they had on the product category of a visit, conduction a comparison of orders that resulted in profit against all visits. I also looked at the count of the product category of profitable orders and their season ordered. 

## Results
The results from my analysis show that the database largely consists of data from the United States, with only a few entries from other countries. The US had the most variation in product category ordered, and the cities that had higher variations in product categories ordered were also among the high revenue cities. In regards to the specific products ordred, the top selling product is resusable shopping bas in Atlanta, followed by SPF lip balm From Sunnyvale. A notable pattern here is that aside from the top two most profitable items, security camera or battery alarm related products are very prevalent in the top most profitable items. The impact of revenue generated from each city is reflected on the stock level of each product. The more profitable products have a higher stock, while the less profitable ones have lower stock. Results of the channel grouping analysis showed that referral orders were the most profitable even though that grouping was not the most frequently appearing the the whole table. The most common product categories in this grouping were related to Nest products. Finally, of the product categories that had category count greater than 1, categories that contained aparrel products only occured in spring, winter and summer had multiple Nest products, but fall had no multiple category counts at all. 
## Challenges 
The majority of the challenges I faced was with cleaning the data. There were many NULL values as were as string values that provided insufficient information for my analysis. I also initially had a challenging time with accidentally dropping a column I actually required, then having tto go through the hassle of importing the column information in again. This was an oversight that was fixed by using the table toe create a View of itself in which I conducted data manipulation on, and then creating a new table off of the VIEW. 

## Future Goals
If I had more time I would do a deeper cleaning of the product category column for a closer analysis. Specifically, I noticed that each product often had several categories in the string, often with a lot of redundancy. For example, so many categories start in the Home category, and there is a lot of overlap for Apparel, Office, as well as seperation by sex (men vs. women). As such, I think it would be productive for future analysis if the product category column was isolated to a single, more specific string.

