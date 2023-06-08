(In the QA.md file, identify and describe your risk areas. Develop and execute a QA process to address them and validate the accuracy of your results.):

What are your risk areas? Identify and describe them.

(Answer: My risk areas include: Unknown weak points, deleted data, multiple data sources, "Clean" data needs to replace "Old" data and consistant, costly maintencance. )

Provide the SQL queries used to execute the QA process below:


Question 1: Find unknown weak points?

SQL Queries: 

SELECT DISTINCT
	PRODUCT.ProductID,
	PRODUCT.Name
FROM Production.Product PRODUCT
INNER JOIN Sales.SalesOrderDetail DETAIL
ON PRODUCT.ProductID = DETAIL.ProductID
OR PRODUCT.rowguid = DETAIL.rowguid;

SELECT
	PRODUCT.ProductID,
	PRODUCT.Name
FROM Production.Product PRODUCT
INNER JOIN Sales.SalesOrderDetail DETAIL
ON PRODUCT.ProductID = DETAIL.ProductID
UNION
SELECT
	PRODUCT.ProductID,
	PRODUCT.Name
FROM Production.Product PRODUCT
INNER JOIN Sales.SalesOrderDetail DETAIL
ON PRODUCT.rowguid = DETAIL.rowguid

Answer:

Question 2: Can you find any deleted data?

SQL Queries:



Answer:

Question 3: 

SQL Queries:

Answer:

Question 4: 

SQL Queries:


Answer:

Question 5: 

SQL Queries:



Answer:
