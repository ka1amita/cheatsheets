# MySQL <a name="top"></a>

## General notes

* instead od deleting a `deleted` row, column deleted with value `false`; it should be indexed for faster performence

* **primary key** can be **combination of more columns** that is **unique**! e.g. palyer_inventory table with player-item pairs

## Table Relationship

* 1..1
  * by using `FOREIGN KEY` together with `UNIQUE INDEX`

* \*..1
  * by using `FOREIGN KEY` so the DB is checking for *Constraint* and it **doesn't** allow removing the entry **until** the *relatonship* is removed!

* \*..\*
  * by using **junction (join) table** with *foreighn key*

## Style Guide

[sqlstyle](https://www.sqlstyle.guide/)

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

## KEYWORDS

### Column Modifiers
  
* `NOT NULL`
* `AUTO_INCREMENT`
* `DEFAULT` *\<value>*
* `PRIMARY KEY`*(\<id>)* goes at the end of declaration
* `FOREIGN KEY` *(\<foreign_id>)* `REFERENCES` *\<table>*(*\<id>*) note above; the DB is checking for Constraint and it doesn't allow removing the entry before removing the relatonship!
* `UNIQUE INDEX` *(\<foreign_id>)*

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

>  Returns all records from the right table, and the matching records from the left table

> It does'm mean tha there is (exactly) only one row from the right table. E.g. the rows from right table are "copied" as long as there are unique rows in left table with corresponding match.

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

> set operations **combine results** of multiple query blocks into a **single result** assuming that they have the same column **count**, **order** and **data type**

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

## Order of Execution

[sqlbolt](https://sqlbolt.com/lesson/select_queries_order_of_execution)

1. FROM and JOINs
2. WHERE
3. GROUP BY
4. HAVING
5. SELECT
6. DISTINCT
7. UNION
8. ORDER BY
9. LIMIT / OFFSET

## Subqueries

> A *subquery* can be referenced anywhere a normal table can be referenced.
> Inside a `FROM` clause, you can `JOIN` subqueries with other tables, inside a `WHERE` or `HAVING` constraint, you can test expressions against the results of the subquery, and even in expressions in the SELECT clause.

### Correlated Subqueries

> In *correlated Subqueries* the inner query **references**, and is dependent on, a **column** or **alias** from the **outer query**.
> Unlike the subqueries above, **each** of these **inner querie**s need to be **run** for **each** of the of the **outer query row**s
 
> Five **meaningful aliases** to the temporary values and tables.

> In addition, correlated subqueries can be difficult to optimize, so performance characteristics may vary across different databases.


### Snippets

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
  ORDER BY marks;
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

##### Correlated Subquery
```sql
SELECT *
  FROM employees
  WHERE salary > (SELECT AVG(revenue_generated)
                    FROM employees AS dept_employees
                    WHERE dept_employees.department = employees.department);
```
```sql
SELECT *,
FROM mytable
WHERE column
  NOT IN (SELECT another_column
            FROM another_table);
```
## Normalization

### Dependencies

* dependency --> e.g. man --> age
* multi-value -->> e.g. man -->> nationality 

## Normalization Levels

* 1.NF
  * > row order must **not** convey **information**
  * > **mixing** **data types** is not allowed
  * > table must have a **primary keys** (can be conbination of more columns)
  * > table must not have **repeating group** on a **single row**
* 2.NF
  > each **non-key attribute**[^1] must **depend** on the **entire** primary key
  * deletion anomaly
  * update anomaly
  * insertion anomaly
* 3.NF
  > each (**non-key**) **attribute** must **depend** on the the primary key, the whole primary key, and **nothing but** the **primary key**
  > e.g. no *transiont dependencies*
* 4.NF
  > only allowed **multivalued dependencies** in a table must be multivalued dependencies **on the key**
* 5.NF
  > the table (in 4.NF) musn't be **describable** as the logical result of **joining** some **other tables** together

[^1]: non-key attributes == non-key columns
