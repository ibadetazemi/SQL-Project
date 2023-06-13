# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
(To create and analyze sales performance report + to analyze and clean data + find risk areas, identifying potential issues)

## Process
### (Step 1: Remove irrelevant, redundant, or duplicate data)
### (Step 2: Clean “structural” issues)
### (Step 3: Type conversion)
### (Step 4: Clean missing data)
### (Step 5: Clean outliers)
### (Step 6: Validate)

Data cleaning steps:

Remove irrelevant, redundant, or duplicate data:
SELECT * FROM visitid LIMIT 200

SELECT * FROM id WHERE country = 'Canada' SELECT DISTINCT * FROM persons

SELECT userid, firstname FROM accounts SELECT DISTINCT(visitid), firstname FROM accounts

SELECT / <1,000,000> FROM products [WHERE sales_report]

Clean “structural” issues:
SELECT id, name, email, year, country, IF(state IN ('Not Applicable', 'N/A'), NULL, state) AS state_clean FROM accounts

3)Type conversion:

SELECT CAST(birth_date AS DATE) AS birthdate FROM persons

Clean missing data:
SELECT * FROM sales WHERE salesid IS NULL

SELECT * FROM userid WHERE year IS NOT NULL

SELECT CASE WHEN year IS NULL THEN (SELECT AVG(year) FROM accounts),year) ELSE year END FROM accounts

SELECT city, region FROM accounts GROUP BY city, region

COALESCE(products, company, 'missing')

Clean outliers:
SELECT age FROM accounts WHERE age >= 18 AND age <= 99

Validate:
SELECT * FROM sales

---

##Codes:


SELECT
   Website_sessions_sku,
   Website_sessions_orderquantity,
   Website_sessions_stocklevel,
   COUNT (DISTINCT Website_sessions. website_sessions_id) AS sessions
   COUNT (DISTINCT Orders, order_id) AS orders,
   COUNT (DISTINCT Orders. order_id)/COUNT (DISTINCT Wbsite_sessions. website_sessions_id) *100 AS session_to_order_conversion_rate
   
   FROM Website_sessions
      LEFT JOIN Orders
         On Websire_sessions. website_session_id=Orders. website_session_id
         
   GROUP BY sku, orderquantity,stocklevel
   
   ORDER BY COUNT(DISTINCT Website_sessions.website_session_id)DESC;
   
SELECT

   Website_sessions. device_type.
   COUNT (DISTINCT Website_sessions.website_session_id) AS sessions,
   COUNT (DISTINCT Orders. order_id) AS orders,
   COUNT (DISTINCT Orders. order_id)/COUNT (DISTINCT Website_sessions. website_session_id) AS session_to_order_conversion_rate
   
FROM Website_sessions
     LEFT JOIN Orders
        ON Website_sessions. website_session_id=Orders.website_session_id
        
WHERE Website_sessions.sku='brand/nonbrand'
   AND orderquantity='max'
   AND stocklevel='max'
   
AND Website_sessions.created_at<'2012-05-11'
GROUP BY Website_sessions.device type
    
## Results
(Fill in what you discovered this data could tell you: What I had discovered with this data is that it had contained a very large amounts of products and sales and took some time to clean and filter the data and it had contained some patterns, some errors, duplicates etc.)
And how you used the data to answer those questions: I had used the data to answer these questions by analysing the data and interpreting the data)

## Challenges 
(Some challenges that I had experienced include: Inconsistent data that creates confusion, incorrect data, value entered in the wrong field, Errors in domain format and missing values.)

## Future Goals
(What would you do if you had more time?)
(Answer: If I had more time I would go more in depth with the data cleaning process and educate others about what I had learned and try to learn more.)
