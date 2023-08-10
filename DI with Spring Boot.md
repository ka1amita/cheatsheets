## IoC[^1], DI[^2]

Dependency Inversion Principle by Robert C. Martin aka *Uncle Bob*
• **High-Level** modules should **not depend** on **low-level** modules. Both should depend on **abstractions**.
• Abstractions should not depend on details. **Details** should **depend** upon **abstractions**.

Hollywood principle
> Don't call us, we'll vallo you.

### Anotations
* `@Service` creates a **service** that can be recognised by Spring for **injection**
* `@Autowire` marks **usage** of the **dependency** inside of the application 
* `@Qualifier`
* `@Configuration`
* `@ComponentScan`
* `@Component`
* `@Beans` [stackoverflow](https://stackoverflow.com/questions/17193365/what-in-the-world-are-spring-beans)

## CommandLineRunner
  
* `CommandLineRunner` interface
  * to run some code before the application starts, e.g. for initialising a databese
 
### Serialization

> class `implements` *Serializable* interface

> "new" java.NIO library reads the whole file at once hwereas old "old" java.IO library is able to read line by line by [BufferedReader()](https://docs.oracle.com/javase%2F7%2Fdocs%2Fapi%2F%2F/java/io/BufferedWriter.html); usefull for big files 

* `FileOutputStream` (filename)
* `ObjectOutputStream` (fileOutputStream)
* `FileInputStream` (filename)
* `ObjectInputStream` (fileInputStream)

#### Snippets

##### @Service
```java
@Service
public class SpellChecker {
    public SpellChecker() {
        System.out.println("Inside SpellChecker constructor." );
    }

    public void checkSpelling() {
        System.out.println("Inside checkSpelling." );
    }
}
```
```java
@SpringBootApplication
public class SpellcheckerApplication implements CommandLineRunner {

    private SpellChecker checker;

    @Autowired
    SpellcheckerApplication(SpellChecker checker) {
        this.checker = checker;
    }

    public static void main(String[] args) {
        SpringApplication.run(SpellcheckerApplication.class, args);
    }

    @Override
    public void run(String... args) throws Exception {
        checker.checkSpelling();
    }
}
```


##### saving object to a file
```java
public void saveTaskToFile (TodoTask todoTask) {
  try {
    FileOutputStream fileOutputStream = new FileOutputStream (filename = "todoTask.txt");
    ObjectOutputStream objectOutputStream = new ObjectOutputStream (fileOutputStream);
    objectOutputStream.writeObject(todoTask);
    objectoutputStream.flush();
    objectoutputStream.close();
  } catch (Exception e) {...}
```
##### reading object from a file
```java
public TodoTask readTaskFromFile {
  try {
    FileInputStream fileInputStream = new FileInputStream (filename = "todoTask.txt") ;
    objectInputStream objectInputStream = new ObjectInputStream (fileInputStream);
    TodoTask todoTask = (TodoTask) objectInputStream.readObject();
    objectInputStream.close();
    return todoTask;
  } catch (Exception e) {...}
```
[^1]: Inversion of Control
[^2]: Dependency Injection
