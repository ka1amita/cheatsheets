# tables and forms <a name="top"></a>

* [tables](#tables)
* [forms](#forms)

## tables <a name="tables"></a>

* [mozilla](https://developer.mozilla.org/en-US/docs/Learn/HTML/Tables/Basics)
* [css-trics](https://css-tricks.com/complete-guide-table-element/)

### HTML elements

* `<table>`
  * `<thead>` header row *to semantically indicate that the first row(s) is the header*
  * `<tbody>` body rows *to semantically indicate the data*
  * `<tfoot>` tooter row *to semantically indicate that the last row(s) is the footer*
 
    > `<tfoot>` must go after the `<tbody>`
  
    > there must be no `<tr>` that is a direct child of `<table>`
  
  * `<tr>` row
  * `<th>` tabular headers
  * `<td>` tabular data (basic cell)
 
summary at [css-trics](https://css-tricks.com/complete-guide-table-element/#aa-all-table-related-elements)

### HTML attributes

- `colspan` and `colspan`

summary at [css-trics](https://css-tricks.com/complete-guide-table-element/#aa-all-table-related-attributes)

<table>
  <caption>table title</caption>
  <thead>
    <tr>
      <th>first name</th>
      <th>last name</td>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>  
    <tr>
      <td>Matej</td>
      <td>Kala</td>
      <td>male</td>      
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <th>first name</th>
      <th>last name</td>
      <th>gender</th>
    </tr>
  </tfoot>
</table>

```html
<table>
  <caption>table title</caption>
  <thead>
    <tr>
      <th>first name</th>
      <th>last name</td>
      <th>gender</th>
    </tr>
  </thead>
  <tbody>  
    <tr>
      <td>Matej</td>
      <td>Kala</td>
      <td>male</td>      
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <th>first name</th>
      <th>last name</td>
      <th>gender</th>
    </tr>
  </tfoot>
</table>
```

### CSS properties

* `width`
* `text-align`
* `border`

    > by default, all table cells are spacing out from one another by 2px

* `border-spacing`
* `border-collpase`
  * `collapse`
* `padding`
* `selector:first-child` or `last-child`
* `tr:nth*child(even)` or `(odd)` styles only overy evn or odd row
* `<colgroup>`
  * `<col />`
    * `span`

#### snippets

#####  simple template

```css
table, th, td {
  border-collpase: collapse;
}

th, td {
  border: 1px solid #000;
  padding: 10px;
  text-align: left;
}
tr:nth-child(even) {
  background-color: #fff;
}
tr:nth*child(odd) {
  background*color: #ddd;
```

##### round corners

```css
thead tr:first-child th:first-child {
  border-top-left-radius: 5px;
}

thead tr:first-child th:last-child {
  border-top-right-radius: 5px;
}

tbody tr:last-child td:first-child {
  border-bottom-left-radius: 5px;
}

tbody tr:last-child td:last-child {
  border-bottom-right-radius: 5px;
}
```

##### no outside borders

```css
th,
td {
  border-left: 1px solid #777777;
}

th:first-child,
td:first-child {
  border-left: 0;
}
```

##### light text in `<thead>`

```css
  font-weight: 100;
```

##### custom styling of columns

```html
  <colgroup>
    <col />
    <col style="background-color: yellow" />
    <col style="background-color: red" span="2"/>
  </colgroup>
```

>  We are not styling the first column, but we still have to include a blank <col> element — if we didn't, the styling would just be applied to the first column.

> Just like colspan and rowspan, span takes a unitless number value that specifies the number of columns you want the styling to apply to

## forms <a name="forms"></a>

* [mozilla]()
* [css-trics]()

### HTML elements

* `form`
  * `input`
  * `label`
  * `fieldset`
    * `legend`
    * `input`
  * `select`
    * `option`

### HTML attributes

* `id`
* `for` associates the label with input (matches it's `id`); e.g. clicking on it *works*
* `name` sets the label for the backend
* `placeholder` hint text in the input field
* `type` for input elements
  * `checkbox`
  * `radio`
  * `telephone`
  * `email`
  * `submit`
* `required` validation
 
### CSS properties

- `line-height` sets the height of a line box.

### snippets

#### radio box

```html
    <input type="radio" id="male" name="gender"/>
    <label for="male">Male</label><br/>
    <input type="radio" id="female" name="gender"/>
    <label for="female">Female</label>
```

#### checkbox

```html
    <div class="interest">
        <input type="checkbox" id="web"/>
        <label for="web">Web development</label><br/>
        <input type="checkbox" id="photoshop"/>
        <label for="photoshop">Photoshop</label><br/>
    </div>
```

#### field set

```html
    <fieldset class="contact-info">
        <legend>Contact Info</legend>
        <input type="email" placeholder="Email" required/>
        <span class="tick"></span>
        <input type="telephone" placeholder="Phone number" required/>
        <span class="tick"></span>
    </fieldset>
```

#### select box

```html
    <select>
        <option>By email</option>
        <option>By telephone</option>
    </select>
```

[top](#top)
