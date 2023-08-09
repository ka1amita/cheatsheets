# MySQL <a name="top"></a>

## Syntax

* **statemnents**
  * **queries** are statements with `SELECT` 

* <i>**not** case sensitive</i>
* `;` and of statement
* `-- comment ` line comment
* `/* comment */` multiline comment

* `AS <alias>` can be imitted
* `WITH <alias>`

## Column Datatypes

* `INT`
  * `UNSIGNED`
* `DOUBLE`
* `CHAR(size)`, `VARCHAR(max_size)` e.g. `VARCHAR(255)`
* `TEXT`
* ... many more

### Column Modifiers
  
* `NOT NULL`
* `AUTO_INCREMENT`
* `DEFAULT <value>`
* `PRIMARY KEY(<id>)` goes at the end of declaration
* `FOREIGN KEY (band_id) REFERENCES <TABLE>(<id>)` as above

### chatching errors

* `IF EXISTS`
* `IF NOT EXISTS`

### *CRUD* Keywords

* `CREATE` vs `DROP`
  * `TABLE` <table>
  * `DATABASE` <databese> == `SCHEMA` <schema>
* `ALTER TABLE` <table>
  * `ADD <column>`
  * `RENAME TO` <new_table>
* `UPDATE` <table>
  * `SET` <column> = <value>
    * `WHERE` <consition>
  * `RENAME TO` <new_table>
* `DELETE FROM` <table>

> If you decide to **leave out** the `WHERE` constraint, then **all** rows are removed, which is a quick and easy way to clear out a table completely (if intentional).

### Other Clasuse Keywords

* `ORDER BY` <column>
  * `ASC`, `DESC`
* `LIMIT` <number>

### Operators

* `=`
* `!=` or `<>`
* `LIKE '<pattern>'` accepts `%` wildcart
* `OR`, `AND`
* `CASE` <variable> `WHEN` <condition> `THEN` <value> `ELSE` <value> `END` don't forget the `END`! there is alse `CASE` *Statement*
* ... many more

### Joins

* `FROM <table1>,<table2> WHERE <condition>` corresponds to **cross product**; deprecated in favour of `JOIN`s
* `JOIN`s
  * `INNER JOIN`  == `JOIN`
  * `LEFT JOIN`
  * `RIGHT JOIN`
  * `CROSS JOIN`   

### Agregate **Functions**

go with `GROUP BY` and the columns must be usually listed in the `GROUP BY` statemnet

* `DISTINCT` *use without ()*
* `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`, ... many more icl. statistical
* `FIRST`, `LAST` returns the **first** or the **last** value of a column in a group
* `CONCAT`, `GROUP_CONCAT`
* `COALESCE` takes **any number** of arguments and returns the **first not null** value.
* `IFNULL` takes **two** arguments and returns the first value that is not null.

#### snippets

##### start *i.e.* create and use databese (schema), create

```sql
CREATE DATABASE IF NOT EXISTS database;

USE databese;

CREATE TABLE table (
  id INT UNSIGNED NOT NULL AUTO_INCREMENT,
  PRIMARY KEY(id)
);
```
```sql
ALTER TABLE albums ADD (release_year INT);
```
```sql
DROP TABLE table;

DROP SCHEMA IF EXISTS database;
```
```sql
ALTER TABLE table
  RENAME TO new_table;
```
```sql
INSERT INTO bands (name)
  VALUES ('Deuce'), ('Avenged Sevenfold'), ('Ankor'); --note the parentheses!;
```
```sql
INSERT INTO albums (name, release_year, band_id) --more robust way (in case of future column modification)
  VALUES (...);
```
```sql
UPDATE albums
SET release year = 1982
  WHERE id = 1;
```
```sql
DELETE FROM mytable
  WHERE condition;
```
```sql
COALESCE(party,'None')
```

