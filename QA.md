### What are your risk areas? Identify and describe them.
It was crucial that all the NUll values and '(not set)', 'not avaiable' values were adjusted in the all_sessions table. My QA process  involved ensuring that those values were removed from relevant column.    


### QA Process: Describe your QA process and include the SQL queries used to execute it.
```
SELECT * FROM cleaned_sessions WHERE NOT (total_transaction_revenue IS NOT NULL) -- repeated for each column 
```
```
SELECT 
	* 
FROM 		cleaned_sessions
WHERE 		page_title LIKE '%not available in demo dataset%' --'(not set)'
```
