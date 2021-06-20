# Video speed controller UI

This is a demonstration of using a pill-shaped slider on the right to control video playback from 0.5x to 4.0x.

```html
<body>
  <div class="wrapper">
    <video
      class="flex"
      width="765"
      height="430"
      src="http://clips.vorwaerts-gmbh.de/VfE_html5.mp4"
      loop
      controls
    ></video>
    <p></p>
    <div class="speed">
      <div class="speed-bar">1×</div>
    </div>
  </div>

  <script>
    const speed = document.querySelector(".speed")
    const bar = speed.querySelector(".speed-bar")
    const video = document.querySelector(".flex")

    function handleMove(e) {
      // We can't assume that the top of our pill is at the top of the page
      const y = e.pageY - this.offsetTop
      // Convert our y value to a percentage that we can display
      const percent = y / this.offsetHeight

      // We will set playback speed to be 0.4x to 4.0x normal playback
      const min = 0.4
      const max = 4
      const height = Math.round(percent * 100) + "%"
      // We want to display 0.4 to 4.0 - not 0 to 100
      const playbackRate = percent * (max - min) + min
      bar.style.height = height
      bar.textContent = playbackRate.toFixed(2) + "×"
      video.playbackRate = playbackRate
    }

    speed.addEventListener("mousemove", handleMove)
  </script>
</body>
```
