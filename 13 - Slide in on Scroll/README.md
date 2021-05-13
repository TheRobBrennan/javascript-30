# Slide in on scroll

An example of having images slide in from the left and the right while scrolling down a page.

This may not be a preferred effect - an aesthetic I don't personally like - but it is worth knowing how to do.

`transform` and `opacity` are leveraged to give us the effect of an image slid off to the left or right side with an `opacity` of `0.`

We have a custom `debounce` function because this is a framework-free tutorial.

```js
// Restricts excessive calls so that the function executes at most every 20ms
function debounce(func, wait = 20, immediate = true) {
  var timeout
  return function () {
    var context = this,
      args = arguments
    var later = function () {
      timeout = null
      if (!immediate) func.apply(context, args)
    }
    var callNow = immediate && !timeout
    clearTimeout(timeout)
    timeout = setTimeout(later, wait)
    if (callNow) func.apply(context, args)
  }
}
```

For this example, we want the slide in/out effect to work when scrolling from top to bottom and bottom to top.

```js
const sliderImages = document.querySelectorAll(".slide-in")

function checkSlide() {
  sliderImages.forEach((sliderImage) => {
    // half way through the image
    const slideInAt =
      window.scrollY + window.innerHeight - sliderImage.height / 2
    // bottom of the image
    const imageBottom = sliderImage.offsetTop + sliderImage.height
    const isHalfShown = slideInAt > sliderImage.offsetTop
    const isNotScrolledPast = window.scrollY < imageBottom
    if (isHalfShown && isNotScrolledPast) {
      sliderImage.classList.add("active")
    } else {
      sliderImage.classList.remove("active")
    }
  })
}

window.addEventListener("scroll", debounce(checkSlide))
```
