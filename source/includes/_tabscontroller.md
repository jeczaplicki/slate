# tabsController

```javascript
/*
  tabsController object:
*/
var tabsController = {
    init : function() {
        $('.tabs').each(function() {
            var hash = hashController.getHash();
            var no_hash = true;
            // determine if there is tab content
            if ( $(this).find('.tab_content').length ) {
                var parent = $(this);
                var tab_wrap = $(this).find('.section-tab-wrapper');
                var parent = $(this);
                $(parent).find('.tab_nav a').each(function() {
                    var href = $(this).attr('href').replace('#', '') ;
                    // if there is a hash in the URL, show that content
                    if (href == hash) {
                        no_hash = false;
                        tabsController.showContent($(this), parent);
                    }
                    $(this).off('click.tabs').on('click.tabs', function(e) {
                        e.preventDefault();
                        if (parent.hasClass('tabs--show_select_menu') === false){
                            hashController.setHashFromElementAttribute($(this), 'href');
                            tabsController.showContent($(this), parent);
                        }
                    })
                });
                if ($(this).find('.tab_nav .current').length === 0) {
                    tabsController.showContent($(this).find('.tab_nav li a').eq(0), parent);
                }
                if(parent.data('tabs-responsive') === true) {
                    tabsController.initSelectMenu(parent);
                }
            }
        });
    },
    showContent : function(el, parent) {
        if ( el.length) {
            if (typeof $(el).attr('href') !== 'undefined') {
                var href = $(el).attr('href').replace('#', '');
                $(parent).find('.tab_nav a').removeClass('current');
                $(el).addClass('current');
                $(parent).find('.tab_content').addClass('hide');
                $(parent).find('.' + href).removeClass('hide');
            } else if (typeof $(el).data('tab') !== 'undefined') {
                var tab = $(el).data('tab').replace('#', '');
                $(parent).find('.tab_nav a').removeClass('current');
                $(parent).find('.tab_nav a[href="'+$(el).data('tab')+'"]').addClass('current');
                $(el).prop('selected', true);
                $(parent).find('.tab_content').addClass('hide');
                $(parent).find('.' + tab).removeClass('hide');
            }
        }
    },
    initSelectMenu : function(parent) {
        var selectMenuEl = tabsController.buildSelectMenuElement(parent);
        if(selectMenuEl !== false) {
            var selectMenuWrapper = $('<div class="tab_nav-select_menu--wrapper">');
            selectMenuWrapper.append(selectMenuEl);
            $(parent).find('.tab_nav').after(selectMenuWrapper);
            styledSelects.init();
            selectMenuEl.off('change.tabSelectOption').on('change.tabSelectOption', function(){
                var selected_option = $(this).find(':selected');
                hashController.setHashFromElementAttribute(selected_option, 'data-tab');
                tabsController.showContent(selected_option, parent);
            });
        }

        tabsController.initTabHeightWatch(parent);
    },
    buildSelectMenuElement : function(parent) {
        var selectOptions = tabsController.collectSelectMenuOptions(parent);
        if(selectOptions.length > 0){
            var selectEl = $('<select class="tab_select_menu">');
            $.each(selectOptions, function(index, option){
                selectEl.append( $('<option>').attr('value', option.text).text(option.text).attr('data-tab', option.val).attr('selected', option.selected).trigger('change') );
            });
            return selectEl;
        }
        return false;
    },
    collectSelectMenuOptions : function(parent) {
        var options = [];
        $(parent).find('.tab_nav a').each(function(){
            var thisOption = {};
            thisOption.text = $(this).text();
            thisOption.val = $(this).attr('href');
            if($(this).hasClass('current') === true){
                thisOption.selected = true;
            } else {
                thisOption.selected = false;
            }
            options.push(thisOption);
        });
        return options;
    },
    initTabHeightWatch : function(parent){
        tabsController.evalTabHeight(parent);
        $(window).resize(function() {
            tabsController.evalTabHeight(parent);
        });
    },
    evalTabHeight : function(parent) {
        var tabsNavEl = $(parent).find('.tab_nav').eq(0);
        var tabs = $(tabsNavEl).find('li');
        var properTabPosition = 0;
        var numTabs = tabs.length;
        var properState = 'tabs';
        for(var i = 0; i < numTabs; i++){
            var curTab = tabs[i];
            if(i === 0){
                properTabPosition = $(curTab).offset().top;
            } else {
                if($(curTab).offset().top !== properTabPosition){
                    properState = 'select_menu'
                }
            }
        }
        if(properState === 'tabs') {
            // tabs
            $(parent).removeClass('tabs--show_select_menu tabs--init');
        } else {
            // select menu
            $(parent).addClass('tabs--show_select_menu').removeClass('tabs--init');
            // update the select menu selected value
            var current_tab = $(parent).find('li a.current');
            var current_tab_text = current_tab.text();
            var current_tab_value = current_tab.attr('href');
            $(".select .val").text(current_tab_text);
            // select option with this value as well
            $(parent).find('.tab_select_menu option').each( function(index) {
                if ($(this).data('tab') === current_tab_value) {
                    $(this).prop('selected', true);
                }
            });
        }
    }
}


/*
  Example usages:
*/

//initialize tabsController
$(document).ready(function(){
    tabsController.init();
});

```

