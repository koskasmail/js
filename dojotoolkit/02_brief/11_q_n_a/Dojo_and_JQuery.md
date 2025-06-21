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

----

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

##### [ Thoughts ]
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

##### [ Thoughts ]
- The **jQuery button** uses `$().click()` to update its own result text.
- The **Dojo button** uses `dojo/on` to handle its click event separately.
- They coexist peacefully in the same page!

----

#### Example 3: `custom Dojo widget` that interacts with `jQuery` elements on the same page.

* create a Dojo widget, and when the widget is clicked, it will update content `inside a jQuery-managed element`.


##### File Structure
```
/project
│
├── app/
│   └── GreetingWidget.js
│
└── index.html
```


### Dojo Widget – `GreetingWidget.js`
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

### 2. HTML – `index.html`

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

##### [ Thoughts ]

* The **Dojo widget** initializes normally using `data-dojo-type`.
* On click, it `talks to jQuery` using `jQuery(...)` directly from inside the widget.
* This shows how both libraries can communicate smoothly in the same interface.

----

#### Example 4: `jQuery` will trigger an update inside a `Dojo widget`

* We’ll let a `jQuery` button click change the content of a `Dojo widget's` internal DOM.
* For that, we’ll expose the widget's `inner node` and give it an `ID` (a hook).

---

### `GreetingWidget.js`

* Update the widget so it exposes a named DOM node we can update later:

```javascript
define([
  "dojo/_base/declare",
  "dijit/_WidgetBase",
  "dojo/dom-construct"
], function(declare, _WidgetBase, domConstruct) {

  return declare([_WidgetBase], {
    postCreate: function() {
      this.inherited(arguments);

      // Add content and ID so jQuery can hook into it
      this.domNode.innerHTML = "<span id='greetingText'>👋 Hello from Dojo Widget!</span>";
    }
  });
});
```

### 2. HTML - `index.html`

* We’ll place a jQuery button and use it to update the Dojo widget:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>jQuery Updates Dojo Widget</title>

  <!-- jQuery -->
  <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>

  <!-- Dojo -->
  <script src="https://ajax.googleapis.com/ajax/libs/dojo/1.16.4/dojo/dojo.js"
          data-dojo-config="async: true, parseOnLoad: true"></script>

  <!-- Widget activation -->
  <script>
    require(["dojo/parser", "app/GreetingWidget", "dojo/domReady!"], function(parser) {
      parser.parse();
    });

    // jQuery interaction (wait until everything is ready)
    $(document).ready(function() {
      $("#triggerChange").click(function() {
        $("#greetingText").text("👨‍💻 jQuery updated the widget!");
      });
    });
  </script>
</head>
<body>
  <h2>jQuery Controls Dojo</h2>

  <button id="triggerChange">Update Widget (via jQuery)</button>

  <div data-dojo-type="app/GreetingWidget"></div>
</body>
</html>
```

##### [ Thoughts ]

* Why This Works:
   * We gave the widget's internal content a unique ID (`greetingText`) so jQuery could find and update it.
   * On button `click`, jQuery uses `$("#greetingText")` to modify the DOM inside the Dojo widget.
   * This approach maintains their boundaries, but lets them collaborate smoothly.

----

#### Example 5: a dynamic interaction: 

* a `Dojo form widget` and `jQuery` -powered validation plus animation.
* These two libraries can complement each other beautifully when orchestrated right.
* You fill out a Dojo `TextBox` widget, and when you submit, `jQuery` checks the input and gives animated feedback.


### Widget + Validation Example

#### HTML - `index.html`

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Dojo Form + jQuery Validation</title>

  <!-- jQuery -->
  <script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>

  <!-- Dojo -->
  <link rel="stylesheet" href="https://ajax.googleapis.com/ajax/libs/dojo/1.16.4/dijit/themes/claro/claro.css">
  <script src="https://ajax.googleapis.com/ajax/libs/dojo/1.16.4/dojo/dojo.js"
          data-dojo-config="async: true, parseOnLoad: true"></script>

  <style>
    body { font-family: sans-serif; margin: 30px; }
    #feedback {
      display: none;
      margin-top: 10px;
      padding: 8px;
      border-radius: 4px;
    }
    #feedback.success { background-color: #daf5d7; color: #255625; }
    #feedback.error { background-color: #f8d7da; color: #842029; }
  </style>

  <script>
    require([
      "dojo/parser", 
      "dijit/form/TextBox", 
      "dojo/domReady!"
    ], function(parser) {
      parser.parse();

      // jQuery validation
      $("#submitBtn").click(function() {
        const name = $("#userInput").val().trim();

        if (name === "") {
          $("#feedback")
            .removeClass("success")
            .addClass("error")
            .text("Please enter your name.")
            .fadeIn()
            .delay(2000)
            .fadeOut();
        } else {
          $("#feedback")
            .removeClass("error")
            .addClass("success")
            .text(`Welcome, ${name}! 🎉`)
            .fadeIn()
            .delay(2000)
            .fadeOut();
        }
      });
    });
  </script>
</head>
<body class="claro">
  <h2>Dojo TextBox with jQuery Validation</h2>

  <label for="userInput">Your Name:</label>
  <input id="userInput" data-dojo-type="dijit/form/TextBox">

  <button id="submitBtn">Submit</button>

  <div id="feedback"></div>
</body>
</html>
```

---


##### [ Thoughts ]

* `Dojo` gives us a styled `TextBox` widget.
* `jQuery` handles validation, feedback, and smooth animations with `fadeIn`, `delay`, and `fadeOut`.
* Works great for hybrid apps where you want both the elegance of `Dojo widgets` and the ease of `jQuery` effects.



----

<p align="right">(<a href="#topage">back to top</a>)</p>
<br/>
<br/>
