### What issues will you address by cleaning the data?

The `all_sessions` table was the focus of my data cleaning as most of my query and analysis was done using that table. The main issues I addressed in cleaning were the numerous amounts of columns, as well as rows that have either null values or a string indicating not available or not set. Additionally, I also divided the product price and total transaction revenue by 1,000,000 for easier readibility. Cleaning the `products` table involved dropping the columns unecessary for my analysis. 

### Queries: Below, provide the SQL queries you used to clean your data.
```
-- Deleting columns:
ALTER TABLE    all_sessions 
DROP COLUMN    session_quality_dim, 
DROP COLUMN    product_refund_amount,
DROP COLUMN    product_variant,
DROP COLUMN    item_quantity,
DROP COLUMN    item_revenue,
DROP COLUMN    transaction_revenue,
DROP COLUMN    page_path_level1,
DROP COLUMN    ecommerce_action_type,
DROP COLUMN    ecommerce_action_step,
DROP COLUMN    ecommerce_action_option,
DROP COLUMN    transaction_id,
DROP COLUMN    search_keyword
DROP COLUMN 	restocking_lead_time, 
DROP COLUMN	sentiment_score, 
DROP COLUMN 	sentiment_magnitude
```
I chose to clean my data by creating a temporary view on which my question queries refer to. This prevented the frustration of accidentally deleting required data in the table and going through the hassle of importing it in again.
```
CREATE VIEW cleaned_sessions AS 
	WITH non_null_sessions AS (
		SELECT 
			full_visitor_id,
			channel_grouping, 
			country,
			city,
			COALESCE(total_transaction_revenue, 0) AS total_transaction_revenue,
			COALESCE(transactions, 0) AS transactions,
			COALESCE(time_on_site, 0) AS time_on_site,
			COALESCE(page_views, 0) AS page_views,
			date,
			visit_id,
			COALESCE(product_quantity, 0) AS product_quantity,
			ROUND(COALESCE(product_price, 0)/1000000, 2) AS product_price, -- dividing by 1,000,000 
			COALESCE(product_revenue, 0) AS product_revenue,
			product_sku,
			product_name, 
			COALESCE(product_category, '(not set)') AS product_category, -- I can delete it together with the other not set rows in the next step
			currency_code,
			COALESCE(page_title, '(not set)') AS page_title
		FROM all_sessions
	)
	SELECT 
		*
	FROM	non_null_sessions
	WHERE	country NOT LIKE '%(not set)%'
	AND 	city NOT IN ('(not set)', 'not available in demo dataset')
	AND 	product_category NOT LIKE '%(not set)%'
	AND		page_title NOT LIKE '%(not set)%'	
```
The result is a view that has significantly less columns, NULL values replaced by 0, each country and city properly defined and product price in an appropriate unit. 
```
SELECT 
	* 
FROM 	cleaned_sessions
LIMIT 	10
```
