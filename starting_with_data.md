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

Answer: There were multiple found.



Question 2: --Populate Customer Address data:

SQL Queries:

SELECT ParcelID, CustomerAddress
FROM Address..customer
ORDER BY ParcelID

SELECT a.ParcelID, a.CustomerAddress, b.ParcelID, b.CustomerAddress,ISNULL(a.CustomerAddress, b.CustomerAddress)
FROM Customer..address a
JOIN Customer..address b
ON a.ParcelID = b.ParcelID
AND a.[UniqueID ]<>b.[UniqueID ]
WHERE a.CustomerAddress IS NULL

UPDATE a
SET PropertyAddress = ISNULL(a.PropertyAddress, b.PropertyAddress)
FROM Customer..address a
JOIN Customer..address b
ON a.ParcelID = b.ParcelID
AND a.[UniqueID ]<>b.[UniqueID ]
WHERE a.CustomerAddress IS NULL

Answer: Some errors were found.



Question 3: --Break Address into individual columns (Address, City, State):

SQL Queries:

SELECT
SUBSTRING(PropertyAddress, 1, CHARINDEX(',',PropertyAddress)-1) as Address
, SUBSTRING(PropertyAddress, CHARINDEX(',',PropertyAddress)+1, LEN(PropertyAddress)) as City
FROM Customer..address

ALTER TABLE Customer..address
ADD PropertyAddressClean Nvarchar (250)

UPDATE Customer..address
SET PropertyAddressClean = SUBSTRING(PropertyAddress, 1, CHARINDEX(',',PropertyAddress)-1)

ALTER TABLE Customer..address
ADD Property_city Nvarchar (250);

UPDATE Customer..address
SET Property_city = SUBSTRING(PropertyAddress, CHARINDEX(',',PropertyAddress)+1, LEN(PropertyAddress))

Answer: Some address information was missing.



Question 4: --Change Y and N to YES and No in Column 'Address':

SQL Queries:

SELECT Distinct( SoldAsVacant), COUNT(SoldAsVacant)
FROM Customer..address
Group by SoldAsVacant
ORDER BY 2

SELECT SoldAsVacant,
CASE
WHEN SoldAsVacant = 'Y' THEN 'Yes'
WHEN SoldAsVacant = 'N' THEN 'No'
ELSE SoldAsVacant
END
FROM Customer..address


UPDATE Customer..address
SET SoldAsVacant=CASE
WHEN SoldAsVacant = 'Y' THEN 'Yes'
WHEN SoldAsVacant = 'N' THEN 'No'
ELSE SoldAsVacant
END

Answer: Helped to filter the data.



Question 5: --Remove Duplicates:

SQL Queries:

--First, we need to identify the duplicates
with RowNumCTE as(
Select *,
ROW_NUMBER () over (
Partition by ParcelID,
VisitorAddress,
LegalReference
Order BY
UniqueID
) as row_num
From Visitor..address
)
Select*
FROM RowNumCTE
WHERE row_num>1

--Let's delete them
with RowNumCTE as(
SELECT *,
ROW_NUMBER () over (
Partition by skuID,
VisitorAddress,
LegalReference
Order BY
UniqueID
) as row_num
From Visitor..address
)
DELETE
FROM RowNumCTE
WHERE row_num>1

Answer: Some duplicates were found.


Question 6: --Delete Unused Columns:

SQL Queries:

Select *
FROM visitor..address

Alter Table Visitor..address
DROP Column visitorAddress, VisitorAddress, 

Answer: Some unused columns were found.


