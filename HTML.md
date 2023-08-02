# HTML <a name="top"></a>

* [tables](#tables)
* [forms](#forms)

## tables <a name="tables"></a>

* [mozilla](https://developer.mozilla.org/en-US/docs/Learn/HTML/Tables/Basics)
* [css-trics](https://css-tricks.com/complete-guide-table-element/)

### elements

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

### attributes

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

### css

* `width`
* `text-align`
* `border`

    > by default, all table cells are spacing out from one another by 2px

* `border-spacing`
* `border-collpase`
  * `collapse`
* `padding`
* `tr:nth*child(even)` or `(odd)` styles only overy evn or odd row

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

[top](#top)
