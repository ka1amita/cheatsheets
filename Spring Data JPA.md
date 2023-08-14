#Java (J...) Persistance API
## Hibernate App

### Set-up
##### DB Credentials
```java
spring.datasource.url=jdbc:mysql://localhost:3307/foxfarm?serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=password
spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL8Dialect
spring.jpa.show-sql=true // sets queries to be visible in command line while running the program
spring.jpa.properties.hibernate.format_sql=true
```

### Dependencies

* **Spring Data JPA**
* **MySQL Driver**

### Annotations
* `@Entity`
* `@Table`
  * `name`
* `@Id`
  * `GeneratedValue`
    * `strategy` 
      * GenerationType
      * ...
* `@Column` specifies column metadata
  * `name`
  * `nullable`
  * ...
* `Transient` DB ignores column
* `...Date` suffix to the field name sets filetype to `DATETIME`
* `@Temporal` sets datatype to `DATE`
* `@Enumerated`

### Assigning Primary Key values

* provide the value of **all fields** yourself in the main program
  > In cases where the id carries some **business information**,e.g. a phone number
* provide only the non-primarykey values and *Hibernate* provides the value of primary key column **automatically**

### Default methods

use @Override annotation

`extends CrudRepository` \<ClassName, Id_Datatype> optionally combined with `@Repository`

* `createFox` (Fox fox)}
* `save`
* `findAll`
* `findById`
* `count()`
* `deleteById` (Long id) 
* `updateFoxById` (Long id)

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



