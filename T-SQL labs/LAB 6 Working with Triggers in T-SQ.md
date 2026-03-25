LAB 6: Working with Triggers in T-SQL:



Lab Objective

In this lab, you will learn how to:



* Create triggers in T-SQL
* Understand how triggers work automatically
* Use AFTER triggers for INSERT, UPDATE, DELETE
* Use INSTEAD OF triggers to control operations
* Work with special tables: inserted and deleted



Scenario



You are working as a database developer for a company.

The business wants to:



* Track all changes made to products
* Maintain a log of INSERT, UPDATE, DELETE operations
* Prevent certain operations when required



Task 1: Create Demo Tables



Step 1: Create Products Table: 



CREATE TABLE ProductsDemo (

&#x20;   ProductID INT PRIMARY KEY,

&#x20;   Name VARCHAR(50),

&#x20;   Price INT

);



Step 2: Create Log Table

CREATE TABLE ProductLogs (

&#x20;   LogID INT IDENTITY(1,1),

&#x20;   ProductID INT,

&#x20;   ActionType VARCHAR(10),

&#x20;   ActionDate DATETIME

);



Expected Outcome



You now have:



* A main table (ProductsDemo)
* A logging table (ProductLogs)



Task 2: Create AFTER INSERT Trigger

Step 1: Create Trigger



CREATE TRIGGER trg\_AfterInsertProduct

ON ProductsDemo

AFTER INSERT

AS

BEGIN

&#x20;   INSERT INTO ProductLogs (ProductID, ActionType, ActionDate)

&#x20;   SELECT ProductID, 'INSERT', GETDATE()

&#x20;   FROM inserted;

END;



Step 2: Test Trigger



INSERT INTO ProductsDemo VALUES (1, 'Laptop', 50000);



SELECT \* FROM ProductLogs;



Expected Outcome

* A new row is inserted into ProductsDemo
* A corresponding log entry is created automatically



Task 3: Create AFTER DELETE Trigger



Step 1: Create Trigger

CREATE TRIGGER trg\_AfterDeleteProduct

ON ProductsDemo

AFTER DELETE

AS

BEGIN

&#x20;   INSERT INTO ProductLogs (ProductID, ActionType, ActionDate)

&#x20;   SELECT ProductID, 'DELETE', GETDATE()

&#x20;   FROM deleted;

END;



Step 2: Test Trigger





DELETE FROM ProductsDemo WHERE ProductID = 1;



SELECT \* FROM ProductLogs;



Expected Outcome

The product is deleted

A log entry is created with action type DELETE





Task 4: Create AFTER UPDATE Trigger

Step 1: Insert Data Again

INSERT INTO ProductsDemo VALUES (2, 'Mobile', 20000);



Step 2: Create Trigger



CREATE TRIGGER trg\_AfterUpdateProduct

ON ProductsDemo

AFTER UPDATE

AS

BEGIN

&#x20;   INSERT INTO ProductLogs (ProductID, ActionType, ActionDate)

&#x20;   SELECT ProductID, 'UPDATE', GETDATE()

&#x20;   FROM inserted;

END;



Step 3: Test Trigger



UPDATE ProductsDemo

SET Price = 25000

WHERE ProductID = 2;



SELECT \* FROM ProductLogs;



Expected Outcome

* Product is updated
* A log entry is created with action type UPDATE





Task 5: Create INSTEAD OF DELETE Trigger



Step 1: Create Trigger



CREATE TRIGGER trg\_InsteadOfDelete

ON ProductsDemo

INSTEAD OF DELETE

AS

BEGIN

&#x20;   PRINT 'Delete operation is not allowed!';

END;



Step 2: Test Trigger



Expected Outcome

* Product is NOT deleted
* Message is displayed: Delete operation is not allowed



**Triggers make your database smart**

**it reacts automatically without manual queries**


**Create a trigger:**

When new product is added
If price < 0 → block it”

(Hint: use INSTEAD OF INSERT)








