# styledSelects

```javascript
/*
  styledSelects object:
*/

var styledSelects = {
    init: function() {
        $('select').each(function() {
            // if statement to prevent doubling up
            if (!$(this).hasClass('stylized') && $(this).attr('multiple') !== 'multiple') {
                $(this).addClass('stylized');
                // there are different colors and sizes of selects, can be used in combination:
                // - default, which requires no additional markup
                //first, we'll check to see if there is an attribute
                var styled_class = $(this).attr('styled-class');
                // if the attribute doesn't exist, redefine styled_class to nothing.
                if (typeof styled_class === 'undefined' || styled_class === false) {
                    styled_class = '';
                }
                var select_width = $(this).outerWidth();
                var widthstyle = '';
                $(this).wrap('<span class="select ' + styled_class + '" ' + widthstyle + ' />');
                var orig_select = $(this);
                var parent_span = $(this).parent();
                $(orig_select).before('<span class="val"></span>');
                $(orig_select).before('<span class="stylized_arrow"></span>');
                var value = $(parent_span).find('.val');
                $(parent_span).click(function() {
                    $(this).find('select').focus();
                });
                //initially set up span to mimic select
                var found_select = false;
                var display_option = $(orig_select).find('option:first-child').html();
                $(this).find('option').each(function() {
                    if ($(this).is(':selected')) {
                        display_option = $(this).html();
                    }
                })
                $(value).html(display_option);
                //update the span on change
                $(orig_select).on('change', function() {
                    display_option = $(this).find('option:selected').html();
                    $(value).html(display_option);
                    $(orig_select).blur();
                });
                // add keypress events
                $(orig_select).keyup(function() {
                    display_option = $(this).find('option:selected').html();
                    $(value).html(display_option);
                });
                //accessibility
                $(orig_select).focus(function() {
                    $(parent_span).addClass('focused');
                    $(this).blur(function() {
                        $(parent_span).removeClass('focused');
                    })
                });
            }
        });
    },
    reinit: function(el) {
        var parent_span = $(el).parent();
        var value = $(parent_span).find('.val')
        $(el).on('change', function() {
            var display_option = $(this).find('option:selected').html();
            $(value).html(display_option);
            $(el).blur();
        });
        //accessibility
        $(el).focus(function() {
            $(parent_span).addClass('focused');
            $(this).blur(function() {
                $(parent_span).removeClass('focused');
            })
        });
    }
}

/*
  Example usages:
*/

//initialize styledSelects
$(document).ready(function(){
    styledSelects.init();
});

```

```css
span.select {
    border: solid 1px #ccc;
    position: relative;
    background-color: transparent;
    padding: 12px 44px 12px 12px;
    display: inline-block;
    cursor: pointer;
    overflow: hidden;
    text-transform: uppercase;
    min-width: 155px;
}
span.select.focused {
    -webkit-box-shadow: 0px 0px 5px #9CF;
    -moz-box-shadow: 0px 0px 5px #9CF;
    box-shadow: 0px 0px 5px #9CF;
    border: solid 1px #000;
}
span.select .val {
    font-size: 14px;
    cursor: pointer;
    white-space: nowrap;
    float: left;
    width: 100%;
    overflow: hidden;
    line-height: normal;
    color: #666;
}
span.select select {
    position: absolute;
    width: 100%;
    top: 0px;
    left: 0px;
    opacity: 0;
    -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=0)";
    filter: alpha(opacity=0);
    cursor: pointer;
    top: 0px;
    left: 0px;
    background: none;
    font-size: .75em;
    height: 100%;
}
span.select select option{
    background-color:#fff;
}
.select .stylized_arrow {
    color: #666;
    position: absolute;
    right: 12px;
    font-size: 20px;
    top: 17px;
    content: "";
    width: 0;
    height: 0;
    display: inline-block;
    border-top: 10px solid #666;
    border-left: 10px solid transparent;
    border-right: 10px solid transparent;
}
span.select{
    width: 100%;
    box-sizing: border-box;
}
```

```html
<select>
    <option>Example Option 1</option>
    <option>Example Option 2</option>
    <option>Example Option 3</option>
</select>
```

styledSelects replaces standard select menus with a stylized replacement. It acts on all select menus.
