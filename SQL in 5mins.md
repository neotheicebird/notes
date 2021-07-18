`SELECT * FROM table_name` - Gets all data in a table. Here `SELECT` keyword tells Database that we are querying.

`CREATE TABLE table_name (
column_1 datatype,
column_2 datatype
)` - Used to create a table.

Common datatypes - `INTEGER`, `DATE`, `NULL`, `TEXT`

`INSERT INTO table_name (column_name, column_name) VALUES (value_1, value_2)` - Insert row

`UPDATE table_name
SET column_name = 'value'
WHERE condition = condition_value
`

e.g `UPDATE celebs
SET twitter_handler = '@vijayanna'
WHERE id = 4` this updates a row

`DELETE table_name
WHERE condition = condition_value` - Deletes rows in a table.

constraints can be added to columns that makes sure that an `insert` is rejected/adjusted if the data inserted doesn't meet the constraints.

```
CREATE TABLE celebs (   
id INTEGER PRIMARY KEY,    
name TEXT UNIQUE,   
date_of_birth TEXT NOT NULL,   
date_of_death TEXT DEFAULT 'Not Applicable');
```

Here `PRIMARY KEY`, `NOT NULL` and `DEFAULT 'value'` are constraints.

`BETWEEN` operator can work on integers to specify range from A to B (included), or it can work on strings, where it checks for text that `begins with` X upto Y (not included). 

e.g. `SELECT name FROM movies WHERE name BETWEEN 'A' AND 'C'` would return all movie titles starting with A and B. if a movie name is itselt "C" then that would be returned.