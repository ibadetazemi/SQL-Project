What issues will you address by cleaning the data??

Answer: The issues that I had addressed by cleaning data includes: (incorrect, corrupted, incorrectly formatted, duplicate, or incomplete data).


Queries:
Below, provide the SQL queries you used to clean your data.




Data cleaning steps:

1) Remove irrelevant, redundant, or duplicate data:

SELECT * FROM visitid
LIMIT 200

SELECT * FROM id WHERE country = 'Canada'
SELECT DISTINCT * FROM persons

SELECT userid, firstname FROM accounts
SELECT DISTINCT(visitid), firstname FROM accounts

SELECT <sales> / <1,000,000>
FROM products
[WHERE sales_report]

update all_sessions 
set city = NULL
where city = '(not set)'

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


2) Clean “structural” issues:

SELECT id, name, email, year, country,
IF(state IN ('Not Applicable', 'N/A'), NULL, state) AS state_clean
FROM accounts

3)Type conversion:

SELECT CAST(birth_date AS DATE) AS birthdate FROM persons

4) Clean missing data:
  
  
SELECT *
FROM sales
WHERE salesid IS NULL

SELECT * FROM userid WHERE year IS NOT NULL


SELECT
CASE
WHEN year IS NULL THEN (SELECT AVG(year) FROM accounts),year)
ELSE year
END
FROM accounts


SELECT city, region FROM accounts
GROUP BY city, region
  
COALESCE(products, company, 'missing')

5) Clean outliers:

SELECT age FROM accounts WHERE age >= 18 AND age <= 99

6) Validate:

SELECT * FROM sales

7) Changing the price by divided by 1,000,0000
   
UPDATE all_sessions
SET productPrice= CAST(productprice/1000000.0 AS DECIMAL(10, 2))
;

UPDATE all_sessions
SET totaltransactionrevenue= CAST(totaltransactionrevenue/1000000.0 AS DECIMAL(18, 2))
;

UPDATE analytics
SET unitprice= CAST(unitprice/1000000.0 AS DECIMAL(10, 2))
;

UPDATE analytics
SET revenue= CAST(revenue/1000000.0 AS DECIMAL(18, 2))
;

8) Format date and time
   
ALTER TABLE all_sessions
ALTER COLUMN time
TYPE VARCHAR
USING LPAD(time::VARCHAR, 6, '0');

UPDATE public.all_sessions
SET time = SUBSTR(time, 1, 2) || ':'||  SUBSTR(time, 3, 2) || ':'||  SUBSTR(time, 5, 2);

UPDATE public.all_sessions
SET time = TIME '00:00:00' + 
           INTERVAL '1 hour' * CAST(SUBSTR(time, 1, 2) AS INTEGER) +
           INTERVAL '1 minute' * CAST(SUBSTR(time, 4, 2) AS INTEGER) +
           INTERVAL '1 second' * CAST(SUBSTR(time, 7, 2) AS INTEGER)
;

9) Clean up city and country
    
UPDATE all_sessions
SET country = CASE
        WHEN country LIKE '%not set%'
		THEN 'n/a'
        ELSE CAST(country AS VARCHAR)
		END
;
UPDATE all_sessions
	SET city= CASE
        	WHEN city IN ('not available in demo dataset', '(not set)') 
				THEN CAST(country AS VARCHAR)
        	ELSE CAST(city AS VARCHAR)
		END
;
Organized product base on brand

10) UPDATE all_sessions
    
SET brand= CASE
		WHEN v2productname LIKE '%Google%' OR v2productcategory LIKE '%Google%'
			THEN 'Google'
		WHEN v2productname LIKE '%Android%' OR v2productcategory LIKE '%Android%'
			THEN 'Android'
		WHEN v2productname LIKE '%Waze%' OR v2productcategory LIKE '%Waze%'
			THEN 'Waze'
		WHEN v2productname LIKE '%Nest%' OR v2productcategory LIKE '%Nest%'
			THEN 'Nest'
		WHEN v2productname LIKE '%YouTube%' OR v2productcategory LIKE '%YouTube%'
			THEN 'YouTube'
		ELSE 'Generic'
	END 
;

11) Create a new table using all_sessions to contain product info
    
