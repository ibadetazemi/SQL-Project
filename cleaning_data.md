What issues will you address by cleaning the data?

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
