<a name="topage"></a>
 
# Dojo_and_JQuery


----

#### Q1: can `dojo Toolkit` contain `JQuery` library ?

* Yes, you can use the Dojo Toolkit alongside jQuery in the same project, but with a few caveats.
* `Dojo` and `jQuery` are both powerful JavaScript libraries,
* but they have overlapping features like
   * DOM manipulation,
   * AJAX handling
   * effects
   * if you're using both, it's important to clearly separate their roles to avoid unnecessary duplication or conflicts.

* Here’s how you might do it:
   * Use `jQuery` for quick DOM manipulations or plugin support if you need a specific jQuery plugin.
   * Use `Dojo` for its robust widget system (Dijit), modular architecture, and layout tools.

* Just be mindful:
   * Use require or define from Dojo’s AMD loader to structure your modules.
   * Avoid mixing the two in the same logic unless necessary—it can lead to maintenance headaches.

* Example: load both **Dojo Toolkit** and **jQuery** on the same page and use them without stepping on each other's toes.


#### Example 1: Using Dojo and jQuery Together

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Dojo + jQuery Example</title>

  <!-- Load jQuery first -->
  <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>

  <!-- Load Dojo Toolkit -->
  <script src="https://ajax.googleapis.com/ajax/libs/dojo/1.16.4/dojo/dojo.js"></script>

  <script>
    // jQuery part
    $(document).ready(function() {
      $("#jqueryDiv").text("This was changed by jQuery!");
    });

    // Dojo part
    require(["dojo/dom", "dojo/domReady!"], function(dom) {
      var dojoDiv = dom.byId("dojoDiv");
      dojoDiv.innerHTML = "This was changed by Dojo!";
    });
  </script>
</head>
<body>
  <h2>Mixed Dojo and jQuery Demo</h2>
  <div id="jqueryDiv">Original jQuery Text</div>
  <div id="dojoDiv">Original Dojo Text</div>
</body>
</html>
```

##### Key Points:
- jQuery runs on `$(document).ready(...)`, while Dojo uses AMD with `require()` and `domReady!`.
- Each library updates its own section, avoiding overlap.
- If needed, you can also **scope jQuery with `noConflict()`** to avoid any global symbol conflicts with `$`.

----

#### Example 2: Click Buttons with Dojo and jQuery

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Dojo + jQuery Interaction</title>

  <!-- jQuery -->
  <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>

  <!-- Dojo Toolkit -->
  <script src="https://ajax.googleapis.com/ajax/libs/dojo/1.16.4/dojo/dojo.js"></script>

  <script>
    // jQuery section
    $(document).ready(function() {
      $("#jqueryBtn").click(function() {
        $("#jqueryResult").text("Handled by jQuery!");
      });
    });

    // Dojo section
    require(["dojo/dom", "dojo/on", "dojo/domReady!"], function(dom, on) {
      var btn = dom.byId("dojoBtn");
      var output = dom.byId("dojoResult");

      on(btn, "click", function() {
        output.innerHTML = "Handled by Dojo!";
      });
    });
  </script>
</head>
<body>
  <h2>Hybrid Dojo + jQuery UI</h2>

  <div style="margin-bottom: 20px;">
    <button id="jqueryBtn">Click Me (jQuery)</button>
    <div id="jqueryResult">jQuery Output</div>
  </div>

  <div>
    <button id="dojoBtn">Click Me (Dojo)</button>
    <div id="dojoResult">Dojo Output</div>
  </div>
</body>
</html>
```

##### Key Points:
- The **jQuery button** uses `$().click()` to update its own result text.
- The **Dojo button** uses `dojo/on` to handle its click event separately.
- They coexist peacefully in the same page!

----

#### Example 3: `custom Dojo widget` that interacts with `jQuery` elements on the same page.

---

* create a Dojo widget, and when the widget is clicked, it will update content `inside a jQuery-managed element`.

---

##### File Structure
```
/project
│
├── app/
│   └── GreetingWidget.js
│
└── index.html
```

---

### 1. **Dojo Widget – GreetingWidget.js**
```javascript
define([
  "dojo/_base/declare",
  "dijit/_WidgetBase",
  "dojo/dom-construct"
], function(declare, _WidgetBase, domConstruct) {

  return declare([_WidgetBase], {
    postCreate: function() {
      this.inherited(arguments);

      this.domNode.innerHTML = "👋 Hello from Dojo Widget!";
      this.domNode.style.cursor = "pointer";

      this.connect(this.domNode, "click", function() {
        // Update jQuery-managed element
        if (window.jQuery) {
          jQuery("#jqueryTarget").text("Updated by Dojo widget!");
        }
      });
    }
  });
});
```

---

### 2. **HTML – index.html**

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Dojo Widget Meets jQuery</title>

  <!-- jQuery -->
  <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>

  <!-- Dojo -->
  <script src="https://ajax.googleapis.com/ajax/libs/dojo/1.16.4/dojo/dojo.js"
          data-dojo-config="async: true, parseOnLoad: true"></script>

  <!-- Register custom widget -->
  <script>
    require(["dojo/parser", "app/GreetingWidget", "dojo/domReady!"], function(parser) {
      parser.parse();
    });
  </script>
</head>
<body>
  <h2>Dojo Widget + jQuery DOM Update</h2>

  <!-- jQuery-managed area -->
  <div id="jqueryTarget">Waiting for widget...</div>

  <!-- Custom Dojo widget -->
  <div data-dojo-type="app/GreetingWidget"></div>
</body>
</html>
```

### [ Thoughts ]

* The **Dojo widget** initializes normally using `data-dojo-type`.
* On click, it `talks to jQuery` using `jQuery(...)` directly from inside the widget.
* This shows how both libraries can communicate smoothly in the same interface.


----


<p align="right">(<a href="#topage">back to top</a>)</p>
<br/>
<br/>
