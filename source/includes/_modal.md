# modal

```javascript
/*
  modal object:
*/
var modal = {
    modal_id: '#modal',
    modal_bg_id: '#modal-bg',
    modal_wrap_id: '#modal-wrap',
    modal_min_top_btm_margin: 60,
    has_video_banner: false,
    center: function() {
        var mod_h = $(modal.modal_id).height();
        var total_modal_h = mod_h + (modal.modal_min_top_btm_margin * 2)
        var bg_height;
        var scrollTop = $(document).scrollTop();
        if (windowDimensions.getWindowHeight() < total_modal_h) {
            // modal is taller than viewport. Just offset it from the top of the vieport
            $(modal.modal_wrap_id).css({
                // offset the top of our modal
                'top': scrollTop,
                'margin-bottom': modal.modal_min_top_btm_margin
            });
        } else {
            // there is enough room to center our modal
            $(modal.modal_wrap_id).css({
                'top': (scrollTop + (windowDimensions.getWindowHeight() / 2) - (total_modal_h / 2)),
            });
        }
        // if modal height is larger than document height, set bg to modal height. else use doc height
        if (total_modal_h > windowDimensions.getDocumentHeight()) {
            bg_height = total_modal_h;
        } else {
            bg_height = windowDimensions.getDocumentHeight();
        }
        $(modal.modal_bg_id).css({
            'height': bg_height,
            'position': 'absolute'
        });
    },
    slide: function(direction) {
        if (direction == "down") {
            $(modal.modal_id).css('margin-top', -windowDimensions.getWindowHeight());
            $(modal.modal_id).animate({
                'margin-top': '0px'
            }, 500);
        } else {
            $(modal.modal_id).animate({
                'margin-top': -windowDimensions.getWindowHeight()
            }, 500, function() {
                $('#modal-content').html();
            });
        }
    },
    show: function(markup, cb, add_class, no_close_button) {
        if (typeof add_class === 'undefined') {
            add_class = '';
        }
        var modal_markup = '<div id="modal-bg" class="' + add_class + '">\
        <div id="modal-holder">\
        <div id="modal-wrap">\
        <a id="modal-close"></a>\
        <div id="modal">\
        <div id="modal-content"> ' + markup + '</div>\
        </div>\
        </div>\
        </div>\
        </div>';
        $('body').prepend(modal_markup);
        $('.outer_wrap').css('overflow', 'hidden');
        $('#modal-bg').fadeIn(500, function() {
            if (typeof no_close_button === 'undefined' || no_close_button === false) {
                $('#modal-close').css('display', 'block');
            } else {
                $('#modal-close').css('display', 'none');
            }
            if (typeof cb == "function") {
                cb();
            }
        });
        modal.center();
        $(window).resize(function() {
            modal.center();
        });
        $('#modal').on('click.preventBubble').on('click.preventBubble', function(e) {
            e.stopPropagation();
        });
        $('#modal-bg, #modal-close').on('click.closeModal').on('click.closeModal', function(e) {
            e.preventDefault();
            modal.hide();
        });
    },
    hide: function(cb) {
        $('#outer-wrap').css('overflow', 'auto');
        $('#modal-bg').fadeOut(300, function() {
            $('#modal-bg').remove();
            if (typeof(cb) == 'function') {
                cb();
            }
        });
    }
};


/*
  Example usages:
*/

//trigger a modal
modal.show('<h1>Modal Example</h1>');


```

```css
#modal-bg {
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    display: none;
    z-index: 500;
    background-color: rgba(0, 0, 0, .9);
    box-sizing: border-box;
}
#modal-holder {
    text-align: center;
    margin: 0px auto;
    width: 100%;
    position: relative;
    z-index: 1000;
    width:100%;
    max-width: 960px;
    height: 100%;
}
#modal-wrap {
    z-index: 1000;
    width: 100%;
    max-width: 960px;
    height: 0px;
    padding: 0px 10px;
    float: left;
    position: absolute;
    left: 0px;
    padding-top:60px;
    box-sizing: border-box;
}
#modal {
    width: 100%;
    box-sizing: border-box;
    min-height: 100px;
    float: left;
    position: relative;
    z-index: 501;
}
#modal-content{
    text-align: left;
    float: left;
    display: inline;
    box-sizing: border-box;
    width: 100%;
    background-color: #fff;
    padding: 20px;
}
#modal-close {
    cursor: pointer;
    top: -40px;
    top: 15px;
    position: absolute;
    right: 10px;
    color: #fff;
    font-size: 24px;
    width: 28px;
    line-height: 28px;
    -webkit-transition: opacity .2s ;
    -moz-transition: opacity .2s ;
    -ms-transition: opacity .2s ;
    -o-transition: opacity .2s ;
    transition: opacity .2s ;
    opacity: 0.8;
    border: 3px solid #fff;
    border-radius: 180px;
}
#modal-close:before {
    content: '×';
    font-family: Arial;
}
#modal-close:hover,
#modal-close:active  {
    opacity: 1;
}
```

```html
<!-- No HTML needed -->
```

modal is used to render a responsive modal.

modal utilizes the following common Jell JS:

* windowDimensions
