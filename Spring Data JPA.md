Spring Data JPA (Jakarta Persistance API)
===============

## Introduction

### Definitions

* **ORM** == **Object Relational Mapping**
  > **framework** that anables to **map** **objects** to **rows** in relational **DSs**

### Material Review

- What is a **Foreign Key**?
  > A column or set of columns that **refer** to a **primary key** in another table
  > FC constraints: ensures data **integrity**, prevent **orphan records**
- Relationships between tables
  - When should we use one-to-one relationship?
    > 
  - What database elements do we need to create a one-to-one relationship?
    > 
  - When should we use one-to-many relationship?
    > 
   - What database elements do we need to create a one-to-many relationship?
  - When should we use many-to-many relationship?
  - What database elements do we need to create a many-to-many relationship?
- JPA
  - `@OneToOne, @OneToMany, @ManyToOne, @ManyToMany`
  - CascadeType
  - FetchType
  - mappedBy attribute
  - `@JoinColumn`
  - `@Temporal`
- Hibernate
- Web Layer, Service Layer

## Hibernate ORM

### Set-up

#### DB Credentials

##### set *Environment Variables* in IntelliJ Spring Boot project

> unbder `Run / Debug Configurations... > Modify options / Enviromental Variables` [stackoverflow](https://stackoverflow.com/questions/71450194/how-do-i-add-environment-variables-in-intellij-spring-boot-project)

##### set *application.properties* file

> in `/recources/application.properties` type `DB_username=root;DB_password=password`

```java
spring.datasource.url=jdbc:mysql://localhost:3307/foxfarm?serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=password
spring.datasource.username=${DB_USERNAME} // set by Environment Varibales
spring.datasource.password=${DB_PASSWORD} // set by Environment Varibales
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
spring.jpa.show-sql=true // sets queries to be visible in command line while running the program
spring.jpa.properties.hibernate.format_sql=true
```
##### H2
navigate to `localhost:8080/h2-console/login.do`

```java
spring.datasource.url=jdbc:h2:mem:foxfarm
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console
```
### Dependencies

* Spring Data JPA
* MySQL Driver

### Annotations
* `@Entity`
* `@Table`
  * `name`
* `@Id`
  * `GeneratedValue`()
    * `strategy` =
      * `GenerationType`.
        * `AUTO` default (based on DB)
        * `IDENTITY`
        * `TABLE`
        * `SEQUENCE`
* `@Column` specifies column metadata
  * `name` = \<name>
  * `nullable`
* `Transient` DB ignores column
* <column_name>`Date` **suffix** to the field name sets filetype to `DATETIME`
* `@Temporal`
  * (temporalType.DATE) sets datatype to `DATE`
* `@Enumerated`()
  * `EnumType`.`STRING` 


#### Table Relationship Annotations

##### setting ON DELETE or ON UPDATE bahaviour

* [mysql:FOREIGN KEY Constraints:Referential Actions](https://dev.mysql.com/doc/refman/8.0/en/create-table-foreign-keys.html#foreign-key-referential-actions)(
* [stackoverflow](https://stackoverflow.com/questions/9944137/have-jpa-hibernate-to-replicate-the-on-delete-set-null-functionality)
* [stackoverflow](https://stackoverflow.com/questions/8243400/on-delete-set-null-in-hibernate-in-onetomany))

##### 1..1 `+--+`

* `@OneTone`
  * `fetch`
    * `FetchType`.`EAGER` the field **fetched** (loaded) **together** with the rest of the fields; **default**
    * `FetchType`.`LAZY` load fetched on-demand (when you call the university's `getStudents()`)
    * [stackoverflow](https://stackoverflow.com/questions/2990799/difference-between-fetchtype-lazy-and-eager-in-java-persistence-api)
  * `cascade` = `CascadeType`.`ALL`
  * `mappedBy` together with `@JoinColumn` below ?
* `@JoinColumn`
  * `name` together with `@OneTone`(`mappedBy`) above ?
 
##### \*..1 `>--+`
* **Unidirectional**
  > `@OneToMany` together with `@JoinColumn` annotation
* **Bidirectional**
  > `@OneToMany` together with `@ManyToOne` annotation
  >
`cascade`

##### \*..\* `>--<`

* `@ManyToMany`
* `@JoinTable`(`name`, `joinColumn`, `inverseJoinColumns`)
* `@ManyToMany`(`mappedBy`)

### Assigning Primary Key values

* provide the value of **all fields** yourself in the main program
  > In cases where the id carries some **business information**,e.g. a phone number
* provide only the non-primarykey values and *Hibernate* provides the value of primary key column **automatically**

### Queries

#### Default methods (queries)

> create new interface and extend `CrudRepository`\<ModelClass, IdDatatype> (optionally include `@Repository`)

> use `@Override` annotation

* .`createFox`
* .`save` works bot for UPDATE and INSERT besed on whether the entry exists or not
* .`findAll` combine with .`forEach`(someList::`add`)
* .`findOne`
* .`findById`
* .`count`
* .`delete` 
* .`updateFoxById`

#### Custom methods (queries)

extracts the **query** from the **method's name**
[spring docs](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.query-methods.query-creation)

> `extends JpaRepository`\<ModelClass, IdDatatype>

> `findFoxByNameContainsIgnoreCase`

##### JPQL vs Native queries

> JPQL queri works across databeses, native is specific


#### Snippets
##### name DB table
```java
@Entity(name="EMPLOYEE")
@Table(name="EMPLOYEE")
```
##### override column metadata
```java
@Column(name = "FULL_NAME", nullable = false)
@Column(table="EMP_DATA")
```
##### set composite Primary key
```java
@Entity
@IdClass(CompositeKey.class)
public class EntityExample {
    @Id
    private int column1;
    @Id
    private int column2;
    @Id
    private int column3;
}
```
##### extend CrudRepository
```java
public interface DepartmentRepository extends CrudRepository<Department, Long> {...}
```
##### JPA queries
```java
@Query("SELECT f FROM Fox f WHERE f.color = 'Red'")
List<Fox> findAllRedFoxes ();
@Query(value = "SELECT * FROM foxes WHERE color = 'Red', nativeQuery = true)
List<Fox> findAllRedFoxesNative();
```
##### custom queries
```java
List<Post> findAllByAuthorFirstName(String first);
List<Post> findAllByAuthorFirstNameIgnoreCase(String first);
List<Post> findAllByAuthorFirstNameIgnoreCaseOrderByPostedOnDesc(String first);
List<Post> findAllByAuthorFirstNameAndAuthorLastName(String first, String last);|
```
#### Ralationships

##### 1..1
```sql
@Entity
public class User {
  @Id
  @Generted Value(strategy = GeneratedValue.IDENTITY)
  private long id;
  ...
  @OneTone(fetch = FetchType.LAZY, cascade = CascadeType.ALL)
  @JoinColumn(name ="userprofile_id")
  private UserProfile userProfile;
}

@Entity
public class UserProfile {
  @Id
  @Generted Value(strategy = GeneratedValue.IDENTITY)
  private long id;
  ...
  @OneToOne(fetch = FetchType.LAZY, cascade = CascadeType.ALL, mappedBy = "userProfile")
  private User user;
}
```
##### 1..*
```sql
@Entity
public class Course {
  @Id
  @Generted Value(strategy = GeneratedValue.IDENTITY)
  private long id;
  ...
  @ManyToOne (mappedBy = "")
  private List<Device> devices; // List of !
}

@Entity
public class Device {
  @Id
  @Generted Value(strategy = GeneratedValue.IDENTITY)
  private long id;
  ...
  @
  private 

}
```

##### *..*
```sql

``` 
