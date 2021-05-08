# Fun with HTML5 Canvas

This tutorial explores drawing randomly colored squiggles from when the user presses the mouse. Drawing stops when they release the mouse button or drag the mouse cursor outside of the canvas.

I loved using the website [Mother Effing HSL](https://mothereffinghsl.com), an easy way to play with hue, saturation, and light online.

The `draw` function is the most critical piece of this example:

```js
function draw(e) {
  if (!isDrawing) return // stop the fn from running when they are not moused down
  console.log(e)
  ctx.strokeStyle = `hsl(${hue}, 100%, 50%)`
  ctx.beginPath()
  // start from
  ctx.moveTo(lastX, lastY)
  // go to
  ctx.lineTo(e.offsetX, e.offsetY)
  ctx.stroke()
  ;[lastX, lastY] = [e.offsetX, e.offsetY]

  hue++
  if (hue >= 360) {
    hue = 0
  }
  if (ctx.lineWidth >= 100 || ctx.lineWidth <= 1) {
    direction = !direction
  }

  if (direction) {
    ctx.lineWidth++
  } else {
    ctx.lineWidth--
  }
}
```

BONUS: Use array destructuring to declare our `lastX` and `lastY` values in one shot:

```js
;[lastX, lastY] = [e.offsetX, e.offsetY]
```
