* `@Service`
* `@Autowire`
* `@Qualifier`
* `@Beans` [stackoverflow](https://stackoverflow.com/questions/17193365/what-in-the-world-are-spring-beans)
  
* `CommandLineRunner` interface
  * to run some code before the application starts, e.g. for initialising a databese
 
* FileOutputStream

#### Serialization
##### saving to file
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
