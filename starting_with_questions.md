Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:

SELECT SUM(totaltransactionrevenue)
AS highest_level_of_transaction, currencycode, country, city, timeonsite FROM all_sessions WHERE totaltransactionrevenue IS NOT NULL AND city !='not available in demo dataset' GROUP BY totaltransactionrevenue, currencycode, city, country, timeonsite ORDER BY totaltransactionrevenue DESC



Answer: USA (Atlanta, Sunnyvale, Los Angeles, Seattle), Isreal (Tel-Aviv-Yafo), Australia (Sydney)




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:

SELECT *
FROM products
WHERE UPPER(quantityperunit) LIKE '%PIECES%'
WHERE country IN ('Canada', 'USA, 'Austrailia', 'Isreal') 
 AND title = 'Products'
 AND city = 'All cities'
 AND country = 'All countries'


SELECT * FROM sales
WHERE country IN ('Canada', 'USA') 
 AND title = 'Products'
 AND city = 'All cities'
 AND country = 'All countries'

Answer: 10





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

SELECT * FROM Customers
WHERE Product LIKE 'All cities%, 'All countries%';

SELECT * FROM Customers
WHERE Products LIKE 'All cities%, 'All countries%';

Answer: Yes there is a patten





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:

SELECT MAX(products)
FROM sales
WHERE top-selling;
WHERE country IN ('Canada', 'USA') 
 AND title = 'Products'
 AND city = 'All cities'



Answer: Personal care/food items/handbags, no worthy pattern found.





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

 ( current month revenue - previous month revenue)/previous month revenue * 100.
 WHERE country IN ('All Countries', 'All cities') 


Answer: A high revenue volume has been generated.







