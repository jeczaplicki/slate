# validateNumeric

```javascript
/*
  validateNumeric function:
*/

function validateNumeric(value){
    return !isNaN(parseFloat(value)) && isFinite(value);
}

/*
  Example usages:
*/

var some_value = 123;
var is_valid = validateNumeric(some_value);

```

```css
/*
    No CSS needed
*/
```

```html
    <!-- No HTML needed -->
```

validateNumeric is a function that returns true or false for whether a value is a valid numeric value.
