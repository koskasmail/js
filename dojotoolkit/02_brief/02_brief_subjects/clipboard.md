
<a name="topage"></a>
 
# clipboard

#### unsorted file/subject... (temp placed here)

* create a new files by subject with order.

----

* Modules: 

```
Think of a module as a building block of functionality.
It’s a self-contained piece of code that performs a specific
task or holds related functions and data.
For example,
an authentication module might handle login
and password recovery separately from other app features.
```

* Module – A basic greeting module (greetingModule.js)

```javascript
define([], function() {
  return {
    getGreeting: function(name) {
      return "Hello, " + name + "!";
    }
  };
});
```

* Widgets: 

```
Widgets are the visual components you see on a screen—buttons,
sliders, text boxes, or entire user interface elements.
In UI frameworks like Flutter, a widget could be as simple as a text label
or as complex as a complete layout of an app page.
```

----

* Widget – A custom widget using Dijit (GreetingWidget.js)

```javascript
define([
  "dojo/_base/declare", 
  "dijit/_WidgetBase", 
  "dijit/_TemplatedMixin", 
  "dojo/text!./templates/GreetingWidget.html",
  "./greetingModule"
], function(declare, _WidgetBase, _TemplatedMixin, template, greetingModule) {
  
  return declare([_WidgetBase, _TemplatedMixin], {
    templateString: template,
    name: "World",
    
    postCreate: function() {
      this.inherited(arguments);
      this.greetingNode.innerHTML = greetingModule.getGreeting(this.name);
    }
  });
});
```

Template file (templates/GreetingWidget.html)

```html
<div>
  <h2 data-dojo-attach-point="greetingNode"></h2>
</div>
```

----

* Layout: 

```
This defines how widgets (or other content) are arranged visually on the screen.
It controls positioning, spacing, alignment, and responsiveness across different screen sizes.
```

* Layout – Placing the widget in a layout container (e.g., BorderContainer)
```html
<div data-dojo-type="dijit/layout/BorderContainer" 
     data-dojo-props="design:'headline',gutters:true" 
     style="width: 100%; height: 100%;">
  
  <div data-dojo-type="dijit/layout/ContentPane" 
       data-dojo-props="region:'center'">
    <div data-dojo-type="app/GreetingWidget" 
         data-dojo-props="name:'Dojo Developer'"></div>
  </div>
</div>
```


----

<p align="right">(<a href="#topage">back to top</a>)</p>
<br/>
<br/>
