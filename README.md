Zolipop
===========
2016-01-28


a modal dialog.


zoli can be installed as a [planet](https://github.com/lingtalfi/Observer/blob/master/article/article.planetReference.eng.md).



It depends on jquery;







Features
-------------

- simple to extend
- lightweight
- draggable plugin (including constrained in window default feature)



How to use?
---------------


### A working example

The example below shows how you can click on a link to open the popup.
You can also drag the popup around the screen, and its movements will be 
constrained inside the view port.



```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8"/>
    <script src="http://code.jquery.com/jquery-2.1.4.min.js"></script>
    <script src="/libs/zoli/js/zolipop.js"></script>
    <script src="/libs/zoli/js/zolipop.draggable.js"></script>
    <title>Here demo page</title>

    <style>
        .hide_me {
            display: none;
        }
        #the_ugliest_popup_ever{
            background: red;
        }
    </style>

</head>

<body>

<h1>Hello, this is zoli modal demo</h1>

<div>
    <p>
        Lorem ipsum dolor sit amet, consectetur adipisicing elit. A et, mollitia! Commodi eos fugiat laudantium nam
        vitae. Accusamus alias, aperiam, beatae et facilis molestiae obcaecati, odio quasi quo reprehenderit vitae!
    </p>

    <p>
        Lorem ipsum dolor sit amet, consectetur adipisicing elit. Ab amet animi autem consequatur error ex inventore
        itaque natus numquam possimus, quaerat qui quo quod sit tempora tempore, ullam voluptates? Harum!
    </p>
</div>
<div>
    <a id="the_link" href="#">I'm the link here!</a>
</div>
<div>
    <p>
        Lorem ipsum dolor sit amet, consectetur adipisicing elit. A et, mollitia! Commodi eos fugiat laudantium nam
        vitae. Accusamus alias, aperiam, beatae et facilis molestiae obcaecati, odio quasi quo reprehenderit vitae!
    </p>

    <p>
        Lorem ipsum dolor sit amet, consectetur adipisicing elit. Ab amet animi autem consequatur error ex inventore
        itaque natus numquam possimus, quaerat qui quo quod sit tempora tempore, ullam voluptates? Harum!
    </p>
</div>
<div class="hide_me">
    <div id="the_ugliest_popup_ever" class="zolipop_draggable_handle" >I'm the rawest <span class="zolipop_close">popup</span> ever</div>
</div>


<script>
    (function ($) {
        $(document).ready(function () {
            
            var jPopup = $("#the_ugliest_popup_ever");
            var oPop = new zolipop({
                jPopup: jPopup,
                plugins: [new zolipopDraggable()]
            });
            
            $("#the_link").on('click', function () {
                oPop.pop({
                    width: 300,
                    height: 250
                });
                return false;
            });
        });
    })(jQuery);
</script>
</body>
</html>
```




zolipop instance options
-----------------

```js
{
    //------------------------------------------------------------------------------/
    // REQUIRED
    //------------------------------------------------------------------------------/
    /**
     * The jquery handle to the element to pop up.
     */
    jPopup: null,
    //------------------------------------------------------------------------------/
    // OPTIONAL
    //------------------------------------------------------------------------------/
    /**
     * If true, a click on the overlay makes the popup disappear.
     */
    clickCloseOverlay: true,
    /**
     * The jquery selector for elements that close the popup.
     * The elements have to be inside the overlay in order for this to work.
     * If you want to make the popup close when you click on the overlay,
     * use the clickCloseOverlay option.
     */
    closeSelector: ".zolipop_close",
    /**
     * The background color of the overlay, or null to use as is.
     * Warning: if you use rgba, don't put a space between the "rgba" keyword
     * and the first opening bracket (it didn't work for me using jquery 2.1.4)
     */
    overlayColor: 'rgba(0, 0, 0, 0.5)',
    /**
     * An array of plugins.
     * A plugin can hook with the zolipop plugin using a few methods:
     * 
     * - init ( jPopup ) 
     * 
     */
    plugins: []
}
```


zolipop pop method options
----------------------------

The pop method opens the popup, with an overlay below it.
You can change the overlay color, the size and position of the popup.


```js
{
    //------------------------------------------------------------------------------/
    // OPTIONAL
    //------------------------------------------------------------------------------/
    /**
     * If null, the width is not set programmatically (and you can style it with css).
     * The expected value here is any value that jquery.css method will accept.
     */
    width: null,
    /**
     * If null, the height is not set programmatically (and you can style it with css).
     * The expected value here is any value that jquery.css method will accept.
     */
    height: null,
    /**
     * If null, the popup will be positioned in the middle of the screen.
     * In this case, the popup element will be absolute positioned (i.e. it will get the position: absolute attribute).
     *
     * Or you can provide a callback to position as you wish.
     *
     * Your callback should return an array of [width, height], in which case the values will be
     *              passed to the jquery.css property.
     *              Use null to skip one or both of the properties.
     *              If you use one of those properties, the popup element will be absolute positioned.
     *              
     *              Developer Note: it's easy to add marginLeft and marginTop argument behind the width and height...
     *
     *              You can also return null and handle the positioning yourself.
     *
     */
    position: null
}
```



Using a draggable plugin 
----------------------------

Zolipop focuses on displaying a popup the screen, it takes care of displaying 
the overlay on the screen, and putting the popup inside of it.

However, if you want to drag the popup, you need a plugin.
I coded a plugin called zolipopDraggable that does that, or you can create your own plugin 
if you want.


To use a plugin, add it to the plugins list upon the zolipop instantiation:
 
```js
// ...
var oPop = new zolipop({
    jPopup: jPopup,
    plugins: [new zolipopDraggable()]  // of course, you need to include the correponding plugin file in your html head
});
// ...
``` 


ZolipopDraggable plugin options
------------------------------------

```js 
{
    /**
     * Abstract:
     * To drag an element, you need an handle.
     * The dragged element and the handle might not always have a parent relationship.
     * When you start dragging the handle, the dragged element syncs with it until
     * you mouse up.
     *
     * Concrete:
     * In this case, the dragged element is always the jPopup element.
     * Also in this plugin in particular, the handle has to be INSIDE the jPopup element.
     *
     * The jquery selector to handle element(s).
     */
    handleSelector: '.zolipop_draggable_handle',
    /**
     * Using this callback, you might be able to implement a boundary system.
     * The callback takes the following arguments:
     *
     *      array:newPosition      callback ( int:mouseX, int:mouseY, int:jPopupX, int:jPopupY, jquery: jPopup )
     *
     *                                  The first four parameters all indicate a position relative to the document.
     *                                  The last parameter is the jquery handle of the dragged element.
     *
     *
     *                                  It returns an array containing the new left and
     *                                  top positions of the jPopup element.
     *                                  Those positions are any left and top values that the jquery.css method
     *                                  would accept.
     *
     *
     * There is a default callback that makes the jPopup element prisoner of the window.
     * To use it, use the string "default" (which is the default by the way).
     *
     *
     * The adjustPosition can take the following values:
     * - str: default, the default callback provided by me
     * - null: don't apply any adjustments
     * - callback: your own callback
     *
     */
    adjustPosition: 'default'
}
```





History Log
------------------
    
- 1.0.0 -- 2016-01-28

    - initial commit
    
    







