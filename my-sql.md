MySQL <a name="top"></a>
=====

[](#General-notes)
[](#normalization-forms)

## Introduction

### Instalation

##### MySQL locations[^3]

`/usr/local/mysql/bin`

##### add mysql to your PATH[^3]

`echo 'export PATH="/usr/local/mysql/bin:$PATH"' >> ~/.bash_profile`

##### login

`mysql -u root -p`

### Definitionas and General Notes

* **instead** od **deleting** a row, column *deleted* is set to **false**; it should be **indexed** for faster performence

* **primary key** can be **combination of more columns** that is **unique**! e.g. palyer_inventory table with player-item pairs

### Material Review

* Questions
  > Answers 

## Table Relationship

* 1..1 `+--+`
  * by using `FOREIGN KEY` together with `UNIQUE INDEX`

* \*..1 `>--+`
  * by using `FOREIGN KEY` so the DB is checking for *constraint* and it **doesn't allow removing** the entry **until** the *relatonship* is **removed**

* \*..\* `>--<`
  * by using **junction (join) table** with `FOREIGN KEY`s

## Normalization

### Reasons

* minimaze **duplicate data**
  * **increases storage** and **decreases performance**
  * difficult to **maintain**
* avoid data **modification issues**
* **simplyfy queries**

### But!
> There are times when you’ll intentionally **denormalize data**.
> If you need to **present** **summarized data** to a user, and that data is very time **consuming** or **resource intensive** to **create**, it may make sense to **maintain** this data **separately**.

### Anomalies

  * **insertion** anomaly
    > there are facts we cannot record until we know information for the entire row.
  * **update** anomaly
    > the same information is recorded in multiple rows
  * **deletion** anomaly
    > removal of row results in removal of more than one set of facts

### Dependencies

* dependency --> e.g. man --> age
* multi-value -->> e.g. man -->> nationality 

## Normalization Forms
> The forms are **progressive**, meaning that to **qualify** for **2nd** normal form a table must first satisfy the rules for **1st** normal form etc.

* 1NF
  > row **order** must **not** convey **information**
  > **mixing** **data types** is not allowed, i.e. they are *atomic values*[^2]
  > table must have a **primary keys** (can be conbination of more columns)
  > table must not have **repeating group** on a **single row**
* 2NF
  > each **non-key attribute**[^1] must **depend** on the **entire** **primary key**
* 3NF
  > each (**non-key**) **attribute** must **depend** on the the primary key, the entire primary key, and **nothing but** the **primary key**
  > *transiont dependencies* are **not allowed**
* 4NF
  > only allowed **multivalued dependencies** in a table must be multivalued dependencies **on the key**
  > e.g. student id student's classes and student's hobbies
* 5NF
  > the table (in 4NF) musn't be **describable** as the logical result of **joining** some **other tables** together (contain **independen't** attributers)
  > e.g person, favourite brands and favourite flavours can't be together in single table in **not 5NF**
  > but e.g. particular supplier suppling a particular part to a particular customer (signel order) **is** in **5NF**

## Style Guide

[sqlstyle](https://www.sqlstyle.guide/)

## Syntax

* **queries** are _statements_ with `SELECT`

* ``` `` ``` aka backticks (can be) are used to **quote** table and column _names_
  > that becomes necessary when useing **reserved keywords** as names, e.g. table
* `''` aka single quotes are used for **string input**
  > **not reccomanded** for number type as it has to be **type converted** and can result in some inconsistencies
  
  
* <i>**not** case sensitive</i>
* `;` and of statement
* `-- comment ` line comment
* `/* comment */` multiline comment

* `AS <alias>` can be imitted
* `WITH <alias>`

## Datatype

* [mysql docs](https://dev.mysql.com/doc/refman/8.0/en/data-types.html)
* [3schools](https://www.w3schools.com/sql/sql_datatypes.asp)

* `INT`
  * `UNSIGNED`
* `DOUBLE`
* `CHAR(size)`, `VARCHAR(max_size)` e.g. `VARCHAR(255)`
* `TEXT`
* `BOOLEAN`
* `FLOAT`
* `DATE` supported range is '1000-01-01' to '9999-12-31'
* `DATETIME`
* `BLOB` Binary Large Objects
* ... many more

## Functions

* [3schools](https://www.w3schools.com/sql/sql_ref_mysql.asp)

## KEYWORDS

### Column Constraints (Modifiers)
  
* `NOT NULL`
* `AUTO_INCREMENT` \[`=` \<step>\]
* `DEFAULT` \<value>
* `PRIMARY KEY`(\<id>) goes at the end of declaration
* `FOREIGN KEY` (\<foreign_id>) `REFERENCES` \<table>(\<id>) note above; the DB is checking for
  Constraint and it doesn't allow removing the entry before removing the relatonship!
  * \[`CONSTRAINT` \<name> ...\]
* `UNIQUE INDEX` (\<foreign_id>)

### Catching Errors

* `IF EXISTS`
* `IF NOT EXISTS`

### _DDL_ Keywords

* `CREATE` vs `DROP`
  * `TABLE` \<table> (\<column>
    \<datatype> \[`AUTO_INCREMENT`, `UNIQUE`, `NOT NULL`, `PRIMARY KEY`, ...\], \[`CONSTRAINT`
    \<constraint_name> `PRIMARY KEY`(\<column>)\])
  * `DATABASE` \<database>
  * `SCHEMA` \<schema>
* `RENAME TABLE` \<table> `TO` \<new_name>
* `ALTER TABLE` \<table>
  * `RENAME TO` \<new_table>
  * `ADD`
    * \<column> \<datatype> \[<ddl,...>]
    * `CONSTRAINT` \<constraint_name> `REFERENCES` \<table> (id) \[`ON` `DELETE`
      \<option>] \[`ON` `UPDATE`\<option>]
  * `DROP` `CONSTRAINT` \<constraint_name>
  * `CHANGE COLUMN` \<column> \<new_name> `DATATYPE` `MODIFIERS` necessary!, it rewrites the whole
    definition
* `DROP`
  * `TABLE` \[`IF EXISTS`] \<table>
  * `SCHEMA` \<schema>
* `ALTER` `TABLE` \<table>
  * `DROP` \<column>;
  * `MODIFY` \<column> {`NOT NULL` |...}

### *CRUD* Keywords

* `INSERT INTO` \<table> \[(\<column,...>)[^columns]\] `VALUES` (\<value,...>)
* `UPDATE` \<table>
  * `SET` \<column> `=` \<value> `WHERE` \<condition>
* `DELETE FROM` \<table> `WHERE` \<condition>
> If you decide to **leave out** the `WHERE` constraint, then **all** rows are removed, which is a quick and easy way to clear out a table completely (if intentional).

* `SELECT` <column,... or `*`> `FROM` \[`WHERE`, ...] \[`JOIN`...]

[^columns]: more robust way (in case of future column modification)

### Other Clauses Keywords

* `ORDER BY` \<column>
  * `ASC`, `DESC`
* `LIMIT` \<number>

### Operators

* `=`
* `!=` or `<>`
* `LIKE` \<pattern> accepts `%` wildcard
* `OR`, `AND`
* `CASE` \<value> `WHEN` <condition> `THEN` \<result> `ELSE` \<result> `END` - don't forget the `END`! there is also `CASE` *Statement*
* ... many more

### Joins

>  Returns all records from the right table, and the matching records from the left table

> It doesn't mean tha there is (exactly) only one row from the right table. E.g. the rows from right table are "copied" as long as there are unique rows in left table with corresponding match.

* `FROM` \<table1>,\<table2> `WHERE` \<condition> corresponds to **cross product**; deprecated in favour of `JOIN`s
* `JOIN`s
  * `INNER JOIN` == `JOIN`
  * `{ LEFT | RIGHT } [ OUTER ] JOIN`
  * `CROSS JOIN`
  * ~~`OUTER JOIN`~~ **doesn't exist**, use `UNION` with two `JOIN`s [stack overflow](https://stackoverflow.com/questions/4796872/how-can-i-do-a-full-outer-join-in-mysql)

```sql
SELECT `l`.`column`, `r`.`column`
    FROM `left` AS `l`
    LEFT JOIN `right` AS `r` ON `l`.`id` = `r`.`left_id`
    GROUP BY `id`;
```

### Cross Product

> CROSS JOINs are used to combine **each row** of one table with **each row** of another table, and return the *Cartesian product*

* `CROSS JOIN`

### Set Operations

> set operations **combine results** of multiple query blocks into a **single result** assuming that they have the same column **count**, **order** and **data type**

* `UNION`, `UNION ALL` returns **all distinct** rows (omitting any duplicates), in contrast, `UNION ALL` returns **all** rows, including **duplicates** rows
* `INTERSECT` returns rows **in common**, **omitting** _any duplicates_.
* `EXCEPT` returns rows **from A** which are **not in B**, **omitting** any _duplicates_.

### Functions

* `COALESCE` takes **any number** of arguments and returns the **first not null** value.
* `IFNULL` takes **two** arguments and returns the first value that is not null.

 
### Aggregate Functions

[MySQL docs: Aggregate Function Descriptions](https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions-and-modifiers.html)

**_Aggregate functions_** **operate on sets** of _values_. 
They are often **used** **with** a `GROUP BY` clause to **group** _values_ into **subsets**.

* `DISTINCT` *use without ()*
* `COUNT(*)`, `COUNT(DISTINCT)`, `SUM()`, `AVG()`, `MIN()`, `MAX()`, `STDDEV()`, ... many more icl.
  statistical
* `CONCAT()`, `GROUP_CONCAT()`

[//]: # (* ~~`FIRST`~~, ~~`LAST`~~ returns the **first** or the **last** value of a column in a group)

### `GROUP BY`

* `GROUP BY` \<column>

##### `GROUP BY` and functional dependence

`SELECT name, address, MAX(age) FROM t GROUP BY name;`

> The query is invalid if `name` is not a _primary key_ of `t` or a _unique_ `NOT NULL` column. 
> In this case, no functional dependency can be inferred and an error occurs:

To tell _MySQL_ to accept the query, you can **use** the `ANY_VALUE()` function or **disable** `ONLY_FULL_GROUP_BY`[^functional-dependence].

[^functional-dependence]: _MySQL_ implements **detection** of _functional dependence_.
  If the `ONLY_FULL_GROUP_BY` SQL mode is enabled (which it is by **default**),
  _MySQL_ **rejects** queries for which the select list,
  `HAVING` condition, or `ORDER BY` list refer to _nonaggregated_ columns that are
  **neither named** in the `GROUP BY` clause nor are functionally **dependent** on them.
  **Disabling** `ONLY_FULL_GROUP_BY` is useful primarily when you **know** that,
  due to some property of the data,
  all values in each nonaggregated column not named in the `GROUP BY` are **the same** for each group.


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

## Sub-queries

> A *subquery* can be referenced anywhere a normal table can be referenced.
> Inside a `FROM` clause, you can `JOIN` subqueries with other tables, inside a `WHERE` or `HAVING` constraint, you can test expressions against the results of the subquery, and even in expressions in the SELECT clause.

### Correlated Sub-queries

> In *correlated Subqueries* the inner query **references**, and is dependent on, a **column** or **alias** from the **outer query**.
> Unlike the subqueries above, **each** of these **inner querie**s need to be **run** for **each** of the of the **outer query row**s
 
> Five **meaningful aliases** to the temporary values and tables.

> In addition, correlated subqueries can be difficult to optimize, so performance characteristics may vary across different databases.

## Snippets

#### CRUD

##### create and use databese (schema), create table

```mysql
CREATE DATABASE IF NOT EXISTS `database`;

USE `databese`;

CREATE TABLE `orders`
(
  `id` INT UNSIGNED NOT NULL AUTO_INCREMENT,
  PRIMARY KEY (`id`)
    CONSTRAINT `FK_person_order` FOREIGN KEY (`persons_id`) REFERENCES persons(`id`) -- optional naming of constraint
);
```
##### insert row
```sql
INSERT INTO `albums` (`name`, `release_year`, `band_id`) -- more robust way (in case of future column modification)
  VALUES (...);
```
##### insert multiple rows
```sql
INSERT INTO `bands` (`name`)
  VALUES ('Deuce'), ('Avenged Sevenfold'), ('Ankor'); --note the parentheses!;
```
##### modify values
```sql
UPDATE `albums`
SET `release_year` = 1982
WHERE `id` = 1;
```
##### delete rows
```sql
DELETE
FROM `mytable` -- always do a dry run with SELECT!
  WHERE condition;
```
##### COALESCE
```sql
COALESCE(party,'None')
```

```sql

```
##### UNION
```sql
(SELECT stud_name, subject, marks FROM students)  
  UNION  
(SELECT stud_name, subject, marks FROM student2)  
  ORDER BY marks;
```
##### FULL OUTER JOIN emulation
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
##### Subquery
```sql
SELECT *
FROM `mytable`
WHERE column
  NOT IN (SELECT another_column
            FROM another_table);
```

#### get today's date
```sql
CAST(NOW() as DATE);
```
```sql
`created` DATE NOT NULL DEFAULT (CAST(NOW() as DATE)),
```

#### Troubleshooting

##### drop schema
```sql
DROP SCHEMA IF EXISTS `database`;
```
##### drop table
```sql
DROP TABLE `table`;
```
##### add column
```sql
ALTER TABLE `albums`
  ADD (`release_year` INT);
```
##### rename table
```sql
ALTER TABLE `table`
  RENAME TO new_table;
```
##### modify column
```sql
ALTER TABLE `todos`
  CHANGE COLUMN column new_name DATATYPE MODIFIERS; -- both necessary! it rewrites whole column definition
```
##### add foreign key constraint to existing column
```sql
ALTER TABLE Orders
  ADD FOREIGN KEY (PersonID) REFERENCES Persons(PersonID);
```

[^1]: non-key attributes == non-key columns
[^2]: we can’t further subdivide the value, e.g. the value "Chicago” **is** atomic whereas “Chicago; Los Angeles; New York” **is not**.
[^3]: [bradtraversy](https://gist.github.com/bradtraversy/c831baaad44343cc945e76c2e30927b3)
