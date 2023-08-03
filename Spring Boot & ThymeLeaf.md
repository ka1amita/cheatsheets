# Spring Boot

* `return: "redirect:"`

# Thymeleaf

[thymeleaf.org](https://www.thymeleaf.org/doc/articles/standarddialect5minutes.html)

## HTML

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

#### condition

```html
<p th:if="$(cars.isEmpty]">There are no cars to show</p>
<p th:unless="$(cars.isEmpty]">There are no cars to show</p>
```

#### example

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
