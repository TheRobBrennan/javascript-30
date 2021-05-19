# Sorting band names without articles

Use JavaScript's Array `.sort()` method to sort an array of band names.

```js
const bands = [
  "The Plot in You",
  "The Devil Wears Prada",
  "Pierce the Veil",
  "Norma Jean",
  "The Bled",
  "Say Anything",
  "The Midway State",
  "We Came as Romans",
  "Counterparts",
  "Oh, Sleeper",
  "A Skylit Drive",
  "Anywhere But Here",
  "An Old Dog",
]

function strip(bandName) {
  // When our band name starts with "a", "the", or "an", replace it with an empty string. Trim any trailing spaces.
  return bandName.replace(/^(a |the |an )/i, "").trim()
}

const sortedBands = bands.sort((a, b) => (strip(a) > strip(b) ? 1 : -1))

document.querySelector("#bands").innerHTML = sortedBands
  .map((band) => `<li>${band}</li>`)
  .join("")

console.log(sortedBands)
```
