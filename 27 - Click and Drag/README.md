# Click and drag to scroll

A foldout accordion demonstrating clicking and dragging to scroll.

```js
const slider = document.querySelector(".items")
let isDown = false
let startX
let scrollLeft

slider.addEventListener("mousedown", (e) => {
  isDown = true
  slider.classList.add("active")
  startX = e.pageX - slider.offsetLeft
  scrollLeft = slider.scrollLeft
})

slider.addEventListener("mouseleave", () => {
  isDown = false
  slider.classList.remove("active")
})

slider.addEventListener("mouseup", () => {
  isDown = false
  slider.classList.remove("active")
})

slider.addEventListener("mousemove", (e) => {
  if (!isDown) return // stop the fn from running
  e.preventDefault()
  const x = e.pageX - slider.offsetLeft
  const walk = (x - startX) * 3
  slider.scrollLeft = scrollLeft - walk
})
```
