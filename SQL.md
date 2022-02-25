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
   
- **DISTINCT** : is a keyword we use it to discard rows that have a duplicate column value.  
  > SELECT DISTINCT column, another_column, …  
  > FROM mytable  
  > WHERE condition(s);  
- SQL provides a way to sort the results by a given column in ascending or descending order using the **ORDER BY** clause.  
  > SELECT column, another_column, …  
  > FROM mytable  
  > WHERE condition(s)  
  > ORDER BY column ASC/DESC;  
- There are two clauses which is commonly used with the **ORDER BY** clause (they are useful optimization to indicate to the database the subset of the results we care about):  
  1. **LIMIT**: To reduse the number of rows to return.  
  2. **OFFSET**: (Optional) specify where to begin counting the number rows from.  
   > SELECT column, another_column, …  
   > FROM mytable  
   > WHERE condition(s)  
   > ORDER BY column ASC/DESC  
   > LIMIT num_limit OFFSET num_offset;  
- Database **normalization**: minimizes duplicate data in any single table.  
- The **INNER JOIN** is a process that matches rows from the first table and the second table which have the same key(as defined by the **ON** constraint) to create a result row with the combined columns from both tables.  
  > SELECT column, another_table_column, …  
  > FROM mytable  
  > INNER JOIN another_table   
  >  ON mytable.id = another_table.id  
  > WHERE condition(s)  
  > ORDER BY column, … ASC/DESC  
  > LIMIT num_limit OFFSET num_offset;  
- If we want to insert a new row in a table in the database we need to use **INSERT** statement :  
   > INSERT INTO mytable  
   > VALUES (value_or_expr, another_value_or_expr, …),  
   >    (value_or_expr_2, another_value_or_expr_2, …),  
   >    …;  
- Each row of data that we are filling shoud contain values for every corresponding column in the table and in the same order.  
- We can insert multiple rows at a time by just listing them sequentially.  
- If we want to add values to a specified colomns in the table we can insert rows with only the columns of data we have by specifying them explicitly.  
  > INSERT INTO mytable  
  > (column, another_column, …)  
  > VALUES (value_or_expr, another_value_or_expr, …),  
  >    (value_or_expr_2, another_value_or_expr_2, …),  
  >    …;  
- If we want to update any data in the table we need to use **UPDATE** statement, the statement works by taking multiple column/value pairs, and applying those changes to each and every row that satisfies the constraint in the **WHERE** clause.  
  > UPDATE mytable  
  > SET column = value_or_expr,   
  >  other_column = another_value_or_expr,   
  >  …  
  > WHERE condition;  
- If we want to delete data from a table in the database, we can use a **DELETE** statement:  
   > DELETE FROM mytable  
   > WHERE condition;  
- If we decide to leave out the WHERE constraint, then all rows are removed, which is a quick and easy way to clear out a table completely.  
- We can create a new database table using the **CREATE TABLE** statement.  
  > CREATE TABLE IF NOT EXISTS mytable (  
  >  column DataType TableConstraint DEFAULT default_value,  
  >  another_column DataType TableConstraint DEFAULT default_value,  
  >  …  
  > );  
- Table datatypes and costraints : [datatypes and costraints](https://sqlbolt.com/lesson/creating_tables)
- **table schema**: is defined by The structure of the new table which defines a series of columns. Each column has a name, the type of data allowed in that column, an optional table constraint on values being inserted, and an optional default value.  
- If there already exists a table with the same name, the SQL implementation will usually throw an error, so to suppress the error and skip creating a table if one exists, you can use the **IF NOT EXISTS clause**.  
- SQL provides a way for us to update our corresponding tables and database schemas by using the **ALTER TABLE** statement to add, remove, or modify columns and table constraints.  
- To add a column to any table we need to specify the data type of the column along with any potential table constraints and default values to be applied to both existing and new rows.  
   > ALTER TABLE mytable  
   > ADD column DataType OptionalTableConstraint   
   >  DEFAULT default_value;  
- to remove a colomn from the table :  
    > ALTER TABLE mytable  
    > DROP column_to_be_deleted;  
- If you need to rename the table itself, you can also do that using the RENAME TO clause of the statement.  
   > ALTER TABLE mytable  
   > RENAME TO new_table_name;  
- If we want want to remove an entire table including all of its data and metadata, we can use the **DROP TABLE** statement:  
   > DROP TABLE IF EXISTS mytable;  
- The database may throw an error if the specified table does not exist, and to suppress that error, you can use the **IF EXISTS** clause.  
- ***IF YOU WANT TO READ MORE INFORMATION ABOUT SQL AND DATABASE AND DO SOME EXERCISES SEE THIS WEBSITE [SQL](https://sqlbolt.com/) .***
