Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

SELECT SUM(totaltransactionrevenue)
AS highest_level_of_transaction, currencycode, country, city, timeonsite FROM all_sessions WHERE totaltransactionrevenue IS NOT NULL AND city !='not available in demo dataset' GROUP BY totaltransactionrevenue, currencycode, city, country, timeonsite ORDER BY totaltransactionrevenue DESC



Answer: Country: USA City: (San Francisco)




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

SELECT ROUND(AVG(productquantity)) AS Avg_Product_Quantity, city, country
FROM all_sessions
WHERE city != 'not available in demo dataset' AND city IS NOT NULL
GROUP BY productquantity, city, country
ORDER BY productquantity

SELECT a.country, a.city, cast(AVG(an.units_sold) as decimal(10,2)) AS averageProductCount
FROM all_sessions AS a
JOIN analytics AS an ON a.visitId = an.visitId
where units_sold is not null
GROUP BY a.country, a.city;


Answer: 1





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

SELECT
    a.country as country,
    a.city as city,
    a.v2ProductCategory as productcategory,
    SUM(an.units_sold) as totalUnitsSold
FROM all_sessions AS a
JOIN analytics AS an ON a.visitId = an.visitId
JOIN products AS p ON a.productSKU = p.SKU
WHERE a.country != '(not set)'
    AND a.city != '(not set)'
    AND a.v2ProductCategory != '(not set)'
GROUP BY a.country, a.city, a.v2ProductCategory
HAVING SUM(an.units_sold) IS NOT NULL
ORDER BY a.country, a.city, totalUnitsSold DESC


select city, country, v2productcategory, count(v2productcategory) as categ
from all_sessions
where city is not NULL and city != 'not available in demo dataset'
and v2productcategory != '(not set)' and v2productcategory != '${escCatTitle}'
group by v2productcategory,city, country
order by categ desc;

Answer: Yes there is a pattern there was lots of Google products, pet products, men's products and electronics.





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
WITH SalesData AS (
  SELECT Customers.City, Customers.Country, Products.ProductName,
         order_details.UnitPrice * order_details.Quantity AS SalesAmount
  FROM Customers
  INNER JOIN Orders ON Customers.CustomerID = Orders.CustomerID
  INNER JOIN order_details ON Orders.OrderID = order_details.OrderID
  INNER JOIN Products ON order_details.ProductID = Products.ProductID
),
AggregatedSales as (
  SELECT City, Country, ProductName, SUM(SalesAmount) AS TotalSales
  FROM SalesData
  GROUP BY City, Country, ProductName
)

WITH ranked_products as (
SELECT 
    als.country, 
    p.name, 
    RANK() OVER (PARTITION BY als.country ORDER BY ana.units_sold DESC) as rank
FROM all_sessions as als
JOIN analytics as ana ON als.visitid = ana.visitid
JOIN products as p ON als.productsku = p.sku
GROUP BY als.country, p.name, ana.units_sold
ORDER BY UNITS_SOLD
)

SELECT country, rank
FROM ranked_products 
GROUP BY country, Rank



Answer: Home/Apparel/Men's/Men's T-Shirt (SPF-15 Slim & Slender Lip Balm, Alpine Style Backpack, Twill Cap). There is a pattern of home security products.





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

 SELECT city, country,
    SUM(totaltotalTransactionRevenue)AS
  totalRevenue,
    COUNT(DISTINCT "transactionId")AS
  totalTransactions
  FROM all_sessions
  GROUP BY city, country;
  


Answer: USA - San Francisco has the highest revenue.







