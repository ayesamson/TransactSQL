# Transact-SQL Syntax Review

This is a quick T-SQL syntax guide for many of the most frequently used statements.

Comments 
--
-- This is a single line comment

You can also comment out a line using the CTRL+C and then uncomment the same line by pressing CTRL+K

/*
  
  This is a block (multi-line) comment

  You can easily comment out several lines

  of your code with this.

*/

Create Table
--
```SQL
CREATE TABLE [dbo].[TableName] (
  id [int] IDENTITY(1,1) CONSTRAINT [pk_TableName_id] PRIMARY KEY
  ,Column1 [varchar](50) NOT NULL
  ,Column2 [varchar](50) NULL
  ,CreateDate [datetime] CONSTRAINT [DF_TableName_CreateDate] DEFAULT (GETDATE())
);
```
Create Procedure
--
Without parameters

```SQL
CREATE PROCEDURE [dbo].[usp_ProcessName_GetActiveServers] 
AS
SELECT 
  [ServerName]
FROM [dbo].[Servers]
WHERE ([IsActive] = 1)
ORDER BY 1;
```
##### Example:
```SQL
EXECUTE [dbo].[usp_ProcessName_GetActiveServers];
```

With Parameters

```SQL
CREATE PROCEDURE [dbo].[usp_ProcessName_GetActiveServers_ByServerName] (
  @ServerName VARCHAR(50)
)
AS
SELECT 
  [ServerName]
FROM [dbo].[Servers]
WHERE ([ServerName] = @ServerNAme)
ORDER BY 1;
```
##### Example:
```SQL
EXECUTE [dbo].[usp_ProcessName_GetActiveServers_ByServerName] @ServerName = 'Server';
```

With (Optional) Parameter

```SQL
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
```
##### Example:
```SQL
EXECUTE [dbo].[usp_ProcessName_GetActiveServers];
EXECUTE [dbo].[usp_ProcessName_GetActiveServers] @ServerName = 'Server';
```

Create View
--
```SQL
CREATE VIEW [dbo].[vw_ProcessName_ActiveServers]
AS
SELECT 
  [ServerName]
FROM [dbo].[Servers]
WHERE ([IsActive] = 1);
```
##### Example:
```SQL
SELECT [ServerName] FROM [dbo].[vw_ProcessName_ActiveServers];
```
