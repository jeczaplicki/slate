# hashController

```javascript
/*
  hashController object:
*/

var hashController = {
    setHash : function(the_hash) {
        the_hash = the_hash.replace('#', '');
        window.location.hash = the_hash;
    },
    setHashFromElementAttribute : function(el, attribute) {
        var the_hash = el.attr(attribute);
        the_hash = the_hash.replace('#', '');
        window.location.hash = el.attr(attribute);
    },
    getHash : function() {
        var hash = (location.href.split("#")[1] || "");
        if (hash != '') {
            return hash;
        } else {
            return false;
        }
    },
    clearHash : function() {
        if ("pushState" in history) {
            // html5 compatilbe : remove the # too
            history.pushState("", document.title, window.location.pathname);
        } else {
            // not html5 : leaves #
            window.location.hash = "";
        }
    }
}

/*
  Example usages:
*/

//set hash
hashController.setHash('somehash');

//get hash
var current_hash = hashController.getHash();

//clear hash
hashController.clearHash();

//set hash to an elements attribute
hashController.setHashFromElementAttribute($('#some_element'), 'data-slug');

```

```css
/*
    No CSS needed
*/
```

```html
    <!-- No HTML needed -->
```

hashController is a set of functions used to read and modify the hash.


