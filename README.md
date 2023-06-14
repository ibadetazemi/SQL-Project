# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
(To create and analyze total revenue generated, sales performance report + to analyze and clean data + find risk areas, identify potential issues + to understand the website visitor buying behaviour, time spent on site and product category.)

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

##Website activity (Traffic to a site helps helps us to track website activity)


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
   
   ###Finding the conversion rates (If desktop performance is better than on mobile then desktop may provide more volumes)
   
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

##Trend analysis (Using trend analysis to help see how far ecommerce business has come in the last 5 years)

SELECT
   YEAR(created_at)AS year,
   YEAR(created_at)AS week,
   MIN(DATE(created_at)) AS week_starts,
   COUNT (DISTINCT website_session_id) AS sessions
   
FROM Website_sessions
WHERE Website_sessions
WHERE website_session_id BETWEEN 100000 AND 200000--Arbitrary
GROUP BY YEAR (created_at), WEEK(created_at);

##Temporary table for entry page analysis (And join them with itself to get the most common entry page for a specific period)

CREATE TEMPORARY TABLE first_pageview
SELECT
     website_session_id
     MIN(website_pageview_id)AS first_page_visited_id
FROM Website_pageviews
WHERE website_pageview_id<1000--arbitrary
GROUP BY website_session_id;
SELECT
    COUNT(DISTINCT(first_pageview. website_session_id)) AS sessions_hitting_this_lander,
    Website_pageviews. pageview_url AS landing_page--or entry page
    
FROM first_pageview
LEFT JOIN Website_pageviews
   ON first_pageview.first_page_visited_id=Website_pageview_id
GROUP BY Website_pageviews.pageview_url;

##Time On site: (Checking the time on site)

ALTER TABLE analytics
ALTER COLUMN timeonsite
TYPE VARCHAR
USING LPAD(timeonsite::VARCHAR, 6, '0');

UPDATE analytics
SET timeonsite = CONCAT(
    SUBSTRING(timeonsite, 1, 2),
    ':',
    SUBSTRING(timeonsite, 3, 2),
    ':',
    SUBSTRING(timeonsite, 5, 2)
);

UPDATE analytics
SET timeonsite = TIME '00:00:00' + 
           INTERVAL '1 hour' * CAST(SUBSTR(timeonsite, 1, 2) AS INTEGER) +
           INTERVAL '1 minute' * CAST(SUBSTR(timeonsite, 4, 2) AS INTEGER) +
           INTERVAL '1 second' * CAST(SUBSTR(timeonsite, 7, 2) AS INTEGER);

ALTER TABLE analytics
ALTER COLUMN timeonsite TYPE TIME USING timeonsite::TIME;

##Joining tables

SELECT Visitors.VisitorName, Products.ProductsID
FROM Visitors
LEFT JOIN Orders ONVisitors.VisitorID = Orders.VisitorID
ORDER BY Visitor.VisitorName;
    
## Results
(Fill in what you discovered this data could tell you: What I had discovered with this data is that it was not clean data, it took some time to re-organize the data and transform the data and clean/filter the data. It was challenging to manually add the columns and then try to apply different queries to adhere to each column. And USA made the most revenue and organic search was the highest lead. There was no clear pattern on time spent on site.)
And how you used the data to answer those questions: I had used the data to answer these questions by analyzing the data and interpreting the data)

## Challenges 
(Some challenges that I had experienced include: Null values, having to change time, and having to manually add columns as well as numerous transformations and data cleaning. There was also missing values, Inconsistent data that creates confusion, incorrect data, value entered in the wrong field, Errors in columns/tables and missing/null values. The PGAdmin app also crashes and the lack/limited data for the columns.)

## Future Goals
(What would you do if you had more time?)
(Answer: If I had more time I would go more in-depth with the data cleaning process, transformations, troubleshooting, and educate others about what I had learned and try to learn more.)
