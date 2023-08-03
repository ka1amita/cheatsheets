# Spring Boot

* `return: "redirect:"` e.g. after posting (w/ POST method) redirect to home page

# Thymeleaf

[thymeleaf.org](https://www.thymeleaf.org/doc/articles/standarddialect5minutes.html)

## Expression Syntax

* `"argument"` use `"` around arguments
+ `" 'text' "` use `'` around the text within

* `${...}` : **Variable**
* `*{...}` : **Selection**
* `#{...}` : **Message** (i18n)
* `@{...}` : **Link** (URL)
* `~{...}` : **Fragment**

## Operators

* `(value) ?: (defaultvalue)`
* `(if) ? (then)`
* `(if) ? (then) : (else)`

## Frontend - Backend

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
* `th:attr` ???
* `th:insert` ???
* `th:style=`
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


