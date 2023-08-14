```java
package io.javabrains.springbootstarter.topic;
import org.springframework.data.repository.CrudRepository;
public interface TopicRepository extends CrudRepository<Topic, String»| (
```

## Hibernate App
### Annotations
* `@Entity`
* `@Table`
* `@Id`
  * `GeneratedValue`
    * (strategy = GenerationType. )
* `@Column` specifies column metadata
  * (name = "FULL_NAME", nullable = false)
* `Transient` DB ignores column
* `...Date` suffix sets filetype to `DATETIME`
* `@Temporal` sets datatype to `DATE`

### Assigning Primary Key values

* provide the value of **all fields** yourself in the main program
  > In cases where the id carries some **business information**,e.g. a phone number
* provide only the non-primarykey values and *Hibernate* provides the value of primary key column **automatically**.
