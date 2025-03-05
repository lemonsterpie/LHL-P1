What issues will you address by cleaning the data?

The all_sessions table was the focus of my data cleaning as most of my query and analysis was done using that table. 

The first thing I addressed was deleting the columns that were not relevant to the analysis I conducted for this project. 
```
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
```
Next came the issue of deleting the rows that contain any NULL values. 
```
UPDATE    all_sessions 
SET       total_transaction_revenue =  COALESCE(total_transaction_revenue, 0),
          transactions = COALESCE(transactions, 0),
          time_on_site = COALESCE(time_on_site, 0),
          page_views = COALESCE(page_views, 0),
          product_quantity = COALESCE(product_quantity, 0),
          product_price = COALESCE(product_price, 0),
          product_revenue = COALESCE(product_revenue, 0),
          v2_product_category = COALESCE(v2_product_category, '(not set)'),
          page_title = COALESCE(page_title, '(not set)')
```
I also removed rows that had the values of '(not set)' or 'not available in dataset', which mainly appeared in the country and city columns. 
```
DELETE FROM  all_sessions 
WHERE        country NOT LIKE '%(not set)%'
AND          city NOT LIKE '%(not set)%'
AND          city NOT LIKE '%not available in demo dataset%'
AND          v2_product_category NOT LIKE '%(not set)%'
AND          page_title NOT LIKE '%(not set)%'
```
Finally, I'm dividing the product price by 1,000,000.
```
UPDATE      all_sessions
SET         product_price = ROUND(product_price/1000000, 2)
```

