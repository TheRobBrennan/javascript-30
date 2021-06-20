# Stripe Follow Along Dropdown

This example will demonstrate displaying differently sized and changing `div` elements when mousing over navigation elements.

```js
const triggers = document.querySelectorAll(".cool > li")
const background = document.querySelector(".dropdownBackground")
const nav = document.querySelector(".top")

function handleEnter() {
  this.classList.add("trigger-enter")

  // IMPORTANT: We use an arrow function here so that we are referencing the same "this" as our previous line
  //  If we used function instead, "this" changes and is NOT the same as "this.classList" in our previous line
  setTimeout(
    () =>
      this.classList.contains("trigger-enter") &&
      this.classList.add("trigger-enter-active"),
    150
  )
  background.classList.add("open")

  const dropdown = this.querySelector(".dropdown")
  const dropdownCoords = dropdown.getBoundingClientRect()
  const navCoords = nav.getBoundingClientRect()

  const coords = {
    height: dropdownCoords.height,
    width: dropdownCoords.width,
    // Don't assume that navigation is the top-most element in the document. We need to make sure to calculate our offset accordingly.
    top: dropdownCoords.top - navCoords.top,
    left: dropdownCoords.left - navCoords.left,
  }

  background.style.setProperty("width", `${coords.width}px`)
  background.style.setProperty("height", `${coords.height}px`)
  background.style.setProperty(
    "transform",
    `translate(${coords.left}px, ${coords.top}px)`
  )
}

function handleLeave() {
  this.classList.remove("trigger-enter", "trigger-enter-active")
  background.classList.remove("open")
}

triggers.forEach((trigger) =>
  trigger.addEventListener("mouseenter", handleEnter)
)
triggers.forEach((trigger) =>
  trigger.addEventListener("mouseleave", handleLeave)
)
```
