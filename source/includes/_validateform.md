# validateForm

```javascript
/*
  jQuery custom validateForm method:
*/

jQuery.fn.extend({
    validateForm : function(warningLabels, cb) {
        var is_form = false
        if ($(this).is("form")) {
            is_form = true;
        }
        $('span.warning, span.email-warning, span.phone-warning').remove();
        $(this).find('.highlight').removeClass('highlight');
        var errors = 0;
        $(this).find('.is-required').each(function(index) {
            var type = $(this).attr('type');
            var val = $(this).val();
            if (typeof type === 'undefined' || type === false) {
                //if no type, this is a textarea
                type = "textarea";
            }
            if (type == "text" ||  type == "textarea" || type == "password") {
                if (val == '') {
                    errors++;
                    $(this).addClass('highlight')
                    if (warningLabels) {
                        $(this).after('<span class="warning">This field is required.</span> ')
                    }
                }
            }
            if (type == "email") {
                var atpos = val.indexOf("@");
                var dotpos = val.lastIndexOf(".");
                if (val == '') {
                    errors++;
                    $(this).addClass('highlight')
                    if (warningLabels) {
                        $(this).after('<span class="warning">This field is required.</span> ')
                    }
                } else if (atpos < 1 || dotpos <= atpos + 2 || dotpos + 2 >= val.length || val.search(' ') != -1) {
                    // not valid
                    errors++;
                    $(this).addClass('email-highlight')
                    if (warningLabels) {
                        $(this).after('<span class="email-warning">Enter a valid email address.</span> ')
                    }
                }
            }
            if (type == "tel") {
                var cleaned_val = val.replace( /^\D+/g, '');
                if (val == '') {
                    errors++;
                    $(this).addClass('highlight')
                    if (warningLabels) {
                        $(this).after('<span class="warning">This field is required.</span> ')
                    }
                    // make sure there are at least 10 numbers
                } else if ( val.length < 10 ) {
                    // not valid
                    errors++;
                    $(this).addClass('phone-highlight')
                    if (warningLabels) {
                        $(this).after('<span class="phone-warning">Enter a valid phone number.</span> ')
                    }
                }
            }
        });
        if (errors > 0) {
            if (errors == 1) {
                alert('There is an error in this form. Please fix it and resubmit.');
            } else {
                alert('There are some errors in this form. Please fix them and resubmit.');
            }
            $('.highlight').each(function() {
                $(this).keyup(function() {
                    $(this).confirm_field();
                });
            })
            $('.email-highlight').each(function() {
                $(this).keyup(function() {
                    $(this).confirm_email();
                });
            });
            $('.phone-highlight').each(function() {
                $(this).keyup(function() {
                    $(this).confirm_phone();
                });
            });
            return false;
        } else {
            // SUBMIT THE FORM
            if (cb != null) {
                cb();
            } else {
                if (is_form) {
                    $(this).submit();
                }
                return true;
            }
        }
    },
    /*
        -- CONFIRM FIELDS --
        clears out errors after a form validation
    */
    confirm_field : function() {
        var val = $(this).val();
        if (val != '') {
            $(this).removeClass('highlight');
            $(this).parent().find('.warning').remove();
        }
    },
    /*
        -- CONFIRM EMAIL + PHONE --
        Small helper before and following validation.
    */
    confirm_email : function() {
        var val = $(this).val()
        var atpos = val.indexOf("@");
        var dotpos = val.lastIndexOf(".");
        if (atpos > 0 && dotpos >= atpos + 2 && val.length - 2 > dotpos && val.search(' ') == -1) {
            // valid
            $(this).addClass('valid');
            $(this).removeClass('email-highlight');
            $(this).parent().find('.email-warning').remove();
        } else {
            $(this).removeClass('valid');
        }
    },
    confirm_phone : function() {
        var val = $(this).val()
        var cleaned_val = val.replace( /^\D+/g, '');
            if (val.length >= 10) {
                //  valid
                $(this).removeClass('phone-highlight');
                $(this).parent().find('.phone-warning').remove();
          }
    }
});

/*
  Example usages:
*/

//simply validate (no warning labels or callback)
$('#some_element').validateForm();

//validate and show warning labels
$('#some_element').validateForm(true);

//validate, show warning labels, and pass a callback to be used instead of submitting form
var callback = function(){};
$('#some_element').validateForm(true, callback);
```

```css
input.highlight {
    background-color:#efdedd;
}
span.warning,
span.email-warning,
span.phone-warning {
    color:#92231e;
    font-size:16px;
    display:inline-block;
    margin-top:6px;
}
```

```html
<!-- example of various input elements that can be validated by validateForm -->
<div id="some_element">
    <!-- required text field, cannot be empty -->
    <label for="name">Name</label>
    <input name="name" type="text" id="name" class="is-required">
    <!-- required email field, cannot be empty, must pass email validation code -->
    <label for="email">Email</label>
    <input name="email" type="email" id="email" class="is-required">
    <!-- required telephone field, cannot be empty, must pass phone validation code -->
    <label for="telephone">Telephone</label>
    <input name="telephone" type="tel" id="telephone" class="is-required">
    <!-- required textarea, cannot be empty -->
    <label for="comments">Question(s)</label>
    <textarea name="comments" id="comments" class="is-required"></textarea>
</div>
```

validateForm is a custom jQuery method that validates a form or a series of form elements. It has the option for showing warning/error labels. It also has the option to call a callback function instead of submitting the form.

The JS also includes the following jQuery helper methods:

* confirm_field
* confirm_email
* confirm_phone

The helper methods above are used for updating the error state of the form after initial validation.

