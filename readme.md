API
---

Given a basic HTML form

```html
<form id="contact">
  <input name="user[email]" value="jsmith@example.com">
  <input name="user[pets][]" type="checkbox" value="cat" checked>
  <input name="user[pets][]" type="checkbox" value="dog" checked>
  <input name="user[pets][]" type="checkbox" value="bird">
  <input type="submit">
</form>
```

**.serializeObject** &mdash; serializes the selected form into a JavaScript object

```js
$('form#contact').serializeObject();
//=> {user: {email: "jsmith@example.com", pets: ["cat", "dog"]}}
```

**.serializeJSON** &mdash; serializes the selected form into [JSON][json]

```js
$('form#contact').serializeJSON();
//=> '{"user":{"email":"jsmith@example.com","pets":["cat","dog"]}}'
```

**FormSerializer.patterns** &mdash; modify the patterns used to match field
names

Many of you have requested to allow `-` in field names or use `.` to nest keys.
You can now configure these to your heart's content.

[Hyphen][dash-notation] example

```js
$.extend(FormSerializer.patterns, {
  validate: /^[a-z][a-z0-9_-]*(?:\[(?:\d*|[a-z0-9_-]+)\])*$/i,
  key:      /[a-z0-9_-]+|(?=\[\])/gi,
  named:    /^[a-z0-9_-]+$/i
});
```

[Dot-notation][dot-notation] example

```js
$.extend(FormSerializer.patterns, {
  validate: /^[a-z][a-z0-9_]*(?:\.[a-z0-9_]+)*(?:\[\])?$/i
});
```

*Validating and Key parsing*

* `validate` &mdash; only valid input names will be serialized; invalid names
  will be skipped

* `key` &mdash; this pattern parses all "keys" from the input name; You will
  want to use `/g` as a modifier with this regexp.

*Key styles*

* `push` &mdash; push a value to an array

  ```html
  <input name="foo[]" value="a">
  <input name="foo[]" value="b">
  ```

  ```js
  $("form").serializeObject();
  //=> {foo: [a, b]}
  ```

* `fixed` &mdash; add a value to an array at a specified index

  ```html
  <input name="foo[2]" value="a">
  <input name="foo[4]" value="b">
  ```

  ```js
  $("form").serializeObject();
  //=> {foo: [, , "a", , "b"]}
  ```

* `named` &mdash; adds a value to the specified key

  ```html
  <input name="foo[bar]" value="a">
  <input name="foo[bof]" value="b">
  <input name="hello" value="world">
  ```

  ```js
  $("form").serializeObject();
  //=> {foo: {bar: "a", bof: "b"}, hello: "world"}
  ```

