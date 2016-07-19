# styledCheckboxesRadios

```javascript
/*
  styledCheckboxesRadios object:
*/

var styledCheckboxesRadios = {
    focused_class: "focused",
    set: function() {
        $('input[type=checkbox]').each(function(index) {
            styledCheckboxesRadios.styleInputs($(this), 'checkbox');
        });
        $('input[type=radio]').each(function(index) {
            styledCheckboxesRadios.styleInputs($(this), 'radio');
        });
    },
    styleInputs: function(el, type) {
        var checked_markup;
        var unchecked_markup;
        var el_type_styled_class;
        // - add custom styles to checkboxes by adding the attibute styled-class="your_class_name"
        //first, we'll check to see if there is an attribute
        var styled_class = $(el).parent().attr('styled-class');
        // if the attribute doesn't exist, redefine styled_class to nothing.
        if (typeof styled_class === 'undefined' || styled_class === false) {
            styled_class = '';
        }
        if (type == 'checkbox') {
            checked_markup = '<span class="styled_checkbox checked"><span class="fill"></span></span>';
            unchecked_markup = '<span class="styled_checkbox"><span class="fill"></span></span>';
            el_type_styled_class = '.styled_checkbox';
        } else if (type == 'radio') {
            checked_markup = '<span class="styled_radio checked"><span class="fill"></span></span>';
            unchecked_markup = '<span class="styled_radio"><span class="fill"></span></span>';
            el_type_styled_class = '.styled_radio';
        }
        var parent = $(el).parent();
        // if statement to prevent doubling up
        if (!$(parent).hasClass('stylized')) {
            $(parent).addClass('stylized');
            if (styled_class !== '') {
                $(parent).addClass(styled_class);
            }
            if ($(el).is(':checked')) {
                $(parent).prepend(checked_markup);
            } else {
                $(parent).prepend(unchecked_markup);
            }
            // if the checkbox or radio is disabled
            var attr = $(el).attr('disabled');
            if (typeof attr !== 'undefined' && attr !== false) {
                $(el).parent().addClass('disabled')
            }
            var pretty_checkbox = $(parent).find(el_type_styled_class)
            // look for changes on the checkbox and update our pretty checkbox
            $(el).change(function() {
                if (type == 'radio') {
                    //jquery does not fire on uncheck of radio elements.
                    //Therefore, we're going to grab the name of the group, find the styled class, and remove it.
                    //Then we'll apply the selected style to the radio where the change fired.
                    var radioGroup = $(el).attr('name');
                    $('input[name="' + radioGroup + '"]').parent().find(el_type_styled_class).removeClass('checked');

                }
                if ($(el).is(':checked')) {
                    $(pretty_checkbox).addClass('checked')
                } else {
                    //prevent unclick of data-level 1 checkboxes
                    $(pretty_checkbox).removeClass('checked');
                }
            });
            $(el).focus(function() {
                $(pretty_checkbox).addClass(styledCheckboxesRadios.focused_class);
                $(el).blur(function() {
                    $(pretty_checkbox).removeClass(styledCheckboxesRadios.focused_class);
                });
            });
        }
    }
}

/*
  Example usages:
*/

//initialize styledCheckboxesRadios
$(document).ready(function(){
    styledCheckboxesRadios.set();
});

```

```css
input[type="checkbox"],
input[type="radio"] {
    -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=0)";
    filter: alpha(opacity=0);
    opacity: 0;
    position: absolute;
    z-index: inherit;
    margin-left: -28px;
    margin-top: 2px;
    cursor: pointer;
    padding: 0;
    width: 20px;
    height: 20px;
}
.styled_checkbox,
.styled_radio {
    border: solid 1px #d1d1d1;
    position: relative;
    display: inline-block;
    margin-right: 4px;
    cursor: pointer;
    background-color: #fff;
    bottom:-3px;
    -webkit-border-radius: 4px;
    -moz-border-radius: 4px;
    border-radius: 4px;
}
.styled_radio {
    width: 18px;
    height: 18px;
}
.styled_checkbox {
    width:18px;
    height:18px;
}
.styled_checkbox.focused {
    -webkit-box-shadow: 0px 0px 5px #9CF;
    -moz-box-shadow: 0px 0px 5px #9CF;
    box-shadow: 0px 0px 5px #9CF;
}
.styled_checkbox .fill {
    display:none;
}
.styled_checkbox.checked .fill {
    display: block;
    color: #000;
    font-size: 8px;
    width: 12px;
    height: 12px;
    background-color: #0078BF;
    position: absolute;
    top: 50%;
    left: 50%;
    margin-left: -6px;
    margin-top: -6px;
    -webkit-border-radius: 3px;
    -moz-border-radius: 3px;
    border-radius: 3px;
}
label.stylized {
    padding-left: 23px;
    text-indent: -23px;
}
.styled_radio {
    -webkit-border-radius: 12px;
    -moz-border-radius: 12px;
    -o-border-radius: 12px;
    border-radius: 12px;
    position: relative;
    top: 2px;
}
.styled_radio .fill {
    display:none;
}
.styled_radio.checked {
    background-color: #fff;
}
.styled_radio.checked .fill {
    display: block;
    -webkit-border-radius: 12px;
    -moz-border-radius: 12px;
    -o-border-radius: 12px;
    border-radius: 12px;
    width: 12px;
    height: 12px;
    background-color: #0078BF;
    position: absolute;
    top: 50%;
    left: 50%;
    margin-left: -6px;
    margin-top: -6px;
}
.disabled,
.disabled span.styled_radio,
.disabled span.styled_checkbox {
    -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=40)";
    filter: alpha(opacity=40);
    opacity: .4;
    cursor: auto;
}
.styled_radio.focused {
    -webkit-box-shadow: 0px 0px 5px #9CF;
    -moz-box-shadow: 0px 0px 5px #9CF;
    box-shadow: 0px 0px 5px #9CF;
}
```

```html
    <div>
        <input type="checkbox" name="checkbox_example" id="checkbox_example" value="example">
        <label for="checkbox_example">Checkbox Example</label>
    </div>
    <div>
        <input type="radio" name="radio_example" id="radio_example" value="example">
        <label for="radio_example">Radio Example</label>
    </div>
```

styledCheckboxesRadios replaces standard checkboxes and radios with a stylized replacement. It acts on all inputs of the type radio and checkbox.
