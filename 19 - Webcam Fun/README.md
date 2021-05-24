# Unreal webcam fun

An example photo booth created with JavaScript.

This is the first example that needs a locally running server. This is because using a webcam requires a secure origin - an `HTTPS` site or `localhost` will work. Any server will work; however, this example has a `package.json` file that will allow you to install `browser-sync` with `npm install` and then fire off the locally running server with `npm start`

```js
const video = document.querySelector(".player")
const canvas = document.querySelector(".photo")
const ctx = canvas.getContext("2d")
const strip = document.querySelector(".strip")
const snap = document.querySelector(".snap")

function getVideo() {
  navigator.mediaDevices
    .getUserMedia({ video: true, audio: false })
    .then((localMediaStream) => {
      console.log(localMediaStream)

      //  DEPRECIATION :
      //       The following has been depreceated by major browsers as of Chrome and Firefox.
      //       video.src = window.URL.createObjectURL(localMediaStream);
      //       Please refer to these:
      //       Deprecated  - https://developer.mozilla.org/en-US/docs/Web/API/URL/createObjectURL
      //       Newer Syntax - https://developer.mozilla.org/en-US/docs/Web/API/HTMLMediaElement/srcObject

      video.srcObject = localMediaStream
      video.play()
    })
    .catch((err) => {
      console.error(`OH NO!!!`, err)
    })
}

function paintToCanvas() {
  const width = video.videoWidth
  const height = video.videoHeight

  // Make sure the canvas width and height matches incoming video
  canvas.width = width
  canvas.height = height

  return setInterval(() => {
    // Start at the top left hand corner of the canvas and then paint the width and height
    ctx.drawImage(video, 0, 0, width, height)
    // take the pixels out
    let pixels = ctx.getImageData(0, 0, width, height)
    // mess with them
    // pixels = redEffect(pixels);

    pixels = rgbSplit(pixels)
    // Set this to 0.1 and you will see a ghosting effect 👻
    // ctx.globalAlpha = 0.8;

    // pixels = greenScreen(pixels);
    // put them back
    ctx.putImageData(pixels, 0, 0)
  }, 16)
}

function takePhoto() {
  // played the sound
  snap.currentTime = 0
  snap.play()

  // take the data out of the canvas
  const data = canvas.toDataURL("image/jpeg")
  const link = document.createElement("a")
  link.href = data
  link.setAttribute("download", "handsome")
  link.innerHTML = `<img src="${data}" alt="Handsome Man" />`

  // Pre-pend the photo
  strip.insertBefore(link, strip.firstChild)
}

function redEffect(pixels) {
  // Note that each pixel has a value from 0-255 in index 0, 1, 2, 3 - mapping to red, green, blue, and alpha values
  for (let i = 0; i < pixels.data.length; i += 4) {
    pixels.data[i + 0] = pixels.data[i + 0] + 200 // RED
    pixels.data[i + 1] = pixels.data[i + 1] - 50 // GREEN
    pixels.data[i + 2] = pixels.data[i + 2] * 0.5 // Blue
  }
  return pixels
}

function rgbSplit(pixels) {
  for (let i = 0; i < pixels.data.length; i += 4) {
    pixels.data[i - 150] = pixels.data[i + 0] // RED
    pixels.data[i + 500] = pixels.data[i + 1] // GREEN
    pixels.data[i - 550] = pixels.data[i + 2] // Blue
  }
  return pixels
}

function greenScreen(pixels) {
  const levels = {}

  // Be sure to uncomment the selectors in the index.html file for this to work
  document.querySelectorAll(".rgb input").forEach((input) => {
    levels[input.name] = input.value
  })

  for (i = 0; i < pixels.data.length; i = i + 4) {
    red = pixels.data[i + 0]
    green = pixels.data[i + 1]
    blue = pixels.data[i + 2]
    alpha = pixels.data[i + 3]

    if (
      red >= levels.rmin &&
      green >= levels.gmin &&
      blue >= levels.bmin &&
      red <= levels.rmax &&
      green <= levels.gmax &&
      blue <= levels.bmax
    ) {
      // Set the fourth pixel - the alpha pixel - to 0 to be totally transparent
      pixels.data[i + 3] = 0
    }
  }

  return pixels
}

getVideo()

// Once video.play() is called above, it will emit the `canplay` event. This will allow us to pipe an image from the video every 16 ms into the canvas element.
video.addEventListener("canplay", paintToCanvas)
```
