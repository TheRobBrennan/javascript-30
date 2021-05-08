# AJAX Type Ahead

This tutorial looks at an example autocomplete field which loads data from an external endpoint. When the user types, this list is filtered to identify any matching cities or states.

## Additional notes

If you're looking to push data returned from fetch into an array declared as a `const` you can spread the resulting data array as follows:

```js
const cities = []
fetch(endpoint)
  .then((blob) => blob.json())
  .then((data) => cities.push(...data))
```

In `findMatches`, how would you create a regular expression based on the variable passed in?

This example is a `g`lobal search that is case `i`nsensitive when matching.

```js
function findMatches(wordToMatch, cities) {
  return cities.filter((place) => {
    // here we need to figure out if the city or state matches what was searched
    const regex = new RegExp(wordToMatch, "gi")
    return place.city.match(regex) || place.state.match(regex)
  })
}
```

Let's update our code to filter on every keypress (`keyup`) and when the user clicks outside the input (`change`).

```js
const searchInput = document.querySelector(".search")
const suggestions = document.querySelector(".suggestions")

searchInput.addEventListener("change", displayMatches)
searchInput.addEventListener("keyup", displayMatches)
```

Generate the HTML to be returned after we find matching content and build the list items:

```js
function displayMatches() {
  const matchArray = findMatches(this.value, cities)
  const html = matchArray
    .map((place) => {
      const regex = new RegExp(this.value, "gi")
      const cityName = place.city.replace(
        regex,
        `<span class="hl">${this.value}</span>`
      )
      const stateName = place.state.replace(
        regex,
        `<span class="hl">${this.value}</span>`
      )
      return `
      <li>
        <span class="name">${cityName}, ${stateName}</span>
        <span class="population">${numberWithCommas(place.population)}</span>
      </li>
    `
    })
    .join("")
  suggestions.innerHTML = html
}
```

We can highlight the matching text with a highlight class `hl`:

```css
.hl {
  background: #ffc600;
}
```
