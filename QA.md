What are your risk areas? Identify and describe them.

(Answer: My risk areas include: Unknown weak points, deleted data, multiple data sources, "Clean" data needs to replace "Old" data and consistant, costly maintencance. )

Question 1: Which cities and countries have the highest level of transaction revenues on the site?

SQL Queries:

SELECT c.STATE, c.CITY, SUM(s.QTY * s.SALEPRICE) FROM SALE s INNER JOIN CUST c ON s.custid = c.custid GROUP BY c.STATE, c.CITY ORDER BY c.STATE;

Answer:

Question 2: What is the average number of products ordered from visitors in each city and country?

SQL Queries:

SELECT * FROM products WHERE UPPER(quantityperunit) LIKE '%PIECES%' WHERE country IN ('Canada', 'USA') AND title = 'Products' AND city = 'All cities'

SELECT * FROM sales WHERE country IN ('Canada', 'USA') AND title = 'Products' AND city = 'All cities'

Answer:

Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?

SQL Queries:

SELECT *
 FROM products
 WHERE last_name LIKE 'C%' 



Answer:

Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?

SQL Queries:

SELECT MAX(products) FROM sales WHERE top-selling; WHERE country IN ('Canada', 'USA') AND title = 'Products' AND city = 'All cities'

Answer:

Question 5: Can we summarize the impact of revenue generated from each city/country?

SQL Queries:

( current month revenue - previous month revenue)/previous month revenue * 100. WHERE country IN ('All Countries', 'All cities')

Answer:
