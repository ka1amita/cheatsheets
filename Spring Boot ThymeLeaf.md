# Spring Boot

## ???



# Thymeleaf

## HTML

### expression syntax

* `${...}` : **Variable**
* `*{...}` : **Selection**
* `#{...}` : **Message** (i18n)
* `@{...}` : **Link** (URL)
* `~{...}` : **Fragment**

### xxx

* `th:text`
* `th:utext` **doesn't escape** user input (not secure)
* `th:each`
  * `iter` ????
* `th:with`
    * `th:with="id='#'+${iter.count}"`
* `th:object` allows to ommit the object name

### snippets

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
