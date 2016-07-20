# cookieController

```javascript
/*
  cookieController object:
*/

var cookieController = {
    setCookie : function(name, value, days) {
        if (days) {
            var date = new Date();
            date.setTime(date.getTime() + (days * 24 * 60 * 60 * 1000));
            var expires = "; expires=" + date.toGMTString();
        } else
            var expires = "";
        document.cookie = name + "=" + value + expires + "; path=/";
    },
    getCookie : function(name) {
        var nameEQ = name + "=";
        var ca = document.cookie.split(';');
        for (var i = 0; i < ca.length; i++) {
            var c = ca[i];
            while (c.charAt(0) == ' ')
                c = c.substring(1, c.length);
            if (c.indexOf(nameEQ) == 0)
                return c.substring(nameEQ.length, c.length);
        }
        return null;
    },
    deleteCookie : function(name) {
        cookieController.setCookie(name, "", -1);
    }
}

/*
  Example usages:
*/

//set cookie
cookieController.setCookie('cookiename', 'value', 5);

//get cookie by name
var current_cookie = cookieController.getCookie('cookiename');

//delete cookie
cookieController.deleteCookie('cookiename');

```

```css
/*
    No CSS needed
*/
```

```html
<!-- No HTML needed -->
```

cookieController is a set of functions used to read and modify cookies.


