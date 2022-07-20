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

Alter Table
* [Add Column](https://github.com/ayesamson/TransactSQL#add-column)
* [Change Data Type](https://github.com/ayesamson/TransactSQL#change-data-type)
* [Drop Column](https://github.com/ayesamson/TransactSQL#drop-column)
* [Rename Column](https://github.com/ayesamson/TransactSQL#rename-column)

Insert Data
* [Single Record](https://github.com/ayesamson/TransactSQL#single-record)
* [Multiple Records](https://github.com/ayesamson/TransactSQL#multiple-records)
* [Insert With Select - Single Record](https://github.com/ayesamson/TransactSQL#insert-with-select---single-record)
* [Insert With Select - Multiple Records] (https://github.com/ayesamson/TransactSQL#insert-with-select---multiple-records)
* [Single Record - Single Column] (https://github.com/ayesamson/TransactSQL#single-record---single-column)
* [Insert With Select - Single Record - Single Column](https://github.com/ayesamson/TransactSQL/blob/master/README.md#insert-with-select---single-record---single-column)
* [Multiple Records - Single Column](https://github.com/ayesamson/TransactSQL#multiple-records---multiple-columns)
* [Insert With Select - Multiple Records - Single Column](https://github.com/ayesamson/TransactSQL#insert-with-select---multiple-records---multiple-columns)

Select Data
* [Simple Select](https://github.com/ayesamson/TransactSQL#simple-select)
* [Select Case](https://github.com/ayesamson/TransactSQL#select-case)
* [CTE](https://github.com/ayesamson/TransactSQL#cte)
* [Select Into](https://github.com/ayesamson/TransactSQL#select-into)
* [Inner Join](https://github.com/ayesamson/TransactSQL#inner-join)
* [Outer Join](https://github.com/ayesamson/TransactSQL#outer-join)
* [Left Join](https://github.com/ayesamson/TransactSQL#left-join)
* [Right Join](https://github.com/ayesamson/TransactSQL#right-join)
* [Windows Function](https://github.com/ayesamson/TransactSQL#windows-function)
* [Coalesce](https://github.com/ayesamson/TransactSQL#coalesce)
* [Max](https://github.com/ayesamson/TransactSQL#max)
* [Parsename](https://github.com/ayesamson/TransactSQL#parsename)
* [Charindex](https://github.com/ayesamson/TransactSQL#charindex)
* [Left](https://github.com/ayesamson/TransactSQL#left)
* [Len](https://github.com/ayesamson/TransactSQL#len)
* [Right](https://github.com/ayesamson/TransactSQL#right)

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
[Top](https://github.com/ayesamson/TransactSQL#index-reference)

Drop Objects
--
##### Drop Table
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

##### Drop Procedure
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

##### Drop View
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

##### Drop Function
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
[Top](https://github.com/ayesamson/TransactSQL#index-reference)

Create Objects
--
##### Create Table
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
##### Create Procedure
```SQL
-- WITHOUT PARAMETERS
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
-- WITH PARAMETER
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
-- WITH (OPTIONAL) PARAMETER
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

#####Create Function
```SQL
CREATE FUNCTION [dbo].[fn_ProcessName_ReturnServerID] (
  @Server VARCHAR(50)
)
RETURNS INT
AS
DECLARE @serverid INT
SELECT 
  @serverid = [id]
FROM [dbo].[Servers]
WHERE ([ServerName] = @Server);

IF (@serverid IS NOT NULL)
  BEGIN
    RETURN @serverid
  END
ELSE
  BEGIN
    RETURN 0
  END
GO
```
##### Usage Example:
```SQL
SELECT [dbo].[fn_ProcessName_ReturnServerID]([ServerName]) FROM [dbo].[vw_ProcessName_ActiveServers];
```
[Top](https://github.com/ayesamson/TransactSQL#index-reference)

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
[Top](https://github.com/ayesamson/TransactSQL#index-reference)

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
##### Multiple Records - Single columns
```SQL
INSERT INTO [dbo].[TableName] ([Column1]) 
VALUES ('Value1')
,('Value2')
,('Value3')
,('Value4')
,('Value5')
,('Value6');
```
##### Insert With Select - Multiple Records - Single columns
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
[Top](https://github.com/ayesamson/TransactSQL#index-reference)

Select Data
--
##### Simple Select
```SQL
SELECT 
  [Column1]
  ,[Column2]
  ,[Column3]
  ,[Column4]
FROM [dbo].[TableName];
```
##### Select Case
```SQL
SELECT 
  ,[Column1]
  ,CASE 
    WHEN [Column2] = 0 THEN 'No'
    ELSE 'Yes'
  END AS [IsActive]
  ,[Column3]
FROM [dbo].[TableName];
```
##### CTE
```SQL
WITH [full] (dname, dlbd)
AS
(
	SELECT 
		[d].[name]
		,MAX([b].[backup_finish_date])
	FROM [master].[sys].[databases] [d]
	JOIN [msdb].[dbo].[backupset] [b] ON [d].[name] = [b].[database_name]
	WHERE ([b].[type] = 'D')
	AND ([b].[is_copy_only] = 0)
	GROUP BY [d].[name],[b].[type]
),[diff] (iname, ilbd)
AS
(
	SELECT 
		[d].[name]
		,MAX([b].[backup_finish_date])
	FROM [master].[sys].[databases] [d]
	JOIN [msdb].[dbo].[backupset] [b] ON [d].[name] = [b].[database_name]
	WHERE ([b].[type] = 'I')
	AND ([b].[is_copy_only] = 0)
	GROUP BY [d].[name],[b].[type]
),[log] (lname, llbd)
AS
(
	SELECT 
		[d].[name]
		,MAX([b].[backup_finish_date])
	FROM [master].[sys].[databases] [d]
	JOIN [msdb].[dbo].[backupset] [b] ON [d].[name] = [b].[database_name]
	WHERE ([b].[type] = 'L')
	AND ([b].[is_copy_only] = 0)
	GROUP BY [d].[name],[b].[type]
)
SELECT 
	[full].[dname] AS 'Database'
	,[full].[dlbd] AS 'Last Full'
	,[diff].[ilbd] AS 'Last Differential'
	,[log].[llbd]  AS 'Last Log'
	,[state_desc]
FROM [full]
JOIN [sys].[databases] [d] ON [full].[dname] = [d].[name]
LEFT JOIN [diff] ON [full].[dname] = [diff].[iname]
LEFT JOIN [log] ON [full].[dname] = [log].[lname]
ORDER BY 1,2;
```
Database | Last Full | Last Diff | Last Log | State
---|---|---|---|---|
db1 |2016-12-21 21:01:37.000|2016-07-31 18:00:23.000|NULL|ONLINE
db2 |2016-12-21 21:00:55.000|2016-07-31 18:00:28.000|NULL|ONLINE
db3 |2016-12-21 21:03:35.000|2016-07-31 18:00:27.000|2016-12-22 15:31:03.000|ONLINE

##### Select Into
This will create a table called [dbo].[TableName2] and copy the specified columns from [dbo].[TableName]
```SQL
SELECT 
  [Column1]
  ,[Column2]
  ,[Column3]
  ,[Column4]
INTO [dbo].[TableName2]  
FROM [dbo].[TableName];
```
##### Inner Join
This will return only the matching rows from both tables based on the joining column
```SQL
SELECT 
  [t1].[Column1]
  ,[t1].[Column2]
  ,[t1].[Column3]
  ,[t1].[Column4]
  ,[t2].[Column1]
  ,[t2].[Column2]
FROM [dbo].[TableName] [t1]
JOIN [dbo].[TableName2] [t2] ON [t1].[ColumnID] = [t2].[ColumnID];
```
##### Outer Join
This will return all rows from both tables
```SQL
SELECT 
  [t1].[Column1]
  ,[t1].[Column2]
  ,[t1].[Column3]
  ,[t1].[Column4]
  ,[t2].[Column1]
  ,[t2].[Column2]
FROM [dbo].[TableName] [t1]
OUTER JOIN [dbo].[TableName2] [t2] ON [t1].[ColumnID] = [t2].[ColumnID];
```
##### Left Join
This will return all rows from the left table and the matching rows from the right
```SQL
SELECT 
  [t1].[Column1]
  ,[t1].[Column2]
  ,[t1].[Column3]
  ,[t1].[Column4]
  ,[t2].[Column1]
  ,[t2].[Column2]
FROM [dbo].[TableName] [t1]
LEFT JOIN [dbo].[TableName2] [t2] ON [t1].[ColumnID] = [t2].[ColumnID];
```
##### Right Join
This will return all rows from the right table and the matching rows from the left
```SQL
SELECT 
  [t1].[Column1]
  ,[t1].[Column2]
  ,[t1].[Column3]
  ,[t1].[Column4]
  ,[t2].[Column1]
  ,[t2].[Column2]
FROM [dbo].[TableName] [t1]
RIGHT JOIN [dbo].[TableName2] [t2] ON [t1].[ColumnID] = [t2].[ColumnID];
```
##### Windows Function
Add row number to result set 
```SQL
SELECT
	ROW_NUMBER() OVER (ORDER BY Name) AS RowID
	,[name]
	,[recovery_model_desc]
FROM [sys].[databases]
WHERE ([database_id] < 5);
```
RowID|name|recovery_model_desc
---|---|---
1|master|SIMPLE
2|model|FULL
3|msdb|SIMPLE
4|tempdb|SIMPLE

Add row number to result set and restart the numbering when the recovery model changes
```SQL
SELECT
	ROW_NUMBER() OVER (PARTITION BY [recovery_model_desc] ORDER BY [name] ASC) AS RowID
	,[name]
	,[recovery_model_desc]
FROM [sys].[databases]
WHERE ([database_id] < 5);
```
RowID|name|recovery_model_desc
---|---|---
1|model|FULL
1|master|SIMPLE
2|msdb|SIMPLE
3|tempdb|SIMPLE

##### Coalesce
```SQL
DECLARE @cmd VARCHAR(4000)
SELECT @cmd =
COALESCE(@cmd, '') + 'KILL ' + CAST([spid] AS VARCHAR(10)) + '; '
FROM [sys].[sysprocesses]
WHERE ([spid] > 50);
SELECT @cmd
```
(no column name) |
---|
KILL 51; KILL 52; KILL 53; KILL 64; KILL 66; KILL 67; |

##### Max
```SQL
-- IDENTIFY DROPPED/MISSING DATABASES
SELECT 
	[database_name]
	,MAX([backup_finish_date]) AS [LastBackupDate]
FROM [msdb].[dbo].[backupset]
WHERE [database_name] NOT IN (
	SELECT 
		[name] 
	FROM [sys].[databases]
) GROUP BY [database_name]
```

database_name|LastBackupDate
---|---
db1|2015-03-11 16:02:28.000
db2|2015-03-11 16:02:46.000
db3|2015-03-11 16:02:49.000
db4|2015-03-11 16:02:43.000

##### Parsename
```SQL
-- PERIODS (.) ARE THE KEY HERE
DECLARE @ip VARCHAR(25)
SET @ip = '192.168.1.10'
SELECT 
	PARSENAME(@ip,4) AS [Octet1]
	,PARSENAME(@ip,3) AS [Octet2]
	,PARSENAME(@ip,2) AS [Octet3]
	,PARSENAME(@ip,1) AS [Octet4];
```
Octet1|Octet2|Octet3|Octet4
---|---|---|---
192|168|1|10

##### Charindex
```SQL
DECLARE @SqlInstance VARCHAR(50)
SET @SqlInstance = 'Server\InstanceName'
SELECT 
	CHARINDEX('\',@SqlInstance)
```
(no column name) |
---|
7 |

##### Left
```SQL
DECLARE @SqlInstance VARCHAR(50)
SET @SqlInstance = 'Server\InstanceName'
SELECT 
	CHARINDEX('\',@SqlInstance)
	,LEFT(@SqlInstance,CHARINDEX('\',@SqlInstance)-1)
```
(no column name) |(no column name) 
---|---
7 |Server

##### Len
```SQL
DECLARE @SqlInstance VARCHAR(50)
SET @SqlInstance = 'Server\InstanceName'
SELECT 
	CHARINDEX('\',@SqlInstance)
	,LEFT(@SqlInstance,CHARINDEX('\',@SqlInstance)-1)
	,LEN(@SqlInstance)
```
(no column name)|(no column name)|(no column name)
---|---|---
7 |Server|19

##### Right
```SQL
DECLARE @SqlInstance VARCHAR(50)
SET @SqlInstance = 'Server\InstanceName'
SELECT 
	CHARINDEX('\',@SqlInstance)
	,LEFT(@SqlInstance,CHARINDEX('\',@SqlInstance)-1)
	,LEN(@SqlInstance)
	,RIGHT(@SqlInstance,(LEN(@SqlInstance) - CHARINDEX('\',@SqlInstance)))
```
(no column name)|(no column name)|(no column name)|(no column name)
---|---|---|---
7 |Server|19|InstanceName

[Top](https://github.com/ayesamson/TransactSQL#index-reference)
