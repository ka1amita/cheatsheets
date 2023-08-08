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

* `CREATE ` vs `DROP`
  * `TABLE <table>`
  * `DATABASE <databese>` == `SCHEMA <schema>`
* `ALTER TABLE <table>`
  * `ADD <column>`
  * `RENAME TO <new_table>`;
* `UPDATE <table>`
  * `SET <column> = <value>`
    * `WHERE <consition>`
  * `RENAME TO <new_table>`
* `DELETE FROM <table>`

> If you decide to **leave out** the `WHERE` constraint, then **all** rows are removed, which is a quick and easy way to clear out a table completely (if intentional).

### Operators

* `=`
* `!=` or `<>`
* `LIKE '<pattern>'` accepts `%` wildcart
* `OR`, `AND`

### Joins

* `FROM <table1>,<table2> WHERE <condition>` corresponds to **cross product**; deprecated in favour of `JOIN`s
* `JOIN`s
  * `LEFT JOIN`
  * `RIGHT JOIN`
  * `INNER JOIN`
  * `CROSS JOIN`   


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

