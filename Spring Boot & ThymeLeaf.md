# Spring Boot

* `return: "redirect:"` e.g. after posting (w/ POST method) redirect to home page

# Thymeleaf

[thymeleaf.org](https://www.thymeleaf.org/doc/articles/standarddialect5minutes.html)

## Frontend - Backend

### expression syntax

* `"argument"` use `"` around arguments
+ `" 'text' "` use `'` around the text within

* `${...}` : **Variable**
* `*{...}` : **Selection**
* `#{...}` : **Message** (i18n)
* `@{...}` : **Link** (URL)
* `~{...}` : **Fragment**

### attributes

* `th:text`
* `th:utext` **doesn't escape** user input (not secure)
* `th:each`
  * `iter`
    * `iter.count`
* `th:with` allows to define a variable(s)
* `th:object` allows to ommit the object name in the later statements
* `th:if` and `th:unless`

### snippets

#### ternary statements

```html
th:text = "${(car.isDriveable ()) ? 'Driveable' : 'Not Driveable'}"
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

* `th:fragment`

```html
<div xmlns:th="http://www.thymeleaf.org/" th:fragment="navigationFragment">
  <a th:href="@{/]">Home</a>
  <a th:href="@{/add-car}">Add new car</a>
</div>
```

```html
<header th:replace="~{fragments/navigation :: navigationFragment}"></header>
```


