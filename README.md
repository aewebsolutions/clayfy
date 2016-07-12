# Clayfy Jquery Plugin

Clayfy is a full featured JQuery plugin that makes elements draggable, resizable or sortable.

Most of the usual features are included in this plugin, so you don't need to write them again.


## Demos

- Draggable: https://rawgit.com/aewebsolutions/clayfy/master/demos/draggable.html
- Resizable: https://rawgit.com/aewebsolutions/clayfy/master/demos/resizable.html
- Sortable: https://rawgit.com/aewebsolutions/clayfy/master/demos/sortable.html

## Features overview

- No markup changes over element.
- Constrict movement (x/y)
- Container area
- Scroll containers over bounderies.
- Ghost image
- Droppable area
- Migration (reappend element to a new parent on dropping)
- Resizing constricts (min/max)
- Resizing handlers from outside: you can use
- CSS customization resizable handlers
- Sort elements from a same parent or export to a new one
- Events

## Installation

Include JQuery js and plugin’s css and js files in your HTML code.

```html

<script src="/lib/jquery.js"></script> 
<link rel="stylesheet" href="/dist/clayfy.min.css" type="text/css" /> 
<script src="/dist/clayfy.min.js"></script> 

```

## Usage

Next there is basic usage for Clayfy. Further information you can find inside demos and options and events tables below.

### Draggable

When document is ready, you can call the plugin for an **absolute positioned** element.

```javascript

$(document).ready(function(){

    $('#draggable').clayfy({
        type : 'draggable'
    });

});
```

As 'draggable' is Clayfy's `type` default value, you can make that cleaner:
```javascript
    $('#draggable').clayfy();
```

You can assign a container to your draggable element. Your element doesn't need to be a container's child.

```html
<div id="container"><div>
<div id="draggable"></div>
```

```javascript
$('#draggable').clayfy({
    container : #container
});
```

Also, you can assign a droppable element. Further, you can set up draggable and droppable behavior through plugin's options, events and CSS.

Markup:
```html
<div id="droppable-1"><div>
<div id="droppable-2"><div>
<div id="draggable"></div>
```

Javascript:
```javascript
$('#draggable').clayfy({
    droppable : '#droppable-1, #droppable-2'
})
$('#draggable').on({
    'clayfy-dragenter' : function(e){
        console.log(e.target)
    },
});
```

SCSS:
```scss
#draggable{
    &.clayfy-dragenter{
        background : #38c429;
    }
    &.clayfy-dropinside{
        background : #5a6de3;
    }
}
#droppable-1{
    &.clayfy-dragenter{
        border-color : #38c429;
    }
    &.clayfy-dropinside{
        border-color: #5a6de3;
    }
}
```


### Resizable

Resizable elements inherits draggable options. But element does not need to be absolute positioned, although is required if you like to drag it.

Just call:
```javascript
$('#resizable').clayfy({
    type : 'resizable'
});
```

Options: `minSize`, `maxSize` and `preserveAspectRatio` you will surely use. There are further options too that you may need (see table below).

There are a list of events that you may need. For example,

```javascript
$resizable = $('#resizable');
$resizable.on('clayfy-resize clayfy-cancel', function(){
    console.log( $resizable.width() + ' x '+ $resizable.height() )
});
```

Last, for an easy handler styling, use `className` option:

```javascript
$('#resizable').clayfy({
    type : 'resizable',
    className : 'myCustomClass'
});
```

```scss
.myCustomClass{

    &.clayfy-top:after{
        content: '';
        position: absolute;
        height: 2px;
        top:0;left:0;
        width:100%;
        background: red;
    }

}
```

### Sortable
Sortable elements element may be absolute positioned, although is not recommended. 

By default, each sortable element can change place with a sibling that has the same CSS selector and has the same parent:

