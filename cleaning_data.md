What issues will you address by cleaning the data?

Answer: The issues that I will address by cleaning data is: (incorrect, corrupted, incorrectly formatted, duplicate, or incomplete data).


Queries:
Below, provide the SQL queries you used to clean your data.




Data cleaning steps:

1) Remove irrelevant, redundant, or duplicate data:

SELECT * FROM employees
LIMIT 100

SELECT * FROM customers WHERE country = 'Canada'
SELECT DISTINCT * FROM customers

SELECT customerid, firstname FROM customers
SELECT DISTINCT(customerid), firstname FROM customers

SELECT <sales> / <1,000,000>
FROM products
[WHERE sales_report]

2) Clean “structural” issues:

SELECT id, name, email, year, country,
IF(state IN ('Not Applicable', 'N/A'), NULL, state) AS state_clean
FROM customers

3)Type conversion:

SELECT CAST(birth_date AS DATE) AS birthdate FROM customers

4) Clean missing data:

SELECT * FROM customers WHERE year IS NOT NULL


SELECT
CASE
WHEN year IS NULL THEN (SELECT AVG(year) FROM customers),year)
ELSE year
END
FROM customers


SELECT city, region FROM customers
GROUP BY city, region

5) Clean outliers:

SELECT age FROM customers WHERE age >= 18 AND age <= 99

6) Validate:

SELECT * FROM customers
