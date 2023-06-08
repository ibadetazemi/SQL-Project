(In the QA.md file, identify and describe your risk areas. Develop and execute a QA process to address them and validate the accuracy of your results.):

What are your risk areas? Identify and describe them.

(Answer: My risk areas include: Unknown weak points, deleted data, multiple data sources, "Clean" data needs to replace "Old" data and consistant, costly maintencance. )

Provide the SQL queries used to execute the QA process below:


 Checking referential integrity for sales_by_sku + 'products table using categoryID
-----------------------------------------------------------------------------------------------------------

-- Ecommerce SQL Project

1) First step: Select Query to analye data:

--Select command--

SELECT Column1, Column2,
 
FROM <sales_by_sku>;

---Here the following columns: (Column1 + Column2) are the Field/Attribute names.---

---Displays all the available fields---

SELECT Column1, Column2,......
 
FROM <Sales_by_sku>

Answer:

First Name,Last Name,Date Of Birth,Email
John,Doe,1995-01-05,john.doe@postgresqltutorial.com
Jane,Doe,1995-02-05,jane.doe@postgresqltutorial.com<img width="261" 

SELECT * FROM Customer;

2) Get the number of products from the products table:

	
SELECT ID,Products FROM Customers;

SELECT COUNT("ProductId") FROM "Products"

Answer = Total number of products = 246


3) Get the largest value of the Sales_by_sku numeric Column.


SELECT  MAX(Sales_by_sku) FROM Customers;

Answer: Min = 2,000




