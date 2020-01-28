# SQL stuff

### Important SQL commands
 - [SELECT](###&#32;SELECT) - extracts data from a database
 - [UPDATE](###UPDATE) - updates data in a database
 - [DELETE](###DELETE) - deletes data from a database
 - [INSERT INTO](###INSERT&#32;INTO) - inserts new data into a database
 - [CREATE DATABASE](###CREATE&#32;DATABASE) - creates a new database
 - [ALTER DATABASE](ALTER&#32;DATABASE) - modifies a database
 - [CREATE TABLE](CREATE&#32;TABLE) - creates a new table
 - [ALTER TABLE](ALTER&#32;TABLE) - modifies a table
 - [DROP TABLE](DROP&#32;TABLE) - deletes a table
 - [CREATE INDEX](CREATE&#32;INDEX) - creates an index (search key)
 - [DROP INDEX](DROP&#32;INDEX) - deletes an index

### Extras
 - *`*` will select everything*
 - *SQL is NOT case-sensitive*
 - `null` values `!= 0`: tested with `IS` or `IS NOT`
 - `!=` does not exist in SQL - will use `<>` instead
  
---
### SELECT
 - Returns the selected columns
   - `SELECT col1, col2... FROM table`

### WHERE
 - Used for filtering with a condition
   - `SELECT * FROM <table> WHERE ID=1;`
 - Can be used with `AND`, `OR`, `NOT` to help filter specific results
   - `SELECT * FROM <table> WHERE ID=1 OR NAME='bob'...;`

### SELECT DISTINCT
 - Returns unique values only
   - `SELECT DISTINCT <col> FROM <table_name>`

### ORDER BY (`ASC` or `DESC`)
 - Used to sort the results
   - `SELECT <col> FROM <table_name> ORDER BY <col1>, <col2> DESC`

### SELECT TOP
 - Used to specify number of records to return
 - *Not all database systems support this syntax; MySQL uses `LIMIT` for example*
   - `SELECT TOP number/percent <col_name(s)> FROM <table_name> WHERE condition;`

### MIN and MAX
 - Returns min/max value in the selected column
 - Can add `AS` as a label for the return value
   - `SELECT MIN(<col_name>) AS lowest FROM <table_name> WHERE condition;`

### Aliases
 - Used for giving columns and tables more detailed titles
 - Keyword: `AS`
 - Naming aliases with spaces will require *double quotes*
 - Column aliases and table aliases contain different syntax
 - Naming a column:
   - `SELECT <col_name> AS <alias_name> FROM <table_name>`
 - Naming a table:
   - `SELECT <col_name(s) FROM <table_name> AS <alias_name>`

### COUNT, AVG, and SUM functions
 - `COUNT()` returns number of rows that matches criteria
 - `AVG()` returns average value of a column column
 - `SUM()` returns sum of a numeric column
   - `SELECT COUNT/AVG/SUM(<col_name>) FROM <table_name> WHERE condition;`

### IN
 - Used to specify multiple values in a `WHERE` clause
 - Equivalent to multiple `OR` statements
   - `WHERE <col_name> IN (val1, val2, ...)`

### BETWEEN
 - Selects values in given range; can be numbers, texts, or dates
 - Inclusive operator
 - Dates can be specified as `yyyy-mm-dd` or `#dd/mm/yyyy#` enclosed by 2 hashes
   - `WHERE <col_name> BETWEEN val1 AND val2;`

### INSERT INTO
 - Used to insert new records in a table
 - IDs are not manually inserted - automatically incremented when new records are added
 - Columns that are **not** given a value during `INSERT` will be given `null`
   - `INSERT INTO <table_name> (<col1>, <col2>, ...) VALUES (val1, val2, ...);`

### UPDATE
 - Used to modify existing records in a table
 - if `WHERE` is omitted, *all* values in specified columns will have the same updated value
   - `UPDATE <table_name> SET <col1> = val1, <col2> = val2... WHERE ID=2`

### DELETE
 - Used to remove records in a table
   - `DELETE FROM <table_name> WHERE id=1`
 - To delete all records: `DELETE FROM <table_name>`

### LIKE
 - Used with `WHERE` to search for a *pattern*
 - Conjunctions with the following wildcards: 
   - %: represents zero, one, or multiple characters
   - _: represents a single character
 - | LIKE operator                 | Description                                        |
   |-------------------------------|----------------------------------------------------|
   | WHERE <col_name> LIKE 'a%'    | values that start with 'a'                         |
   | WHERE <col_name> LIKE '%a'    | values that end with 'a'                           |
   | WHERE <col_name> LIKE '%a%'   | values that contain letter 'a'                     |
   | WHERE <col_name> LIKE '_r%'   | values that have 'r' in 2nd position               |
   | WHERE <col_name> LIKE 'a_%_%' | values that start with 'a' and has >= 3 characters |
   | WHERE <col_name> LIKE 'a%o'   | values that start with 'a' and ends with 'o'       |


---
### JOIN
 - Used for combine rows from two or more tables based on a mutually identical column
 - Types of JOIN:
   - `(INNER) JOIN`: returns records that have matching values in both tables
   - `LEFT (OUTER) JOIN`: return *all* records from left table, and matched records from the right table
   - `RIGHT (OUTER) JOIN`: return *all* records from right table, and matched records from the left table
   - `FULL (OUTER) JOIN`: return all records when there's a match on *either* table
 - ![INNER JOIN](https://www.w3schools.com/sql/img_innerjoin.gif) ![LEFT JOIN](https://www.w3schools.com/sql/img_leftjoin.gif) ![RIGHT JOIN](https://www.w3schools.com/sql/img_rightjoin.gif) ![FULL OUTER JOIN](https://www.w3schools.com/sql/img_fulljoin.gif)

### INNER JOIN
 - Selects records that contains matching values in both tables
   - `SELECT <col_name(s)> FROM <table_1> INNER JOIN <table_2> ON table1.col_name = table2.col_name;`

### LEFT/RIGHT JOIN
 - LEFT: Selects all records from left table, and matched records from right table
 - RIGHT: Selects all records from right table, and matched reecords from left table
   - `SELECT <col_name(s)> FROM <table_1> LEFT/RIGHT JOIN <table_2> ON table1.col_name = table2.col_name;`

### FULL JOIN
 - Selects all records when there's a match on either left or right table
   - `SELECT <col_name(s)> FROM <table_1> FULL OUTER JOIN <table_2> ON table1.col_name = table2.col_name`

### SELF JOIN
 - Same as a regular `INNER JOIN` but with itself
   - `SELECT <col_name(s)> FROM <table_1> T1, <table_1 T2 WHERE condition;`

### UNION (ALL)
 - Used to combine the result-set of two or more `SELECT` statements
 - Selects only distinct values - use `UNION ALL` to allow duplication
   - Each `SELECT` statement within `UNION` must have the same number of columns
   - Columns must have similar data types
   - Columns in each `SELECT` statement must also be in the same order
   - `SELECT <col_name(s)> FROM <table_1> UNION (ALL) SELECT <col_name(s)> FROM <table_2>`

### GROUP BY
 - Often used with COUNT, MAX, MIN, SUM, AVG functions to group the result-set by one or more columns
   - `SELECT <col_name(s)> FROM <table_name> WHERE condition GROUP BY <col_name(s) ORDER BY <col_name(s);`

### HAVING
 - Same idea as `WHERE` but for aggregation functions such as COUNT, MAX, MIN...
   - `SELECT <col_name(s)> FROM <table_name> WHERE condition GROUP BY <col_name(s)> HAVING condition ORDER BY <col_name(s)>;`
   - e.g. `SELECT COUNT(id), name FROM <table> GROUP BY name HAVING COUNT(id) > 5;`

### EXISTS
 - Used to test for the existence of any record in a *subquery*
 - Returns boolean (and the table of results that matches the test conditions)
   - `SELECT <col_name(s)> FROM <table_name> WHERE EXISTS (SELECT <col_name> FROM <table_name> WHERE condition);`

### ANY and ALL
 - `ANY`: returns TRUE if *any* of the subquery values meet the condition`
 - `ALL`: returns TRUE if *all* of the subquery values meet the condition`
   - `SELECT <col_name> FROM <table> WHERE id = ANY/ALL (SELECT id FROM <table_2> WHERE condition);`

### SELECT INTO
 - Used for copying data from one table to a **new** table
   - `SELECT <col_name(s)> INTO <new_table>  [IN <db_name>] FROM <old_table> WHERE condition;`

### INSERT INTO SELECT
 - Used for copying data from one table to another
 - Requires data types in source and target tables to match
 - Existing records in the target table remains unaffected
   - `INSERT INTO <table_2> SELECT <col_name(s)> FROM <table_1> WHERE condition;`

### CASE
 - Same as a regular `case` in other languages - Goes through conditions until a condition is met, otherwise returns the `ELSE` clause
    ```
    CASE
        WHEN condition1 THEN result1
        WHEN condition2 THEN result2
        etc..
        ELSE result
    END;
    ```
     
### NULL function
 - `IFNULL()`: allows user to return an alternate function if value == null

### STORED PROCEDURE
 - A function
     ```
     CREATE PROCEDURE name
     AS
     <sql_statements>
     GO;
     ```
 - To run the procedure: `EXEC procedure_name;`


### COMMENTS
 - Use `--` for single line
 - User `/* ... */` for multi-line
