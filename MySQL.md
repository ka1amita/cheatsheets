# MySQL <a name="top"></a>

##Datatypes

* `INT`
  * `UNSIGNED`
* `DOUBLE`
* `CHAR(size)`, `VARCHAR(max_size)` e.g. `VARCHAR(255)`
* `TEXT`
* ... many more

## Syntax

* `;` and of statement
* `-- comment ` line comment
* `/* comment */` multiline comment
* 


### Keywords

* `CREATE ` vs `DROP` works with both `DATABASE` and `TABLE`
  * `DATABASE` == `SCHEMA`
* `ALTER TABLE <table>`
  * `ADD <column>`
  * `RENAME TO <new_table>`;
* 

### 

* IF EXISTS
* IF **NOT** EXISTS

### column modifiers
  
* `NOT NULL`
* `AUTO_INCREMENT`
* `PRIMARY KEY(id)`

#### snippets

##### start *i.e.* create and use databese (schema), create

```sql
CREATE DATABASE IF NOT EXISTS database;
USE databese;
CREATE TABLE table (
	id INT UNSIGNED NOT NULL AUTO_INCREMENT,
  PRIMARY KEY(id)
);
DROP TABLE table;
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