```css
.tab_nav li{
    display: inline-block;
    background-image: none;
    padding-left: 0px;
}
.tabs ul {
    border-bottom: solid 1px #ccc;
    position: relative;
    width: 100%;
    margin: 40px auto;
    max-width: 100%;
    box-sizing: border-box;
    padding: 0;
}
.tabs li {
    display: inline-block;
}
.tabs li a {
    font-family: Arial, Helvetica, sans-serif;
    position: relative;
    background-color: #fff;
    border: 1px solid #ccc;
    border-bottom: 1px solid transparent;
    padding: 16px 12px;
    border: 1px solid #ccc;
    padding: 0px 26px;
    line-height: 56px;
    display: block;
    position: relative;
    top: 1px;
    text-transform: uppercase;
    color: #888;
    -webkit-transition: none;
    -moz-transition: none;
     -ms-transition: none;
    -o-transition: none;
    transition: none;
    -webkit-transition: color .2s ;
    -moz-transition: color .2s ;
     -ms-transition: color .2s ;
    -o-transition: color .2s ;
    transition: color .2s ;
}
.tabs li a:hover{
    color: #92231e;
}
.tabs li a.current{
    border-bottom: 1px solid #FFF;
    color: #92231e;
}
.tabs .hide{
    display:none;
}
.tabs .tab_content {
    width: 83.3333333%;
    margin: 40px auto;
    overflow: hidden;
}
.tabs.tabs--init {
    position: initial;
}
.tabs .select {
    display: none;
}
.tabs.tabs--show_select_menu .select {
    display: block;
}
.tabs.tabs--show_select_menu .tab_nav,
.tabs.tabs--init .tab_nav {
    position: absolute !important;
    width: 100% !important;
    left: 0;
    opacity: 0;
    z-index: -1 !important;
}
.tabs.tabs--show_select_menu .tab_nav a {
    cursor: default;
}

```

```html
<div class="tabs" data-tabs-responsive="true">
    <ul class="tab_nav">
        <li><a href="#one">Tab One</a></li>
        <li><a href="#two">Tab Two</a></li>
        <li><a href="#three">Tab Three</a></li>
    </ul>
    <div class="tab_content one">
        Tab Content 1
    </div>
    <div class="tab_content two">
        Tab Content 2
    </div>
    <div class="tab_content three">
        Tab Content 3
    </div>
</div>
```

tabsController handles tabbed content. If the tabs do not fit in the viewport then the tabs will be automatically converted to a select menu, assuming you include the tabs-responsive html data attribute.

tabsController utilizes the following common Jell JS:

* hashController
* styledSelects

