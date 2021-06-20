# Countdown clock

Interesting note about how scrolling in iOS causes any intervals to pause. We are using `now` and `then` to track how much time has elapsed.

```html
<body>
  <div class="timer">
    <div class="timer__controls">
      <button data-time="20" class="timer__button">20 Secs</button>
      <button data-time="300" class="timer__button">Work 5</button>
      <button data-time="900" class="timer__button">Quick 15</button>
      <button data-time="1200" class="timer__button">Snack 20</button>
      <button data-time="3600" class="timer__button">Lunch Break</button>
      <form name="customForm" id="custom">
        <input type="text" name="minutes" placeholder="Enter Minutes" />
      </form>
    </div>
    <div class="display">
      <h1 class="display__time-left"></h1>
      <p class="display__end-time"></p>
    </div>
  </div>

  <script src="scripts-START.js"></script>
</body>
```

```js
let countdown
const timerDisplay = document.querySelector(".display__time-left")
const endTime = document.querySelector(".display__end-time")
// Select anything with the data-time attribute in our HTML
const buttons = document.querySelectorAll("[data-time]")

function timer(seconds) {
  // clear any existing timers
  clearInterval(countdown)

  const now = Date.now()
  const then = now + seconds * 1000

  // Invoke and display our time left without waiting one second for the first interval to trigger
  displayTimeLeft(seconds)
  displayEndTime(then)

  countdown = setInterval(() => {
    const secondsLeft = Math.round((then - Date.now()) / 1000)
    // check if we should stop it!
    if (secondsLeft < 0) {
      clearInterval(countdown)
      return
    }
    // display it
    displayTimeLeft(secondsLeft)
  }, 1000)
}

function displayTimeLeft(seconds) {
  // Convert our seconds value to minutes and seconds
  const minutes = Math.floor(seconds / 60)
  const remainderSeconds = seconds % 60

  const display = `${minutes}:${
    remainderSeconds < 10 ? "0" : ""
  }${remainderSeconds}`
  document.title = display
  timerDisplay.textContent = display
}

function displayEndTime(timestamp) {
  const end = new Date(timestamp)
  const hour = end.getHours()
  const adjustedHour = hour > 12 ? hour - 12 : hour
  const minutes = end.getMinutes()
  endTime.textContent = `Be Back At ${adjustedHour}:${
    minutes < 10 ? "0" : ""
  }${minutes}`
}

function startTimer() {
  const seconds = parseInt(this.dataset.time)
  timer(seconds)
}

buttons.forEach((button) => button.addEventListener("click", startTimer))
// Note how we can use document.customForm to select our form 🤓
document.customForm.addEventListener("submit", function (e) {
  e.preventDefault()
  const mins = this.minutes.value
  console.log(mins)
  timer(mins * 60)
  this.reset()
})
```
