# ScrollToThis

```javascript
/*
  jQuery custom scrollToThis method:
*/

jQuery.fn.extend({
  scrollToThis: function(add_offset) {
    if (isNaN(add_offset)) {
        add_offset = 0;
    }
    var offset_height = add_offset;
    var speed = 500;
    $('html, body').animate({
        scrollTop: $(this).offset().top - offset_height
    }, speed);
  }
});

/*
  Example usage:
*/

$('#some_element').scrollToThis();
```

```css
/*
    No CSS needed
*/
```

```html
    <!-- No HTML needed -->
```

ScrollToThis is a custom jQuery method that triggers the page to scroll to the element it is called on with an optional offset.

<aside class="warning">Make sure that the selector you're using to call ScollToThis is not returning more than one element!</aside>


