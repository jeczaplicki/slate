# showHideDefaults

```javascript
/*
    showHideDefaults object:
*/

var showHideDefaults = {
    init: function() {
        $('.input_default_wrap input, .input_default_wrap textarea').each(function() {

            if ( $(this).attr('type') != 'hidden' ) {
                var label = $(this).parent().find('label')
                showHideDefaults.check();
                setTimeout('showHideDefaults.check()', 500); // check again in case of autofill

                if ( ! $(this).hasClass('showhide') ) {
                    $(this).addClass('showhide');
                    var input = $(this);

                    $(label).click(function() {
                        input.focus();
                    })
                    $(this).on('keydown', function() {

                        if ($(this).val() == '') {
                            $(label).hide();
                        }
                    });
                    $(this).on('keyup', function() {
                        if ($(this).val() == '') {
                            $(label).show();
                        } else {
                            $(label).hide();
                        }
                    });

                    $(this).on('blur', function() {
                        if ($(this).val() == '') {
                            $(label).show();
                        }
                    });
                }
            }
        });

        showHideDefaults.styleRequired();

    },
    check : function() {
        $('.input_default_wrap input, .input_default_wrap textarea').each(function() {
            var label = $(this).parent().find('label')
            if ( $(this).attr('type') != 'hidden' ) {
                if ( $(this).val() != '' ) {
                    $(label).hide();
                }
            }
        });

    },

    styleRequired : function() {

        $('.input_default_wrap label').each(function() {
            var label_html = $(this).html();
            $(this).html( label_html.replace("*", "<span class='required'>*</span>") );
        });
    }
}

/*
    Example usage:
*/

$(document).ready(function() {
    showHideDefaults.init();
});

```

```css
.input_placeholder label,
.input_placeholder input {
    font-size: 20px;
}

.input_placeholder{
    position:relative;
}

.input_placeholder label {
    position: absolute;
    z-index: 5;
    width: 100%;
    overflow: hidden;
    top: 0px;
    left:0px;
    white-space: normal;
    color: #989898;
}

input,
textarea {
    -webkit-appearance: none;
    outline: none;
    border-radius: 0;
}

.input_default_wrap {
    position: relative;
}

.input_default_wrap label {
    position: absolute;
    top: 2px;
    z-index: 100;
    font-size: 16px;
    color: #999;
    overflow: hidden;
    width: 100%;
    padding: 15px;
    padding-right: 15px;
    white-space: normal;
    -moz-box-sizing: border-box;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
}

```

```html
<div class="input_default_wrap">
    <label for="name">Name</label>
    <input name="name" type="text" id="name">
</div>
```

showHideDefaults uses field labels as field placeholders and hides the placeholder when a user enters data into the field.
