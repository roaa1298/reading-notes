# Relational database and SQL
- SQL stand for Structured Query Language.
- Some SQL databases: 
   1. SQLite.
   2. MySQL.  
   3. Postgres.  
   4. Oracle.  
   5. Microsoft SQL Server.
- **SQL**: is a language designed to allow both technical and non-technical
   users query, manipulate, and transform data from a relational database.
- Relational database: represents a collection of related (two-dimensional) tabels.
- The **colomns** in the table are called **attributes** or **properties**.
- The **rows** in the table are called **instances**.
- If we want to select all the colomns in the table we use **asterisk**(*) like this:
   > SELECT * FROM tablename;
- But if we want to select a specific colomns we shoulde replace the asterisk symbole
   with the colomn names and split them by comma(,) :
   > SELECT column, another_column, …  
   > FROM tablename; 
- The previous ways return all rows in the table(inefficient way if we had a table
   with a hundred million rows of data), but if we want to return a specific rows
   we need to use a **WHERE** clause in the query. This clause checking specific 
   column values to determine whether it should be included in the results or not.
   > SELECT column, another_column, …  
   > FROM tablename  
   > WHERE condition  
   > AND/OR another_condition  
   > AND/OR …;  
- There is some operators we can use with the numerical data:  
   1. =, !=, < <=, >, >=    **(Standard numerical operators)**    
   2. BETWEEN … AND …   **(Number is within range of two values (inclusive))**    
   3. NOT BETWEEN … AND …  **(Number is not within range of two values (inclusive))**   
   4. IN (…)   **(Number exists in a list)**    
   5. NOT IN (…)  **(Number does not exist in a list)**   
- There is some operators we can use with the text data(Strings):  
  1. =  **(Case sensitive exact string comparison)**  
  2. LIKE  **(Case insensitive exact string comparison)**  
  3. != or <>  **(Case sensitive exact string inequality comparison)**  
  4. NOT LIKE  **(Case insensitive exact string inequality comparison)**  
  5. %  **(Used anywhere in a string to match a sequence of zero or more characters (only with LIKE or NOT LIKE))**  
  6. _  **(Used anywhere in a string to match a single character (only with LIKE or NOT LIKE))**  
  7. IN (…) 	**(String exists in a list)**  
  8. NOT IN (…) 	**(String does not exist in a list)**  
   
