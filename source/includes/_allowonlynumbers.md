# allowOnlyNumbers

```javascript
/*
  jQuery custom allowOnlyNumbers method:
*/

jQuery.fn.extend({
    allowOnlyNumbers : function (decimal, dash){
        $(this).off('keydown.allowOnlyNumbers').on('keydown.allowOnlyNumbers', function(event) {
            /* Allow: backspace, delete, tab, escape, and enter */
            if ( event.keyCode == 46 || event.keyCode == 8 || event.keyCode == 9 || event.keyCode == 27 || event.keyCode == 13 ||
                /* Allow decimal */
                ( decimal== true && event.keyCode == 190 || decimal== true && event.keyCode == 110)  ||
                /* Allow dashes */
                ( dash && event.keyCode == 109 || dash && event.keyCode == 189)  ||
                /* Allow: Ctrl+A */
                (event.keyCode == 65 && event.ctrlKey === true) ||
                /* Allow: home, end, left, right */
                (event.keyCode >= 35 && event.keyCode <= 39)) {
                /* let it happen, don't do anything */
                    return;
            }
            else {
                /* Ensure that it is a number and stop the keypress */
                if (event.shiftKey || (event.keyCode < 48 || event.keyCode > 57) && (event.keyCode < 96 || event.keyCode > 105 )) {
                    event.preventDefault();
                }
            }
        });
    }
});

/*
  Example usages:
*/

//disallow decimals and dashes
$('#some_element').allowOnlyNumbers();

//allow decimals
$('#some_element').allowOnlyNumbers(true, false);

//allow dashes
$('#some_element').allowOnlyNumbers(false, true);

//allow decimals and dashes
$('#some_element').allowOnlyNumbers(true, true);

```

```css
/*
    No CSS needed
*/
```

```html
    <!-- No HTML needed -->
```

allowOnlyNumbers is a custom jQuery method that restricts a field to numbers with the option of allowing decimals and/or dashes.




