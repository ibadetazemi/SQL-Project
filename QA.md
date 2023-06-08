(In the QA.md file, identify and describe your risk areas. Develop and execute a QA process to address them and validate the accuracy of your results.):

What are your risk areas? Identify and describe them.

(Answer: My risk areas include: Unknown weak points, finding erros, deleted data, multiple data sources, "Clean" data needs to replace "Old" data and consistant, costly maintencance. )

Provide the SQL queries used to execute the QA process below:
-------------------------------

Task 1: Data Profiling

Queries:

SELECT *
FROM information_schema.tables
WHERE table_schema = 'public';


--Get column data types and nullability for the products table--

SELECT column_name, data_type, is_nullable
FROM information_schema.columns
WHERE table_name = 'customers';


--Count unique values for the country column in the sales_by_sku table--

SELECT COUNT(DISTINCT country)
FROM sales_by_sku;

Answer: 5

Task 2: Data Validation

Queries:

--Check for missing data in the sales_report table--

SELECT *
FROM sales_report
WHERE quantity IS NULL;


--Check for duplicate data in the products table--
SELECT productsid, COUNT(*)
FROM products
GROUP BY productsid
HAVING COUNT(*) > 1;


Answer: No missing data + no duplicates found

Task 3: Data Cleansing

Queries:

--Clean the country names in the all_sessions table by removing non-alphabetical characters--

UPDATE all_sessions
SET country = regexp_replace(country, '[^a-z]', '', 'g')
WHERE country LIKE '%(%-%) %-%';

--Standardize the country names in the products table--
UPDATE customers
SET country = CASE
    WHEN country IN ('CA', 'Canada') THEN 'United States'
    WHEN country = 'USA' THEN 'United States'
    ELSE country
    END;

Answer: Multiple countries were found

Task 4: Testing

Queries:

--Test that the order dates in the orders table are valid--

SELECT *
FROM orders
WHERE orderdate < '2023-06-01';


--Test that the discount amounts in the sales_report table are accurate--

Queries:

SELECT orderid, SUM(unitprice * quantity * (1 - discount)) AS total_discount
FROM order_details
GROUP BY orderid
HAVING SUM(unitprice * quantity * (1 - discount)) < 0;

Answer: No discounts were found


--Get the largest value of the Sales_by_sku numeric Column--


SELECT  MAX(Sales_by_sku) FROM Visit;

Answer: Max = 50




