# HTML <a name="start"></a>

- [tables](#tables)
- [forms](#forms)

## tables <a name="tables"></a>

- [mozilla](https://developer.mozilla.org/en-US/docs/Learn/HTML/Tables/Basics)
- [css-trics](https://css-tricks.com/complete-guide-table-element/)

### tags

- `<table>`
  - `<thead>` header row
  - `<tbody>` body rows
  - `<tfoot>` tooter row
  - `<tr>` row
  - `<th>` header
  - `<td>` data (basic cell)

<table>
  <caption>table title</caption>
  <tr>
    <th>first name</th>
    <th>last name</td>
    <th>gender</th>
  </tr>
  <tr>
    <td>Matej</td>
    <td>Kala</td>
    <td>male</td>      
  </tr>
</table>

```html
<table>
  <caption>table title</caption>
  <tr>
    <th>first name</th>
    <th>last name</td>
    <th>gender</th>
  </tr>
  <tr>
    <td>Matej</td>
    <td>Kala</td>
    <td>male</td>      
  </tr>
</table>
```

### css

- `width`
- `text-align`
- `border`
- `border-spacing`
- `border-collpase`
  - `collapse`
- `padding`
- `tr:nth-child(even)` or `(odd)` styles only overy evn or odd row


```css
table, th, td {
  border: 1 px solid #000;
  border-collpase: collapse;
}

th, td {
  padding: 10px;
}
tr:nth-child(even) {
  background-color: #fff;
}
tr:nth-child(odd) {
  background-color: #ddd;
```

