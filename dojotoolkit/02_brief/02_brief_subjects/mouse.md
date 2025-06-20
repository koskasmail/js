<a name="topage"></a>

# mouse

### Common Mouse Events

1. **mousedown**: Triggered when a mouse button is pressed down.
2. **mouseup**: Triggered when a mouse button is released.
3. **click**: Triggered when a mouse button is pressed and released on the same element.
4. **dblclick**: Triggered when an element is double-clicked.
5. **mousemove**: Triggered when the mouse pointer moves over an element.
6. **mouseenter**: Triggered when the mouse pointer enters an element (does not bubble).
7. **mouseleave**: Triggered when the mouse pointer leaves an element (does not bubble).
8. **wheel**: Triggered when the mouse wheel is scrolled.

### Mouse Event Properties

- **clientX**: The X coordinate of the mouse pointer relative to the viewport.
- **clientY**: The Y coordinate of the mouse pointer relative to the viewport.
- **screenX**: The X coordinate of the mouse pointer relative to the screen.
- **screenY**: The Y coordinate of the mouse pointer relative to the screen.
- **button**: Indicates which mouse button was pressed (0 for left, 1 for middle, 2 for right).
- **buttons**: A bitmask representing which buttons are currently pressed.

### Example Usage in JavaScript

Here's a simple example of how to handle mouse events in JavaScript:

```javascript
document.addEventListener('mousedown', function(event) {
    if (event.button === 0) { // Left mouse button
        console.log('Left mouse button pressed at:', event.clientX, event.clientY);
    }
});

document.addEventListener('click', function(event) {
    console.log('Clicked at:', event.clientX, event.clientY);
});
```
