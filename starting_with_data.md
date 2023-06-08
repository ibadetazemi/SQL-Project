Consider the data you have available to you. You can use the data to: - find all duplicate records - find the total number of unique visitors (fullVisitorID) - find the total number of unique visitors by referring sites - find each unique product viewed by each visitor - compute the percentage of visitors to the site that actually makes a purchase

In the starting_with_data.md file, write 3 - 5 new questions that you could answer with this database. For each question, include The queries you used to answer the question The answer to the question

--------------------

--At first glance, I realize what questions that we need to answer next:
--1.Standarize sales date format
--2.Populate Customer Address data
--3.Break Address into individual columns (Address, City, State)
--4.Change Y and N to YES and Number in Column 'Sales'
--5.Remove Duplicates
--6.Delete Unused Columns

Question 1: --Standarize sales date format:

SQL Queries:

SELECT SaleDate, CONVERT (Date, SaleDate)
FROM Project.dbo.products

ALTER TABLE Project.dbo.products
ADD SaleDate2 Date

UPDATE Project.dbo.products
SET SaleDate2 = CONVERT (Date, SaleDate)

SELECT SaleDate, SaleDate2
FROM Project..products

Answer: 



Question 2: --Populate Customer Address data:

SQL Queries:

Answer:



Question 3: --Break Address into individual columns (Address, City, State):

SQL Queries:

Answer:



Question 4: --Change Y and N to YES and No in Column 'Sales':

SQL Queries:

Answer:



Question 5: --Remove Duplicates:

SQL Queries:

Answer:


Question 6: --Delete Unused Columns:

SQL Queries:

Answer:


