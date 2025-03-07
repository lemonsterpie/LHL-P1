### Question 1: What is the distribution of channel grouping for visits that had profitable transactions compared to all visits?

SQL Queries:
```
CREATE VIEW profitable_orders AS
	SELECT 
		*
	FROM 	cleaned_sessions
	WHERE	total_transaction_revenue != 0
```
```
SELECT 
	channel_grouping,
COUNT		(channel_grouping) AS visit_count
FROM		profitable_orders
GROUP BY 	1
ORDER BY	visit_count DESC
```
```
SELECT 
	channel_grouping, 
COUNT		(channel_grouping) AS visit_count 
FROM		cleaned_sessions 
GROUP BY 	1
ORDER BY 	visit_count DESC
```


Answer:                                                                                                                                                                                                                         

For my own analysis I created a view where `total_transaction_revenue` is not `0` to focus on the profitable orders. Comparing the distributions of channel groupings for profitable and all visits, the prominent difference is that the grouping used most by profitable orders was `Referral`, which was not the top grouping for all visits, which was `Organic Search`. 


### Question 2: Are there any patterns of product category on channel groupings?

SQL Queries:
```
SELECT 
	channel_grouping,
	product_category,  
	COUNT(product_category) AS category_count
FROM 		profitable_orders 
GROUP BY 	1, 2
ORDER BY 	category_count DESC
```
```
SELECT 
	product_category,  
	COUNT(product_category) AS category_count
FROM 		profitable_orders 
GROUP BY 	1
ORDER BY 	category_count DESC
```
Answer:                                                                                                                                                                                                                         

Of the grouping and category combinations that had a category count greater than 1, the most popular product categories were related to Nest products, done through Referral search. This aligns with the the observation of Nest prodducts having the highest category counts across all profitable orders.


### Question 3: Are there any correlations between the season of the order and its product category? 

SQL Queries:
```
SELECT 
	CASE
		WHEN EXTRACT(MONTH FROM date) IN (12, 1, 2) THEN 'Winter'
        WHEN EXTRACT(MONTH FROM date) IN (3, 4, 5) THEN 'Spring'
        WHEN EXTRACT(MONTH FROM date) IN (6, 7, 8) THEN 'Summer'
        WHEN EXTRACT(MONTH FROM date) IN (9, 10, 11) THEN 'Fall'
    END AS season, 
	product_category,
	COUNT(product_category) AS category_count
FROM		profitable_orders
GROUP BY 	1, 2
ORDER BY 	category_count DESC	
```
Answer:                                                                                                                                                                                                                          

A notable pattern here is that of the product categories that had category count greater than 1, categories that contained aparrel products only occured in spring. Winter and summer had multiple Nest products, but fall had no multiple category counts at all.

