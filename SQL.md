# SQL stuff

### Important SQL commands
 - [SELECT](###SELECT)    - extracts data from a database
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
  
---
### SELECT
 - Returns the selected columns
 - e.g. `SELECT col1, col2... FROM table`

### WHERE
 - Used for filtering with a condition
 - e.g. `SELECT * FROM <table> WHERE ID=1;`
 - Can be used with `AND`, `OR`, `NOT` to help filter
   - `SELECT * FROM <table> WHERE ID=1 OR NAME='bob'...;`

### SELECT DISTINCT
 - Returns unique values only
 - e.g. `SELECT DISTINCT <col> FROM <table_name>`

### ORDER BY (`ASC` or `DESC`)
 - Used to sort the results
 - e.g. `SELECT <col> FROM <table_name> ORDER BY <col1>, <col2> DESC`

### SELECT TOP
 - Used to specify number of records to return
 - *Not all database systems support this syntax; MySQL uses `LIMIT` for example*
 - e.g. `SELECT TOP number/percent <col_name(s)> FROM <table_name> WHERE condition;`

### MIN and MAX
 - Returns min/max value in the selected column
 - Can add `AS` as a label for the return value
 - e.g. `SELECT MIN(<col_name>) AS lowest FROM <table_name> WHERE condition;`

### COUNT, AVG, and SUM functions
 - `COUNT()` returns number of rows that matches criteria
 - `AVG()` returns average value of a column column
 - `SUM()` returns sum of a numeric column
 - e.g. `SELECT COUNT/AVG/SUM(<col_name>) FROM <table_name> WHERE condition;`

### IN
 - Used to specify multiple values in a `WHERE` clause
 - Equivalent to multiple `OR` statements
 - e.g. `WHERE <col_name> IN (val1, val2, ...)`

### BETWEEN
 - Selects values in given range; can be numbers, texts, or dates
 - Inclusive operator
 - Dates can be specified as `yyyy-mm-dd` or `#dd/mm/yyyy#` enclosed by 2 hashes
 - e.g. `WHERE <col_name> BETWEEN val1 AND val2;`

### INSERT INTO
 - Used to insert new records in a table
 - IDs are not manually inserted - automatically incremented when new records are added
 - Columns that are **not** given a value during `INSERT` will be given `null`
 - e.g. `INSERT INTO <table_name> (<col1>, <col2>, ...) VALUES (val1, val2, ...);`

### UPDATE
 - Used to modify existing records in a table
 - if `WHERE` is omitted, *all* values in specified columns will have the same updated value
 - e.g. `UPDATE <table_name> SET <col1> = val1, <col2> = val2... WHERE ID=2`

### DELETE
 - Used to remove records in a table
 - e.g. `DELETE FROM <table_name> WHERE id=1`
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
