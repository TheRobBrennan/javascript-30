# CSS Text Shadow Mouse Move Effect

An example that has a shadow that follows you around similar to what we see in the [What the Flexbox?!](https://flexbox.io) example at [https://flexbox.io](https://flexbox.io).

NOTE: The animation depicted in the video from this lesson no longer exists or does not work on Safari.

```js
const hero = document.querySelector(".hero")
const text = hero.querySelector("h1")
const walk = 500 // 500px

function shadow(e) {
  const { offsetWidth: width, offsetHeight: height } = hero
  let { offsetX: x, offsetY: y } = e

  // What is this? It will always be the thing we are listening to - our .hero div in this example
  // We want to look at e.target and see if we are within an element other than our original div - such as an h1 tag.
  //  REMEMBER: Each element will have its top left starting at (0,0) - so we need to add this to our hero offset
  if (this !== e.target) {
    x = x + e.target.offsetLeft
    y = y + e.target.offsetTop
  }

  // The xWalk and yWalk is set to have a range of -50 to +50 based on our x and y values
  const xWalk = Math.round((x / width) * walk - walk / 2)
  const yWalk = Math.round((y / height) * walk - walk / 2)

  text.style.textShadow = `
      ${xWalk}px ${yWalk}px 0 rgba(255,0,255,0.7),
      ${xWalk * -1}px ${yWalk}px 0 rgba(0,255,255,0.7),
      ${yWalk}px ${xWalk * -1}px 0 rgba(0,255,0,0.7),
      ${yWalk * -1}px ${xWalk}px 0 rgba(0,0,255,0.7)
    `
}

hero.addEventListener("mousemove", shadow)
```
