Consider the data you have available to you. You can use the data to: - find all duplicate records - find the total number of unique visitors (fullVisitorID) - find the total number of unique visitors by referring sites - find each unique product viewed by each visitor - compute the percentage of visitors to the site that actually makes a purchase

In the starting_with_data.md file, write 3 - 5 new questions that you could answer with this database. For each question, include The queries you used to answer the question The answer to the question

--------------------

Question 1: Calculate the average order value for each visitor:

SQL Queries:

SELECT Vistors.VisitorID, AVG(order_details.UnitPrice * order_details.Quantity) AS AvgOrderValue
FROM Visitors
INNER JOIN Orders ON Visitors.VisitorID = Orders.VisitID
INNER JOIN order_details ON Orders.OrderID = order_details.OrderID
GROUP BY Visitor.VisitID;

Answer:


Question 2: Calculate the total sales for each visitor

SQL Queries:

SELECT Visitor.VisitID, SUM(order_details.UnitPrice * order_details.Quantity) AS TotalSales
FROM Visitors
INNER JOIN Orders ON Visitors.CustomerID = Orders.VisitorID
INNER JOIN order_details ON Orders.OrderID = order_details.OrderID
GROUP BY Visitors.VisitID;

Answer:


Question 3:

SQL Queries:

Answer:


