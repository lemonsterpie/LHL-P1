Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:
```
WITH grouped_total_revenue AS (
	SELECT 
		country, 
		city,
		SUM(total_transaction_revenue) AS total_transaction_revenue
	FROM 		cleaned_sessions
	GROUP BY 	country, city
	ORDER BY 	SUM(total_transaction_revenue) DESC
)
SELECT 
	*
FROM		grouped_total_revenue
WHERE 		total_transaction_revenue IS NOT NULL
ORDER BY 	total_transaction_revenue DESC
```

Answer:                                                                                                                                                                                                                                                
The top 5 cities with the most total transaction revenue are `San Francisco`, `Sunnyvale`, `Atlanta`, `Palo Alto`, and `Tel Aviv-Yafo`, respectively. It is evident that American cities are prominent in the most profitable cities. Of the 10 cities with the most total revenue, only `Tel Aviv-Yafo` and `Sydney` are not located in the US. Excluding American cities from the results, the other three cities that have total transaction revenues are `Toronto`, CNY?, `Zurich`, and `Chuo`, 
```
WITH grouped_total_revenue AS (
	SELECT 
		country, 
		city,
		SUM(total_transaction_revenue) AS total_transaction_revenue
	FROM 		cleaned_sessions
	GROUP BY 	country, city
	ORDER BY 	SUM(total_transaction_revenue) DESC
)
SELECT 
	*
FROM		grouped_total_revenue
WHERE 		total_transaction_revenue IS NOT NULL AND country NOT LIKE  '%United States%'
ORDER BY 	total_transaction_revenue DESC
LIMIT 		10
```


**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
```
SELECT 
	country, 
	city, 
	AVG (product_quantity) AS avg_product_ordered
FROM		cleaned_sessions
GROUP BY 	1, 2
ORDER BY 	avg_product_ordered DESC
```

Answer:                                                                                                                                                                                                                                                             
Grouping by country and city and looking at the average of product quantity for each group, `Madrid` stands out as the city with the most products ordered on average, double the amount of the second ranking city `Detroit`.  



**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:
```
SELECT 
	country, 
	city,
	COUNT(DISTINCT(product_category)) AS category_count
FROM 		cleaned_sessions
GROUP BY 	country, city
ORDER BY 	category_count DESC 
```

Answer:                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           
Looking at the count of product categories, the US ordered the most variety of categories. The cities with the higher category counts are also among the cities with the highest total revenues. 


**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
```
WITH ranked_total_revenues AS (
	SELECT 
		country,
		city, 
		product_name, 
		SUM(total_transaction_revenue) AS total_revenue, 
		ROW_NUMBER()
	OVER 		(PARTITION BY country, city, product_name ORDER BY SUM(total_transaction_revenue) DESC) AS revenue_rank 
	FROM 		cleaned_sessions
	GROUP BY 	1, 2, 3
)
SELECT                           
	country,
	city, 
	product_name,
	total_revenue
FROM		ranked_total_revenues
WHERE 		revenue_rank = 1 
ORDER BY 	total_revenue DESC 
```

Answer:                                                                                                                                                                                                                                                

Using the query to look at the products with the greatest total transaction revenues, a noteworthy pattern is that the majority of top revenue cities have are in the US, and they have security products ssuch as cameras and alarms as their leading most profitable product. 




**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:
```
CREATE VIEW top_products AS 
	WITH ranked_total_revenues AS (
		SELECT 
			country,
			city, 
			product_name, 
			product_sku,
			SUM(total_transaction_revenue) AS total_revenue, 
			ROW_NUMBER()
		OVER 		(PARTITION BY country, city, product_name ORDER BY SUM(total_transaction_revenue) DESC) AS revenue_rank 
		FROM 		cleaned_sessions
		GROUP BY 	1, 2, 3,4
	)
	SELECT                           
		country,
		city, 
		product_name,
		product_sku,
		total_revenue
	FROM		ranked_total_revenues
	WHERE 		revenue_rank = 1 AND total_revenue != 0 
	ORDER BY 	total_revenue DESC 
```

```
SELECT 
	tp.country,
	tp.city,
	tp.product_name,
	tp.total_revenue,
	tp.product_sku,
	p.stock_level
FROM 		top_products tp
INNER JOIN	products p
USING		(product_sku)
ORDER BY 	total_revenue DESC
```


Answer:
                                                                                                                                                                                                                            Having determined the most profitable cities as well as their most profitable product, the impact of revenue can be reflected in the stock level of the product. The more profitable a product, the higher the stock level, while lower profit products had lower stock levels.                           
    






