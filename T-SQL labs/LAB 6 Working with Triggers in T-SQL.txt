LAB 6: Working with Triggers in T-SQL:

Lab Objective
In this lab, you will learn how to:

Create triggers in T-SQL
Understand how triggers work automatically
Use AFTER triggers for INSERT, UPDATE, DELETE
Use INSTEAD OF triggers to control operations
Work with special tables: inserted and deleted

Scenario

You are working as a database developer for a company.
The business wants to:

Track all changes made to products
Maintain a log of INSERT, UPDATE, DELETE operations
Prevent certain operations when required

Task 1: Create Demo Tables

Step 1: Create Products Table: 

CREATE TABLE ProductsDemo (
    ProductID INT PRIMARY KEY,
    Name VARCHAR(50),
    Price INT
);

Step 2: Create Log Table
CREATE TABLE ProductLogs (
    LogID INT IDENTITY(1,1),
    ProductID INT,
    ActionType VARCHAR(10),
    ActionDate DATETIME
);

Expected Outcome

You now have:

A main table (ProductsDemo)
A logging table (ProductLogs)

Task 2: Create AFTER INSERT Trigger
Step 1: Create Trigger

CREATE TRIGGER trg_AfterInsertProduct
ON ProductsDemo
AFTER INSERT
AS
BEGIN
    INSERT INTO ProductLogs (ProductID, ActionType, ActionDate)
    SELECT ProductID, 'INSERT', GETDATE()
    FROM inserted;
END;

Step 2: Test Trigger

INSERT INTO ProductsDemo VALUES (1, 'Laptop', 50000);

SELECT * FROM ProductLogs;

Expected Outcome
A new row is inserted into ProductsDemo
A corresponding log entry is created automatically

Task 3: Create AFTER DELETE Trigger

Step 1: Create Trigger
CREATE TRIGGER trg_AfterDeleteProduct
ON ProductsDemo
AFTER DELETE
AS
BEGIN
    INSERT INTO ProductLogs (ProductID, ActionType, ActionDate)
    SELECT ProductID, 'DELETE', GETDATE()
    FROM deleted;
END;

Step 2: Test Trigger


DELETE FROM ProductsDemo WHERE ProductID = 1;

SELECT * FROM ProductLogs;

Expected Outcome
The product is deleted
A log entry is created with action type DELETE


Task 4: Create AFTER UPDATE Trigger
Step 1: Insert Data Again
INSERT INTO ProductsDemo VALUES (2, 'Mobile', 20000);

Step 2: Create Trigger

CREATE TRIGGER trg_AfterUpdateProduct
ON ProductsDemo
AFTER UPDATE
AS
BEGIN
    INSERT INTO ProductLogs (ProductID, ActionType, ActionDate)
    SELECT ProductID, 'UPDATE', GETDATE()
    FROM inserted;
END;

Step 3: Test Trigger

UPDATE ProductsDemo
SET Price = 25000
WHERE ProductID = 2;

SELECT * FROM ProductLogs;

Expected Outcome
Product is updated
A log entry is created with action type UPDATE


Task 5: Create INSTEAD OF DELETE Trigger

Step 1: Create Trigger

CREATE TRIGGER trg_InsteadOfDelete
ON ProductsDemo
INSTEAD OF DELETE
AS
BEGIN
    PRINT 'Delete operation is not allowed!';
END;

Step 2: Test Trigger

Expected Outcome
Product is NOT deleted
Message is displayed: Delete operation is not allowed

Triggers make your database smart
it reacts automatically without manual queries




