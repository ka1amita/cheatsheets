# MySQL <a name="top"></a>

## General notes

* instead od deleting a `deleted` row, column deleted with value `false`; it should be indexed for faster performence

## Table Relationship

* 1..1
  * by using `FOREIGN KEY` together with `UNIQUE INDEX`

* \*..1
  * by using `FOREIGN KEY` so the DB is checking for *Constraint* and it **doesn't** allow removing the entry **until** the *relatonship* is removed!

* \*..\*
  * by using **junction (join) table** with *foreighn key*

## Syntax

* **statemnents**
  * **queries** are statements with `SELECT` 

* <i>**not** case sensitive</i>
* `;` and of statement
* `-- comment ` line comment
* `/* comment */` multiline comment

* `AS <alias>` can be imitted
* `WITH <alias>`

## Datatypes

* [mysql docs](https://dev.mysql.com/doc/refman/8.0/en/data-types.html)
* [3schools](https://www.w3schools.com/sql/sql_datatypes.asp)

* `INT`
  * `UNSIGNED`
* `DOUBLE`
* `CHAR(size)`, `VARCHAR(max_size)` e.g. `VARCHAR(255)`
* `TEXT`
* `BOOLEAN`
* `FLOAT`
* `DATE` `DATETIME`
* `BLOB` Binary Large Objects
* ... many more

## Functions

* [3schools](https://www.w3schools.com/sql/sql_ref_mysql.asp)

### Column Modifiers
  
* `NOT NULL`
* `AUTO_INCREMENT`
* `DEFAULT` \<value>
* `PRIMARY KEY`(\<id>) goes at the end of declaration
* `FOREIGN KEY` (\<foreign_id>) `REFERENCES` \<table>(\<id>) note above; the DB is checking for Constraint and it doesn't allow removing the entry before removing the relatonship!
* `UNIQUE INDEX` (\<foreign_id>)

### chatching errors

* `IF EXISTS`
* `IF NOT EXISTS`

### *CRUD* Keywords

* `CREATE` vs `DROP`
  * `TABLE` \<table>
  * `DATABASE` \<databese> == `SCHEMA` \<schema>
* `ALTER TABLE` \<table>
  * `ADD <column>`
  * `RENAME TO` \<new_table>
* `UPDATE` \<table>
  * `SET` \<column> = \<value>
    * `WHERE` \<consition>
  * `RENAME TO` \<new_table>
* `DELETE FROM` \<table>

> If you decide to **leave out** the `WHERE` constraint, then **all** rows are removed, which is a quick and easy way to clear out a table completely (if intentional).

### Other Clasuse Keywords

* `ORDER BY` \<column>
  * `ASC`, `DESC`
* `LIMIT` \<number>

### Operators

* `=`
* `!=` or `<>`
* `LIKE` \<pattern> accepts `%` wildcart
* `OR`, `AND`
* `CASE` \<value> `WHEN` <compare_value> `THEN` \<result> `ELSE` \<result> `END` don't forget the `END`! there is alse `CASE` 
* `CASE` `WHEN` \<condition> `THEN` \<result> `ELSE` \<result> `END` there is alse `CASE` *Statement*
* ... many more

### Joins

* `FROM` \<table1>,\<table2> `WHERE` \<condition> corresponds to **cross product**; deprecated in favour of `JOIN`s
* `JOIN`s
  * `INNER JOIN`  == `JOIN`
  * `LEFT JOIN`
  * `RIGHT JOIN`
  * `CROSS JOIN`
  * **OUTER JOIN** does't exist, use `UNION` with two `JOIN`s [stack overflow](https://stackoverflow.com/questions/4796872/how-can-i-do-a-full-outer-join-in-mysql)

### Cross Product

> CROSS JOINs are used to combine **each row** of one table with **each row** of another table, and return the *Cartesian product*

* `CROSS JOIN`

### Set Operations

> set operations **combine results** of multiple query blocks into a **single result**

* `UNION`, `UNION ALL`
  > `UNION` returns **all distinct** rows (omitting any duplicates), in contrast, `UNION ALL` returns **all** rows, including **duplicates** rows
* `INTERSECT`
  > returns rown **in common**, omitting any duplicates.
* `EXCEPT`
  > returns rows **from A** which are **not in B**, omitting any duplicates.


### Group By and gregate Functions

* `GROUP BY` \<column>

go with `GROUP BY` and the columns **must be** for most functions listed in the `GROUP BY` statemnet

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
```sql
(SELECT stud_name, subject, marks FROM students)  
  UNION  
(SELECT stud_name, subject, marks FROM student2)  
  sORDER BY marks;
```
##### **FULL OUTER JOIN**
```sql
SELECT *
  FROM t1 LEFT JOIN t2
    ON t1.id = t2.id
UNION
SELECT *
  FROM t1 RIGHT JOIN t2
    ON t1.id = t2.id
```

