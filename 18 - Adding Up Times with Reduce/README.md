# Tally string times with reduce

Use the `data-time` attribute in our `li` elements with the `reduce` function to tally the total time.

```js
// We can convert this NodeList into an array by spreading this into a new array OR by using Array.from
const timeNodes = Array.from(document.querySelectorAll("[data-time]"))

const seconds = timeNodes
  .map((node) => node.dataset.time)
  .map((timeCode) => {
    // Use parseFloat so that we have an array consisting of numbers and not strings
    const [mins, secs] = timeCode.split(":").map(parseFloat)
    return mins * 60 + secs
  })
  .reduce((total, vidSeconds) => total + vidSeconds)

let secondsLeft = seconds
const hours = Math.floor(secondsLeft / 3600)
// After we chunk our seconds into hours, use modulo to determine how many seconds remain
secondsLeft = secondsLeft % 3600

const mins = Math.floor(secondsLeft / 60)
secondsLeft = secondsLeft % 60

console.log(hours, mins, secondsLeft)
```
