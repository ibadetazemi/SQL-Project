(In the QA.md file, identify and describe your risk areas. Develop and execute a QA process to address them and validate the accuracy of your results.):

What are your risk areas? Identify and describe them.

(Answer: My risk areas include: Unknown weak points, deleted data, multiple data sources, "Clean" data needs to replace "Old" data and consistant, costly maintencance. )

Provide the SQL queries used to execute the QA process below:


 Checking referential integrity between product and category table using categoryID
-----------------------------------------------------------------------------------------------------------

-- union example from previous week
EXPLAIN ANALYZE
SELECT 'products' AS table_name, 
       COUNT(*) AS count_rows, 
       'categoryid' AS key_name,
       COUNT(DISTINCT(categoryid)) AS count_distinct_key
FROM products

UNION ALL

SELECT 'categories' AS table_name, 
       COUNT(*) AS count_rows,    -- second statment column names dont matter
       'categoryid' AS key_name,
       COUNT(DISTINCT(categoryid)) AS count_distinct_key  -- but the amount of columns do... 
FROM categories;


-- cte to QA using distinct value count of categoryID
WITH CTE AS(
SELECT 'products' AS table_name, 
       COUNT(*) AS count_rows, 
       'categoryid' AS key_name,
       COUNT(DISTINCT(categoryid)) AS count_distinct_categoryid
FROM products

UNION ALL

SELECT 'categories' AS table_name, --f
       COUNT(*) AS count_rows,    
       'categoryid' AS key_name,
       COUNT(DISTINCT(categoryid)) AS count_distinct_categoryid   
FROM categories 
            ), 
            
QA_raw AS(
SELECT COUNT(DISTINCT(count_distinct_categoryid)) as test_result FROM cte
)


SELECT 'referential_integrity_check' AS qa_test_name,
       CASE
        WHEN test_result = 1 THEN 'pass'
        ELSE 'fail'
       END AS qa_result
FROM qa_raw;           


-- Adhoc question about ntile windows function


SELECT
 *, 
 ROUND(COALESCE(100 * (row_count/LAG(row_count) 
  OVER (
   ORDER BY row_count DESC) - 1),0), 2) || '%' AS daily_percentage_change
FROM (
        SELECT DISTINCT * from (
                                 SELECT DISTINCT Date(OrderDate),
                                  Count(*) as row_count
                                  FROM
                                   Orders
                                  WHERE
                                    OrderDate is NOT NULL
                                  Group BY 1
                                  ORDER BY OrderDate DESC
      ) as sq
WHERE
 EXTRACT(YEAR from Date) > '1997' -- and daily_percentage_change > 0
) as sq3
GROUP BY
 Date, row_count;


-- we approached it with a cte in 2 steps
WITH cte AS(
SELECT *, NTILE(3) OVER(ORDER BY unitprice) AS n_test
FROM products
    )

SELECT n_test, COUNT(*), MIN(unitprice), MAX(unitprice), AVG(unitprice) 
FROM cte
GROUP BY n_test
ORDER BY n_test;


SELECT * FROM products;





