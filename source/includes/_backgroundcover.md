# backgroundCover

```javascript
/*
  backgroundCover object:
*/

var backgroundCover = {
    init : function(){
        //set styles on elements if they haven't been set with CSS
        $('.mimic-cover').css({'position': 'relative', 'overflow': 'hidden'})
        $('.mimic-cover img.cover').css({'position': 'absolute'})
        $('div.mimic-cover').each(function(){
            var par = $(this);
            var img = $(this).find('img.cover');
            var loadimg = new Image();
                loadimg.src = $(img).attr('src');
            if (loadimg.complete || loadimg.readyState === 4) {
                // image is cached
                backgroundCover.getDims(par, img);
            }
            else {
                $(loadimg).on('load',function(){
                    backgroundCover.getDims(par, img);
                });
            }
        });
    },
    getDims : function(par, img){
        var loadimg = new Image();
        loadimg.src = $(img).attr('src');
        var img_w = loadimg.width;
        var img_h = loadimg.height;
        var img_ratio = img_w / img_h;
        backgroundCover.size(par, img, img_ratio);
        $(window).resize(function(){
            backgroundCover.size(par, img, img_ratio);
            setTimeout(function(){
                backgroundCover.size(par, img, img_ratio);
            }, 200) // try to catch slick
        });
    },
    size : function(par, img, image_ratio){
        var par_w = par.width();
        var par_h = par.height();
        var par_ratio = par_w / par_h;
        if(image_ratio > par_ratio){
            img.addClass('y').removeClass('x').css('margin-left',  -img.width() / 2);
        } else {
            img.addClass('x').removeClass('y').css('margin-top', -img.height() / 2);
        }
    }
}

/*
  Example usages:
*/

//initialize backgroundCover
$(document).ready(function(){
    backgroundCover.init();
});

```

```css
.mimic-cover{
    position: relative;
    overflow: hidden;
}
.mimic-cover img{
    position: absolute;
    width: 100%;
    height: auto;
    top: 0px;
    left: 0px;
    z-index: 0;
}
.mimic-cover img.x{
    top: 50%;
    width: 100%;
    height: auto;
    left: 0px;
    margin-left: 0px !important;
}
.mimic-cover img.y{
    top: 0;
    width: auto;
    height: 100%;
    left: 50%;
    margin-top: 0px !important;
}
```

```html
    <div class="mimic-cover">
        <img class="cover" href="some_image.jpg">
    </div>
```

backgroundCover emulates background size cover on any image element with the class cover whose parent has the class mimic-cover.
