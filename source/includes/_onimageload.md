# onImageLoad

```javascript
/*
  jQuery custom onImageLoad method:
*/

jQuery.fn.extend({
    onImageLoad : function (func){
        var img_src = $(this).attr('src');
        if (img_src && typeof img_src !== 'undefined' && img_src !== '') {
            var img = new Image();
            img.src = img_src;

            if( jQuery.isFunction(func) ) {
                if (img.complete || img.readyState === 4) {
                    // image is cached
                    func();
                } else {
                    $(img).on('load', func);
                }
            }
        }
    }
});

/*
  Example usages:
*/

$('#some_element').onImageLoad(function(){
    console.log('image loaded');
});

```

```css
/*
    No CSS needed
*/
```

```html
    <!-- No HTML needed -->
```

onImageLoad is a custom jQuery method that calls a function when an image has loaded.
