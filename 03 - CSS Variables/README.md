# Playing with CSS variables and JS

This example uses sliders in the UI to control CSS variables that dynamically control:

- Spacing
- Blur
- Base color

There were some interesting points for me to consider - which I have included below for reference.

```css
/* This is where we define our CSS variables */
:root {
  --base: #ffc600;
  --spacing: 10px;
  --blur: 10px;
}
```

```js
// This will give us a NodeList and NOT an array
const inputs = document.querySelectorAll(".controls input")

function handleUpdate() {
  // What is dataset? An object containing all of the attributes for that element
  const suffix = this.dataset.sizing || ""
  document.documentElement.style.setProperty(
    `--${this.name}`,
    this.value + suffix
  )
}

// We want to have more life than just updating when the change is complete. Let's add event handlers for moving and dragging the mouse
inputs.forEach((input) => input.addEventListener("change", handleUpdate))
inputs.forEach((input) => input.addEventListener("mousemove", handleUpdate))
```
