# Custom HTML5 Video Player

A simple custom video player that contains:

- A `player` div with
  - A `<video>` element
  - A `div` containing player controls that can hide/appear as needed

Check out `scripts-FINISHED.js` to see the JavaScript created.

Note how we are updating the `flex-basis` of `.progress__filled` in the progress bar:

```js
function handleProgress() {
  const percent = (video.currentTime / video.duration) * 100
  progressBar.style.flexBasis = `${percent}%`
}
```

How do we automatically handle progress bar animation? Listen for the `timeupdate` event from the video element and invoke our `handleProgress` function.

What about handling video scrub functionality? We will listen for a `click` event on our progress bar and invoke our `scrub` function.

```js
function scrub(e) {
  const scrubTime = (e.offsetX / progress.offsetWidth) * video.duration
  video.currentTime = scrubTime
}
```

Note that the above scrubbing functionality works for a click event, but what about when people move the mouse? Listen for the `mousemove` event.

Let's improve this so that when someone moves their mouse, we want to run the scrub function:

```js
let mousedown = false
progress.addEventListener("click", scrub)
progress.addEventListener("mousemove", (e) => mousedown && scrub(e))
progress.addEventListener("mousedown", () => (mousedown = true))
progress.addEventListener("mouseup", () => (mousedown = false))
```

Suggested challenge - Implement full-screen button and functionality.
