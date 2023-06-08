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

USE RecoverDeletedRecords

GO

SELECT

 [Current LSN],   

 [Transaction ID],

     Operation,

     Context,

     AllocUnitName

FROM

    fn_dblog(NULL, NULL)

WHERE

    Operation = ‘LOP_DELETE_ROWS’
    
    

Answer:

Question 3: Can you find multiple data sources?

SQL Queries:

SELECT * FROM Customers
WHERE Country IN (SELECT Country FROM Suppliers);

SELECT O.custId, C.custName, SUM(totalAmount)
FROM orders.orderinfo O INNER JOIN customers.customer C ON O.custId = C.custId
GROUP BY O.custID, C.custName

Answer:

Question 4: Can you find the "Clean" data needs to replace "Old" data and consistant?

SQL Queries:

UPDATE products
SET column1 = value1, column2 = value2, ...
WHERE products;

SELECT REPLACE('sales', 'A', 'B');

UPDATE Customers
SET ContactName='Jon'
WHERE Country='Canada';

Answer:

Question 5: Can you find consistant data?

SQL Queries:

DBCC CHECKDB



Answer:
