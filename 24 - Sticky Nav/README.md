# Sticky Nav

Here is an example of creating a sticky navigation bar once the user has scrolled down the page.

```js
const nav = document.querySelector("#main")
let topOfNav = nav.offsetTop

function fixNav() {
  if (window.scrollY >= topOfNav) {
    // Remember - when we make an element fixed, it is no longer taking up space in the document. It's effectively layered above and on top of the document.
    // It is for this reason that we want to add padding to the top of our HTML that is the offset height of our fixed Navigation element.
    document.body.style.paddingTop = nav.offsetHeight + "px"
    document.body.classList.add("fixed-nav")
  } else {
    // Remove our fixed class and reset the padding top of our document to be 0
    document.body.classList.remove("fixed-nav")
    document.body.style.paddingTop = 0
  }
}

window.addEventListener("scroll", fixNav)
```

Our `li.logo` has an overflow of `hidden` and width of `0`. We want our fixed nav to display this logo and make it visible programmatically.

```css
.fixed-nav li.logo {
  max-width: 500px;
}
```
