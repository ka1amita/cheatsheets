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
      * Identify
      * 
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

### Queries

#### Default methods (queries)

> use @Override annotation

> `extends CrudRepository`\<ModelClass, IdDatatype>

  > `@Repository` optional

* `createFox`
* `save`
* `findAll`
* `findById`
* `count()`
* `deleteById` 
* `updateFoxById`

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



