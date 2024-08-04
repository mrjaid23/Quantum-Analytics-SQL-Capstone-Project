# Quantum-Analytics-SQL-Capstone-Project

## This is a SQL database Exploration Project for my Data Analytics Certification

- It contains a set of business problems that requires the retrieval of datasets from the database.

[Project Brief](https://github.com/mrjaid23/Quantum-Analytics-SQL-Capstone-Project/blob/f65fbaa9dc5e08c1dfdcdec6803921619259e70e/QA%20SQL%20Capstone%20Project.docx)

> Solutions 

**Question 1**
```
SELECT ProductID, Name, Color, ListPrice, StandardCost AS Price
FROM Production.Product
WHERE COLOR IS NOT NULL AND COLOR NOT IN ('RED', 'SILVER/BLACK', 'WHITE') AND ListPrice BETWEEN 75 AND 750
ORDER BY ListPrice DESC;
```
**Question 2**
```
SELECT BusinessEntityID, JobTitle, BirthDate, Gender, HireDate
FROM HumanResources.Employee
WHERE (Gender = 'M' AND BirthDate BETWEEN '1962-01-01' AND '1970-12-31' AND HireDate > '2001-12-31') 
OR (Gender = 'F' AND BirthDate BETWEEN '1972-01-01' AND '1975-12-31' AND HireDate BETWEEN '2001-01-01' AND '2002-12-31');
```
**Question 3**
```
SELECT TOP 10 ProductID, Name, Color
FROM Production.Product
WHERE ProductNumber LIKE 'BK%';
```
**Question 4**
```
SELECT BusinessEntityID, FirstName, LastName, EMailAddress, CONCAT(FirstName, ' ', LastName) AS FullName, LEN(CONCAT(FirstName, ' ', LastName)) AS FullNameLength
FROM Person.vAdditionalContactInfo;
```
**Question 5**
```
SELECT PS.ProductSubcategoryID, PS.Name AS ProductSubcategoryName, PP.DaystoManufacture
FROM Production.Product AS PP
INNER JOIN Production.ProductSubcategory AS PS
ON PP.ProductSubcategoryID = PS.ProductSubcategoryID
WHERE PP.DaystoManufacture >= 3;
```
**Question 7**
```
SELECT COUNT(DISTINCT JobTitle) AS DistinctJobTitle
FROM HumanResources.Employee;
```
**Question 8**
```
SELECT BusinessEntityID, BirthDate, HireDate, DATEDIFF(YEAR, BirthDate, HireDate) AS AgeAtHiring
FROM HumanResources.Employee;
```
**Question 9**
```
SELECT COUNT(*) AS NoOfEmployeesDueForAward
FROM HumanResources.Employee
WHERE HireDate <= DATEADD(YEAR, 20, GETDATE()) AND HireDate > DATEADD(YEAR, 25, GETDATE());
```
**Question 10**
```
SELECT BusinessEntityID, BirthDate, HireDate, DATEDIFF(YEAR, BirthDate, HireDate) AS AgeAtHiring, DATEDIFF(YEAR, HireDate, GETDATE()) AS NoOfYearsinService, DATEDIFF(YEAR, BirthDate, HireDate) + DATEDIFF(YEAR, HireDate, GETDATE()) AS CurrentAge,
CASE WHEN DATEDIFF(YEAR, BirthDate, GETDATE()) < 65 THEN 65 - DATEDIFF(YEAR, BirthDate, GETDATE())
        ELSE 0
    END AS YearsUntilSentiment
FROM HumanResources.Employee;
```
**Question 11**
```
-- Create a temporary table to store the new prices and commission
SELECT
    ProductID,
    Name,
    Color,
    ListPrice,
    CASE
        WHEN Color = 'White' THEN ListPrice * 1.08
        WHEN Color = 'Yellow' THEN ListPrice * 0.925
        WHEN Color = 'Black' THEN ListPrice * 1.172
        WHEN Color IN ('Multi', 'Silver', 'Silver/Black', 'Blue') THEN SQRT(ListPrice) * 2
        ELSE ListPrice
    END AS NewPrice,
    CASE
        WHEN Color = 'White' THEN ListPrice * 1.08 * 0.375
        WHEN Color = 'Yellow' THEN ListPrice * 0.925 * 0.375
        WHEN Color = 'Black' THEN ListPrice * 1.172 * 0.375
        WHEN Color IN ('Multi', 'Silver', 'Silver/Black', 'Blue') THEN SQRT(ListPrice) * 2 * 0.375
        ELSE ListPrice * 0.375
    END AS Commission
INTO #TempNewPrices
FROM
    Production.Product;
```

-- Select the results from the temporary table
```
SELECT
    ProductID,
    Name,
    Color,
    ListPrice,
    NewPrice,
    Commission
FROM
    #TempNewPrices;
```
**Question 12**
```
SELECT SS.BusinessEntityID, SS.SalesQuota, PP.BusinessEntityID, PP.FirstName, PP.LastName, HE.BusinessEntityID, HE.HireDate, HE.SickLeaveHours, ST.Name AS Region
FROM Sales.SalesPerson AS SS
INNER JOIN Person.Person AS PP
ON SS.BusinessEntityID = PP.BusinessEntityID
INNER JOIN HumanResources.Employee AS HE
ON SS.BusinessEntityID = HE.BusinessEntityID
INNER JOIN Sales.SalesTerritory AS ST
ON ST.TerritoryID = SS.TerritoryID;
```
