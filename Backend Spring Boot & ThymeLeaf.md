# Spring Boot



* `@RequestParam`
  * `(required = flase)` good idea to combine with `Optional<T>`

## Redirections
* `return: "redirect:"` e.g. after posting (w/ POST method) redirect to home page

> If you redirect from one controller to another controller the values in model object won't be available in the other controller. So if you want to share the model object values then you have to say in first controller.

* `RedirectAttributes`
  * `addAttribute()`
    > it will pass the values as *requestparameters* instead of *model*,so when you add some using `addAttribute` you can access those values from `request.getParameter`
    > it essentially constructs request parameters out of your attributes and redirects to the desired page with the request parameters
  * `addFlashAttribute()`
    > it actually stores the attributes in a flashmap (which is internally maintained in the **users session** and **removed** once the next redirected request gets fulfilled)

  > A FlashMap provides a way for one request to store attributes intended for use in another. This is most commonly needed when redirecting from one URL to another -- e.g. the Post/Redirect/Get pattern. A FlashMap is saved before the redirect (typically in the session) and is made available after the redirect and removed immediately.

  > So the advantage of `addFlashAttribute()` will be that you can store pretty much **any object** in your flash attribute (as it is not serialized into request params at all, but maintained as an object), whereas with `addAttribute()` since the object that you add gets transformed to a normal request param, you are pretty limited to the object types like **String or primitives**.
  
### Snippets
#### redirect with model attribute
```java
if (!message.isPresent() || !favColor.isPresent()) {
  redirectAttributes.addFlashAttribute(attributeName:"errorMessage', attributeValue: "please fill in all fields!")
  return "redirect:/";
}
```

# Thymeleaf

[thymeleaf.org](https://www.thymeleaf.org/doc/articles/standarddialect5minutes.html)

## Geberal Syntax

### Expressions

* `"argument"` use `"` around arguments
+ `" 'text' "` use `'` around the text within including cases when using text as argument

* `${...}` : **Variable**
* `*{...}` : **Selection**
* `#{...}` : **Message** (i18n)
* `@{...}` : **Link** (URL)
* `~{...}` : **Fragment**

### Operators

* `(value) ?: (defaultvalue)`
* `(if) ? (then)`
* `(if) ? (then) : (else)`

#### operator snippets

```
"{getBalance()} < 0 ? 'true' : 'false'"
```
```
"{getBalance() < 0} ? 'true' : 'false'"
```
```
th:selected="${drink == currentDrink}
```

## HTML Template

### attributes

* `th:fragment`
* `th:text`
* `th:utext` **doesn't escape** user input (not secure)
* `th:each`
  * `iter`
    * `iter.count` starts from 1
    * `instanceStat.index` starts from 0
* `th:with` allows to define a variable(s)
* `th:object` allows to ommit the object name in the later statements
* `th:if` and `th:unless`
* `th:switch` together with `th:case` note that `switch` must be inside parent element relative to `case` lements
* `th:attr` ???
* `th:insert` ???
* `th:style`
* `th:styleappend`


### snippets

#### conditional styling using styleappend

```html
<td th:styleappend="${condition} ? 'color: red; font-size: 20px;' : 'color: black; font-size: 30px;'"
```

#### iteration

##### using Stat suffix

```
th:each="account : ${accounts}" th:with="id=${accountStat.count}"
```

##### using iter property

```
th:each="car, iter : ${cars}" th:with="id='#'+${iter.count}"
```

#### number format

```
${#numbers.formatDecimal(abc.value, 1, 'DEFAULT', 2, 'DEFAULT')}
```

#### operator

```
th:text = "${(car.isDriveable ()) ? 'Driveable' : 'Not Driveable'}"
```

### th:each
```html
<li th:each= "book : ${books}">
```

#### condition

```html
<p th:if="$(cars.isEmpty]">There are no cars to show</p>
<p th:unless="$(cars.isEmpty]">There are no cars to show</p>
```

#### dynamic URI
```html
<th:block th:with="id=${42774564}">
  <a th:href="@{https://stackoverflow.com/questions/{id}(id=${id})}">Stack Overflow</a>
</th:block>
```

#### table filled from DB

```html
<tabale>
  <thead>
    <tr>
      <th>ID</th>
      <th>Brand</th>
    </tr>
  </thead>
  <tbody>
    <tr th:each="car, iter : ${cars}" th:object="{car}" th:with="id='#'+${iter.count}">
      <td th:text="${id}"></td>
      <td th:text="getBeand()}"></td>
    </tr>
  </tbody>
</table>
```

#### form creating an object

form

```html
<form method="post" action="/add-car" th:object="${car}">
  <label for="brand">Brand</label>
  <input type="text" name="brand" id="car-brand" th:field="*{brand} " required>
</form>
```

default Car constructor

```java
public Car(String brand, String model, int year, boolean driveable, String image) {
  this.brand = brand;
  this.model = model;
  this.year = year;
  this.driveable = driveable;
  this.image = image;
}
```

Controller method

```java
public void addCar(Car car){
  cars.add (car);
}
```

## HTML Fragments

```html
<div xmlns:th="http://www.thymeleaf.org/" th:fragment="navigationFragment">
  <a th:href="@{/]">Home</a>
  <a th:href="@{/add-car}">Add new car</a>
</div>
```

```html
<header th:replace="~{fragments/navigation :: navigationFragment}"></header>
```