```html
<div>
    <div class="sortable">A.1</div>
    <div class="sortable">A.2</div>
    <div class="sortable">A.3</div>
</div>
<div>
    <div class="sortable">B.1</div>
    <div class="sortable">B.2</div>
    <div class="sortable">B.3</div>
</div>
```

```javascript
<script>
    $('.sortable').clayfy({
        type : 'sortable'
    });
</script>
```
Of course, there are severals options. For instance, you can change order between elements from diferent parents (`siblings` option), move node from one to another (`export` option),
constrict movement (see draggable options), etc.

Further examples you will find inside demos.

## Options
Options can be set locally as a paramater or globally through $.clayfy.settings object.

Also, it is worth to say that most of draggable options can be use in resizable and sortable types (but, of course, not vice versa).

### Draggable

Name | Type | Default | Description
--- | --- | --- | ---
**container** | mix | void |  Container that constricts movement. Can be a CSS Selector, an array (top, right, bottom, left) or an instance of DraggableContainer (See $.clayfy.container(...))
**moveY** | boolean | true | Element can be moved vertically
**moveX** | boolean | true | Element can be moved horizontally
**move** | boolean | true | Element can be moved
**not** | string | void | CSS Selector of childs to exclude. When user click on them, draggable element cannot be moved.
**ghost** | mix | false | Element is not dragged itself but an image ghost. Set true or fill with markup. Also, a function that returns markup
**coverScreen** | boolean | true | Cover screen with layer for easily drag all over the screen (also, over iframes)
**scrollable** | mix | void | CSS Selector or JQuery instance. By default, conainer is scrollable. Turn false to not scroll
**droppable** | string | void | CSS Selector or JQuery instance where is allowed to drop
**fit** | boolean | true | Fit inside drop area when drop inside.
**dropoutside** | boolean | false | Allow to drop outside droppable area
**migrate** | boolean | false | Change parent after drop inside drop area
**overflow** | boolean | false | Element will be temporaly append to an helper container outside parent
**escape** | boolean | true | Cancel dragging with esc key

### Resizable

Name | Type | Default | Description
--- | --- | --- | ---
**preserveAspectRatio** | boolean | false | Preserve Aspect Ratio when element is being resized.
**maxSize** | array | [500, 200] | Max size of element: width, height
**minSize** | array | [100, 50] | Min size of element: width, height
**left** | boolean | true | whether the element can be resize by left side
**top** | boolean | true | whether the element can be resize by top side
**right** | boolean | true | whether the element can be resize by right side
**bottom** | boolean | true | whether the element can be resize by bottom side

### Sortable

Name | Type | Default | Description
--- | --- | --- | ---
**siblings** | string | void | CSS selector. Force elements to be considered siblings. By default, only actual siblings.
**export** | boolean | true | Whether an element can be exported to another parent


## Events
Use event paramenter to have access to useful properties, as `target` or `droparea`.

### Draggable
Event | Params | Description
--- | --- | ---
**clayfy-drag** | event | When element is being dragged.
**clayfy-dragstart** | event | When element starts to be dragged.
**clayfy-dragenter** | event | When draggable enters droppable.
**clayfy-dragleave** | event | When draggable leaves droppable.
**clayfy-dropinside** | event | When draggable is dropped inside droppable.
**clayfy-dropoutside** | event | When draggable is dropped outside droppable.
**clayfy-drop** | event | When draggable is dropped.

### Draggable
Event | Params | Description
--- | --- | ---
**clayfy-resize** | event | When element is being resized
**clayfy-resizeend** | event | When resizing ends
**clayfy-resizestart** | event | When resizing starts
**clayfy-cancel** | event | When resizing is cancelled

### Sortable
Event | Params | Description
--- | --- | ---
**clayfy-changeorder** | event | When element has been changed place.


## License

Clayfy has been developed by [Ángel Espro](http://www.aesolucionesweb.com.ar/). It is licensed under the [MIT license](http://opensource.org/licenses/MIT)