CREATE TABLE session_products AS
    SELECT productsku
            , v2productname AS productname
            , v2productcategory AS category
            , brand
            , productquantity
            , productprice
    FROM all_sessions
,

12) Categorize product in all sessions
    
UPDATE session_products
SET category = CASE
        WHEN productname LIKE '%Men%'
	            OR productname LIKE '%Women%' 
	            OR productname LIKE '%Toddler%'
                Or productname LIKE '%Onesie%'
				OR productname LIKE '%Tee%'
				OR productname LIKE '%Cap%'
				OR productname LIKE '%Hat%'
	            OR category LIKE '%Apparel%'
                OR productname LIKE '%hirt%'
                OR productname LIKE '%Hood%'
		    THEN 'Apparel'
        ELSE CAST(category AS VARCHAR)
END 
;
UPDATE session_products
SET category = CASE
        WHEN productname LIKE '%Bottle%'
                OR productname LIKE '%Mug%'
                OR productname LIKE '%Tumbler%'
                OR category LIKE '%Drinkware%'
            THEN 'Drinkware'
        WHEN productname LIKE '%Lip Balm%'
                OR productname LIKE '%Mat%'
				OR productname LIKE '%Sanitizer%'
                OR productname LIKE '%Selfie%'
                OR category LIKE '%Lifestyle%'
                OR category LIKE '%Sports & Fitness%'
            THEN 'Lifestyle'
        WHEN productname LIKE '%Bag%'
                OR productname LIKE '%Rucksack%'
				OR productname LIKE '%Backpack'
				Or productname LIKE '%Pouch'
                OR productname LIKE '%Tote'
                OR category LIKE '%Bag%'
            THEN 'Bag'
        WHEN productname LIKE '%Nest%'
            OR category LIKE '%Nest%'
            THEN 'Nest'
    ELSE CAST(category AS VARCHAR)
END
;
UPDATE session_products
SET category = CASE
        WHEN productname LIKE '%Decal%'
	            OR productname LIKE '%Sticker%' 
	            OR productname LIKE '%Windup%'
                OR productname LIKE '%Tag%'
                OR productname LIKE '%Mount%'
		        OR productname LIKE '%Umbrella%'
		        OR productname LIKE '%Holder%'
                OR productname LIKE '%Device Stand%'
                OR productname LIKE '%Sunglasses%'
                OR category LIKE '%Accessories%'
		    THEN 'Accessories'
    ELSE CAST(category AS VARCHAR)
END
;
UPDATE session_products
SET category = CASE
        WHEN productname LIKE '%Book%'
	            OR productname LIKE '%Pen%' 
                OR productname LIKE '%book%' 
	            OR productname LIKE '%Journal%'
	            OR category LIKE '%Office%'
		    THEN 'Office'
    ELSE CAST(category AS VARCHAR)
END
;
UPDATE session_products
SET category = CASE
        WHEN productname LIKE '%Light%'
                OR productname LIKE '%Device Stand%'
                OR productname LIKE '%Bank%'
				OR productname LIKE '%light%'
				OR productname LIKE '%Bluetooth%'
                OR productname LIKE '%Charger%'
	            OR category LIKE '%ELectronic%'
		    THEN 'Electronics'
    ELSE CAST(category AS VARCHAR)
END
;
UPDATE session_products
SET category = CASE
        WHEN category LIKE '%Home%'
                OR category LIKE '%not set%'
		    THEN 'MISC'
    ELSE CAST(category AS VARCHAR)
    END
;

14) Create Revenue Table from analytics
    
CREATE TABLE revenue AS
	SELECT Distinct (fullvisitorid) AS fullvisitorID
		, SUM(units_sold) AS Total_units_sold
		, SUM (revenue) AS Total_revenue
	FROM Analytics
	WHERE revenue IS NOT NULL
	GROUP BY fullvisitorid
	ORDER BY Total_revenue DESC
 
16) Create sales_analytics table to remove duplicate fullvisitorID
    
CREATE TABLE sale_analytics AS
	SELECT DISTINCT(fullvisitorID)
		, country
		, city
		, type
		, channelgrouping
		, total_revenue 
		, total_units_sold
	FROM revenue 
	JOIN all_sessions
	USING (fullvisitorID)
;
