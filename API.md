## Material Review

* What is the difference between server-side and client-side rendering?
  * frontend vs backend app
    > e.g. Thymeleaf is a backend app - the HTML page including the data is rendered in the ***backend**
    > in the other hand a mobile app in a phone is sending requests trough API requests to DB to get the data and render them in the **frontend**
  * **frontend** app
    > e.g. a mobile app is sending requests trough API requests to DB
  * **REST**
  > RESTful Controller send only data - used for **backend API**S
  > **it is stateless**

* What is API? What is a Web API?
  > 
* What is a web service?
  > 
* What is RESTful web application and what is not?
  > 
* What are the most common HTTP methods and what are they used for?
  > 
* What kind of response types can an API endpoint produce?
  > 
* What are the parts of the URL and how to use them?
  >
* What is common and what differentiates between HTTP requests and responses?
  > 
* What is Postman used for?
  > 
* What is Jackson ObjectMapper and why is it useful?
  > 
* How to create a Java class which maps to a JSON Object?
  > 
* What has a very bad naming in Spring Framework?

####

##### REST Controller
```java
@RestController
@RequestMapping("/api")
  public class MainController {
@GetMapping
public ResponseEntity<String> sayHello(){
return ResponseEntity.ok("Hello");
}
```
