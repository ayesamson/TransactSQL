# Transact-SQL Syntax Review

This is a quick T-SQL syntax guide for many of the most frequently used statements.

Index Reference
--
Comments
* [Single Line Comment](https://github.com/ayesamson/TransactSQL#single-line-comment)
* [Multi-Line Comment](https://github.com/ayesamson/TransactSQL#multi-line-block-comment)

Drop Objects
* [Drop Table](https://github.com/ayesamson/TransactSQL#drop-table)
* [Drop Procedure](https://github.com/ayesamson/TransactSQL#drop-procedure)
* [Drop View](https://github.com/ayesamson/TransactSQL#drop-view)
* [Drop Function](https://github.com/ayesamson/TransactSQL#drop-function)

Create Objects
* [Create Table](https://github.com/ayesamson/TransactSQL#create-table)
* [Create Procedure](https://github.com/ayesamson/TransactSQL#create-procedure)
* [Create View](https://github.com/ayesamson/TransactSQL#create-view)
* [Create Function](https://github.com/ayesamson/TransactSQL#create-function)

Insert Data
* [Single Record](https://github.com/ayesamson/TransactSQL#single-record)
* [Multiple Records](https://github.com/ayesamson/TransactSQL#multiple-records)
* [Insert With Select - Single Record](https://github.com/ayesamson/TransactSQL#insert-with-select---single-record)
* [Insert With Select - Multiple Records] (https://github.com/ayesamson/TransactSQL#insert-with-select---multiple-records)
* [Single Record - Single Column] (https://github.com/ayesamson/TransactSQL#single-record---single-column)
* [Insert With Select - Single Record - Single Column](https://github.com/ayesamson/TransactSQL/blob/master/README.md#insert-with-select---single-record---single-column)
* [Multiple Records - Single Column](https://github.com/ayesamson/TransactSQL#multiple-records---multiple-columns)
* [Insert With Select - Multiple Records - Single Column](https://github.com/ayesamson/TransactSQL#insert-with-select---multiple-records---multiple-columns)



Comments 
--
##### Single Line Comment
```SQL
-- This is a single line comment
```
You can also comment out a line using the CTRL+C and then uncomment the same line by pressing CTRL+K

##### Multi-Line (Block) Comment
```SQL
/*
  This is a multi-line (block) comment
  You can easily comment out several lines
  of your code with this.
*/
```

Drop Objects
--
#####Drop Table
```SQL
IF OBJECT_ID('[dbo].[TableName]') IS NOT NULL
  BEGIN
    DROP TABLE [dbo].[TableName];
  END

IF EXISTS(SELECT [o].[Name] FROM [sys].[objects] [o] JOIN [sys].[schemas] [s] ON [o].[schema_id] = [s].[schema_id] WHERE ([o].[Name] = 'TableName' AND [s].[name] = 'dbo' AND [o].[type] = 'U'))
  BEGIN
    DROP TABLE [dbo].[TableName];
  END

DROP TABLE IF EXISTS [dbo].[TableName];
```

#####Drop Procedure
```SQL
IF OBJECT_ID('[dbo].[usp_ProcedureName]') IS NOT NULL
  BEGIN
    DROP PROCEDURE [dbo].[usp_ProcedureName];
  END
GO

IF EXISTS(SELECT [o].[Name] FROM [sys].[objects] [o] JOIN [sys].[schemas] [s] ON [o].[schema_id] = [s].[schema_id] WHERE ([o].[Name] = 'usp_ProcedureName' AND [s].[name] = 'dbo' AND [o].[type] = 'P'))
  BEGIN
    DROP PROCEDURE [dbo].[usp_ProcedureName];
  END
GO

DROP PROCEDURE IF EXISTS [dbo].[usp_ProcedureName];
GO
```

#####Drop View
```SQL
IF OBJECT_ID('[dbo].[vw_ViewName]') IS NOT NULL
  BEGIN
    DROP VIEW [dbo].[vw_ViewName];
  END
GO

IF EXISTS(SELECT [o].[Name] FROM [sys].[objects] [o] JOIN [sys].[schemas] [s] ON [o].[schema_id] = [s].[schema_id] WHERE ([o].[Name] = 'vw_ViewName' AND [s].[name] = 'dbo' AND [o].[type] = 'V'))
  BEGIN
    DROP VIEW [dbo].[vw_ViewName];
  END
GO

DROP VIEW IF EXISTS [dbo].[vw_ViewName];
GO
```

#####Drop Function
```SQL
IF OBJECT_ID('[dbo].[FunctionName]') IS NOT NULL
  BEGIN
    DROP FUNCTION [dbo].[FunctionName];
  END
GO

IF EXISTS(SELECT [o].[Name] FROM [sys].[objects] [o] JOIN [sys].[schemas] [s] ON [o].[schema_id] = [s].[schema_id] WHERE ([o].[Name] = 'FunctionName' AND [s].[name] = 'dbo' AND [o].[type] = 'FN'))
  BEGIN
    DROP FUNCTION [dbo].[FunctionName];
  END
GO

DROP FUNCTION IF EXISTS [dbo].[FunctionName];
GO
```
Create Objects
--
#####Create Table
```SQL
-- With Identity, Primary Key and constraints
CREATE TABLE [dbo].[TableName] (
  id [int] IDENTITY(1,1) CONSTRAINT [pk_TableName_id] PRIMARY KEY
  ,Column1 [varchar](50) NOT NULL
  ,Column2 [varchar](50) NULL
  ,CreateDate [datetime] CONSTRAINT [DF_TableName_CreateDate] DEFAULT (GETDATE())
);
GO
```
#####Create Procedure
```SQL
-- Without parameters
CREATE PROCEDURE [dbo].[usp_ProcessName_GetActiveServers] 
AS
SELECT 
  [ServerName]
FROM [dbo].[Servers]
WHERE ([IsActive] = 1)
ORDER BY 1;
GO
```
##### Usage Example:
```SQL
EXECUTE [dbo].[usp_ProcessName_GetActiveServers];
```

```SQL
-- With Parameter
CREATE PROCEDURE [dbo].[usp_ProcessName_GetActiveServers_ByServerName] (
  @ServerName VARCHAR(50)
)
AS
SELECT 
  [ServerName]
FROM [dbo].[Servers]
WHERE ([ServerName] = @ServerNAme)
ORDER BY 1;
GO
```
##### Usage Example:
```SQL
EXECUTE [dbo].[usp_ProcessName_GetActiveServers_ByServerName] @ServerName = 'Server';
```

```SQL
--With (Optional) Parameter
CREATE PROCEDURE [dbo].[usp_ProcessName_GetActiveServers] (
  @ServerName VARCHAR(50) = NULL
)
AS
IF (@ServerName IS NOT NULL)
  BEGIN
    SELECT 
      [ServerName]
    FROM [dbo].[Servers]
    WHERE ([ServerName] = @ServerName)
    ORDER BY 1;
  END
ELSE
  BEGIN
    SELECT 
      [ServerName]
    FROM [dbo].[Servers]
    WHERE ([IsActive] = 1)
    ORDER BY 1;
  END
GO
```
##### Usage Example:
```SQL
EXECUTE [dbo].[usp_ProcessName_GetActiveServers];
EXECUTE [dbo].[usp_ProcessName_GetActiveServers] @ServerName = 'Server';
```

#####Create View
```SQL
CREATE VIEW [dbo].[vw_ProcessName_ActiveServers]
AS
SELECT 
  [ServerName]
FROM [dbo].[Servers]
WHERE ([IsActive] = 1);
GO
```
##### Usage Example:
```SQL
SELECT [ServerName] FROM [dbo].[vw_ProcessName_ActiveServers];
```


Insert Data
--
##### Single Record
```SQL
INSERT INTO [dbo].[TableName] ([Column1], [Column2]) VALUES ('Value1', 'Value2');
```
##### Multiple Records
```SQL
INSERT INTO [dbo].[TableName] ([Column1], [Column2]) 
VALUES ('Value1', 'Value2')
,('Value3', 'Value4')
,('Value5', 'Value6');
```
##### Insert With Select - Single Record
```SQL
INSERT INTO [dbo].[TableName] ([Column1], [Column2]) 
SELECT 'Value1', 'Value2';
```
##### Insert With Select - Multiple Records
```SQL
INSERT INTO [dbo].[TableName] ([Column1], [Column2]) 
SELECT 'Value1', 'Value2'
UNION
SELECT 'Value3', 'Value4'
UNION
SELECT 'Value5', 'Value6';
```
##### Single Record - Single column
```SQL
INSERT INTO [dbo].[TableName] ([Column1]) VALUES ('Value1');
```
##### Insert With Select - Single Record - Single column
```SQL
INSERT INTO [dbo].[TableName] ([Column1]) 
SELECT 'Value1';
```
##### Multiple Records - Multiple columns
```SQL
INSERT INTO [dbo].[TableName] ([Column1]) 
VALUES ('Value1')
,('Value2')
,('Value3')
,('Value4')
,('Value5')
,('Value6');
```
##### Insert With Select - Multiple Records - Multiple columns
```SQL
INSERT INTO [dbo].[TableName] ([Column1]) 
SELECT 'Value1'
UNION
SELECT 'Value2'
UNION
SELECT 'Value3'
UNION
SELECT 'Value4'
UNION
SELECT 'Value5'
UNION
SELECT 'Value6';
```

Alter Table
--
##### Add Column
```SQL
ALTER TABLE [dbo].[TableName] ADD [Column3] VARCHAR(50) NULL;
```
##### Change Data Type
```SQL
ALTER TABLE [dbo].[TableName] ALTER COLUMN [Column3] NVARCHAR(50) NULL;
```
##### Drop Column
```SQL
ALTER TABLE [dbo].[TableName] DROP COLUMN [Column3];
```
##### Rename Column
```SQL
EXECUTE [dbo].[sp_rename] 'TableName.ColumnName', 'NewColumnName', 'COLUMN';  
```



