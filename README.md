# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
(To create and analyze total revenue generated, sales performance report + to analyze and clean data + find risk areas, identify potential issues + to understand the website visitor buying behaviour, time spent on site and product category.)

## Process
### (Step 1: Loading csv Files into Database)
### (Step 2: Data Cleaning)
##  (Part 3: Starting with Questions)
### (Step 4: Starting with Data)
### (Step 3: Remove irrelevant, redundant, or duplicate data)
### (Step 3: Clean “structural” issues)
### (Step 4: Type conversion)
### (Step 5: Clean missing data)
### (Step 6: Clean outliers)
### (Step 7: Validate)
### (Step 8: QA Process)
### (Step 8: Results)
### (Step 9: Conclusion)
### (Step 10: Future Goals)

## (Part 1: Loading csv Files into Database)
I had created a database first, then I had created tables/columns and I had also added details for each column/alter column + import csv.

COPY characters
FROM ''
DELIMITER ','
CSV HEADER;

## (Part 2:  Data Cleaning)  
I had took these steps to clean data: removed: (irrelevant, redundant, or duplicate data, Clean “structural” issues, Type conversion, Clean missing data, Clean outliers, Validate).

## (Part 3: Starting with Questions)
I had answered the 5 questions as well as included the querry used that shows the steps I took to get the answer.

## (Part 4: Starting with Data)
I had created and answered 3 questions that could help with the database.

---

##Alter table 

ALTER TABLE analytics ALTER COLUMN timeonsite TYPE VARCHAR USING LPAD(timeonsite::VARCHAR, 6, '0');

##Update time on site

UPDATE analytics SET timeonsite = CONCAT( SUBSTRING(timeonsite, 1, 2), ':', SUBSTRING(timeonsite, 3, 2), ':', SUBSTRING(timeonsite, 5, 2) );

##Update time on site

UPDATE analytics SET timeonsite = TIME '00:00:00' + INTERVAL '1 hour' * CAST(SUBSTR(timeonsite, 1, 2) AS INTEGER) + INTERVAL '1 minute' * CAST(SUBSTR(timeonsite, 4, 2) AS INTEGER) + INTERVAL '1 second' * CAST(SUBSTR(timeonsite, 7, 2) AS INTEGER);

##Alter table

ALTER TABLE analytics ALTER COLUMN timeonsite TYPE TIME USING timeonsite::TIME;


##Updating all_sessions to no longer null

update all_sessions set city = NULL where city = '(not set)'


##Time On site: (Checking the time on site)

ALTER TABLE analytics
ALTER COLUMN timeonsite
TYPE VARCHAR
USING LPAD(timeonsite::VARCHAR, 6, '0');

UPDATE analytics
SET timeonsite = CONCAT(
    SUBSTRING(timeonsite, 1, 2),
    ':',
    SUBSTRING(timeonsite, 3, 2),
    ':',
    SUBSTRING(timeonsite, 5, 2)
);

UPDATE analytics
SET timeonsite = TIME '00:00:00' + 
           INTERVAL '1 hour' * CAST(SUBSTR(timeonsite, 1, 2) AS INTEGER) +
           INTERVAL '1 minute' * CAST(SUBSTR(timeonsite, 4, 2) AS INTEGER) +
           INTERVAL '1 second' * CAST(SUBSTR(timeonsite, 7, 2) AS INTEGER);

ALTER TABLE analytics
ALTER COLUMN timeonsite TYPE TIME USING timeonsite::TIME;

##Joining tables

SELECT Visitors.VisitorName, Products.ProductsID
FROM Visitors
LEFT JOIN Orders ONVisitors.VisitorID = Orders.VisitorID
ORDER BY Visitor.VisitorName;
    
## Results
(Fill in what you discovered this data could tell you: What I had discovered with this data is that it was not clean data, it took some time to re-organize the data and transform the data and clean/filter the data. It was challenging to manually add the columns and then try to apply different queries to adhere to each column. And USA made the most revenue and organic search was the highest lead. There was no clear pattern on time spent on site.)
And how you used the data to answer those questions: I had used the data to answer these questions by analyzing the data and interpreting the data)

## Challenges 
(Some challenges that I had experienced include: Null values, having to change time, and having to manually add columns as well as numerous transformations and data cleaning. There was also missing values, Inconsistent data that creates confusion, incorrect data, value entered in the wrong field, Errors in columns/tables and missing/null values. The PGAdmin app also crashes and the lack/limited data for the columns.)

## Future Goals
(What would you do if you had more time?)
(Answer: If I had more time I would go more in-depth with the data cleaning process, transformations, troubleshooting, and educate others about what I had learned and try to learn more.)
