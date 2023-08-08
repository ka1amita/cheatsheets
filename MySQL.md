# MySQL <a name="top"></a>

## Syntax

* <i>**not** case sensitive</i>
* `;` and of statement
* `-- comment ` line comment
* `/* comment */` multiline comment

* `AS <alias>` can be imitted
* `WITH <alias>`

##Datatypes

* `INT`
  * `UNSIGNED`
* `DOUBLE`
* `CHAR(size)`, `VARCHAR(max_size)` e.g. `VARCHAR(255)`
* `TEXT`
* ... many more

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

### operators
* `LIKE '<pattern>'` accepts `%` wildcart

### chatching errors

* `IF EXISTS`
* `IF NOT EXISTS`

### column modifiers
  
* `NOT NULL`
* `AUTO_INCREMENT`
* `DEFAULT <value>`
* `PRIMARY KEY(<id>)` goes at the end of declaration
* `FOREIGN KEY (band_id) REFERENCES <TABLE>(<id>)` as above

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
