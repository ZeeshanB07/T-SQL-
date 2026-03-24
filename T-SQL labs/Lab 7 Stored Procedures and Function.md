Lab 7 : Stored Procedures and Functions in T-SQL



Objective



In this lab, you will learn how to:



* Create and execute Stored Procedures
* Use parameters in procedures
* Create Scalar Functions
* Create Table-Valued Functions
* Use functions in queries



**What is a Stored Procedure**?



A Stored Procedure is a saved SQL program that can:



* Accept inputs (parameters)
* Execute logic
* Return results



&#x20;**Think of it as a function in programming.**



**What is a Function?**



A Function:



* Takes input
* Returns a value (or table)



&#x20;**Think of it as a calculator**



**PART 1: Create a Stored Procedure:**



**Step 1: Create a basic stored procedure**



CREATE PROCEDURE SalesLT.up\_GetTopProducts

AS

SELECT TOP (10) Name, ListPrice

FROM SalesLT.Product

GROUP BY Name, ListPrice

ORDER BY ListPrice DESC;



**Explanation**

* CREATE PROCEDURE → defines procedure
* TOP (10) → returns top 10 products
* ORDER BY DESC → highest price first



**Step 2: Execute the procedure**

EXECUTE SalesLT.up\_GetTopProducts;



**Explanation**

EXECUTE runs the stored procedure





**PART 2: Stored Procedure with Parameters**

**Step 3: Modify procedure to accept input**



ALTER PROCEDURE SalesLT.up\_GetTopProducts (@count int)

AS

SELECT TOP (@count) Name, ListPrice

FROM SalesLT.Product

GROUP BY Name, ListPrice

ORDER BY ListPrice DESC;



**Explanation**

@count → input parameter

TOP (@count) → dynamic value



**Step 4: Execute with parameter**



EXECUTE SalesLT.up\_GetTopProducts @count = 20;



**PART 3: Create a Scalar Function**

**Step 5: Create discount function**



CREATE FUNCTION SalesLT.fn\_ApplyDiscount 

(@productID int, @percentage decimal)

RETURNS money

AS

BEGIN

&#x20;   DECLARE @discountedPrice money;



&#x20;   SELECT @discountedPrice = 

&#x20;       ListPrice - (ListPrice \* (@percentage / 100))

&#x20;   FROM SalesLT.Product

&#x20;   WHERE ProductID = @productID;



&#x20;   RETURN @discountedPrice;

END;



**Explanation**

* RETURNS money → function returns a value
* DECLARE → variable
* Logic → calculates discounted price
* RETURN → final output





**Step 6: Use the function**



SELECT ProductID, Name, ListPrice, StandardCost,

&#x20;      SalesLT.fn\_ApplyDiscount(ProductID, 10) AS SalePrice

FROM SalesLT.Product;



**Explanation**

Function is used like a column

Runs for each row



**PART 4: Table-Valued Function**

**Step 7: Create table function**



CREATE FUNCTION SalesLT.fn\_ProductProfit (@categoryID int)

RETURNS TABLE

AS

RETURN

(

&#x20;   SELECT ProductID, Name AS Product,

&#x20;          ListPrice, StandardCost,

&#x20;          ListPrice - StandardCost AS Profit

&#x20;   FROM SalesLT.Product

&#x20;   WHERE ProductCategoryID = @categoryID

);



**Explanation**

* Returns a full table
* Used like a normal table



**Step 8: Use the function**

SELECT \* 

FROM SalesLT.fn\_ProductProfit(18);





**Explanation**

* Pass category ID
* Get product profit details



**PART 5: Using CROSS APPLY**

**Step 9: Advanced usage**



SELECT c.Name AS Category, pm.Product, pm.ListPrice, pm.Profit

FROM SalesLT.ProductCategory AS c

CROSS APPLY SalesLT.fn\_ProductProfit(c.ProductCategoryID) AS pm

ORDER BY Category, Product;



**Explanation**

* CROSS APPLY runs function for each row
* Combines results dynamically





**Key Takeaways**

* Stored procedures help reuse logic
* Functions simplify calculations
* Table-valued functions act like virtual tables
* CROSS APPLY enables advanced querying



