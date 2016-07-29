# windowDimensions

```javascript
/*
  windowDimensions object:
*/

var windowDimensions = {
    win_width : 'undefined',
    win_height : 'undefined',
    doc_width : 'undefined',
    doc_height : 'undefined',
    init : function() {
        windowDimensions.updateDimensions();
        $(window).resize(function() {
            windowDimensions.updateDimensions();
        });
    },
    updateDimensions : function() {
        windowDimensions.win_width = windowDimensions.viewport().width;
        windowDimensions.win_height = $(window).height();
        windowDimensions.doc_width = $(document).width();
        windowDimensions.doc_height = $(document).height();
    },
    viewport : function() {
        var e = window,
            a = 'inner';
        if (!('innerWidth' in window)) {
            a = 'client';
            e = document.documentElement || document.body;
        }
        return {
            width: e[a + 'Width'],
            height: e[a + 'Height']
        };
    },
    getWindowWidth : function() {
        return windowDimensions.win_width;
    },
    getWindowHeight : function() {
        return windowDimensions.win_height;
    },
    getDocumentHeight : function() {
        return windowDimensions.doc_height;
    },
    getDocumentWidth : function() {
        return windowDimensions.doc_width;
    }
}

/*
  Example usages:
*/

//initialize windowDimensions
$(document).ready(function() {
    windowDimensions.init();
});

//get window width
var current_window_width = windowDimensions.getWindowWidth();

//get window height
var current_window_height = windowDimensions.getWindowHeight();

//get document width
var current_document_width = windowDimensions.getDocumentWidth();

//get document height
var current_document_height = windowDimensions.getDocumentHeight();

```

```css
/*
    No CSS needed
*/
```

```html
<!-- No HTML needed -->
```

windowDimensions is a set of functions used to track and retrieve window dimensions.


