# svgReplacement

```javascript
/*
  svgReplacement function:
*/

function svgReplacement() {
    // disable svg images for old android, or any other browser that does not support it
    if (!document.implementation.hasFeature("http://www.w3.org/TR/SVG11/feature#Image", "1.1")) {
        $('.svg').addClass('nosvg');
    }
}

/*
  Example usages:
*/

//initialize backgroundCover
$(document).ready(function(){
    svgReplacement();
});

```

```css
/*
    Example CSS:
*/

#logo{
    text-align: center;
    display: inline-block;
    float: left;
    width: 180px;
    height: 22px;
    margin-top: 34px;
}
#logo.nosvg{
    background-image: url(fallback_logo.png);
    background-repeat: no-repeat;
    background-size:100% auto;

}
* .nosvg img{
    display:none;
}
```

```html
    <!-- Example HTML -->
    <a id="logo" class="svg" href="/"><img src="logo_image.svg"></a>
```

svgReplacement swaps out SVG images for raster images if the browser does not support SVGs.
