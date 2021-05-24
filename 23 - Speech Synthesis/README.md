# Speech synthesis

The Voiceinator 5000 - an example exploring synthesizing a text box with available voices on the client 🤣

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Speech Synthesis</title>
    <link
      href="https://fonts.googleapis.com/css?family=Pacifico"
      rel="stylesheet"
      type="text/css"
    />
    <link rel="stylesheet" href="style.css" />
  </head>
  <body>
    <div class="voiceinator">
      <h1>The Voiceinator 5000</h1>

      <select name="voice" id="voices">
        <option value="">Select A Voice</option>
      </select>

      <label for="rate">Rate:</label>
      <input name="rate" type="range" min="0" max="3" value="1" step="0.1" />

      <label for="pitch">Pitch:</label>

      <input name="pitch" type="range" min="0" max="2" step="0.1" />
      <textarea name="text">Hello! I love JavaScript 👍</textarea>
      <button id="stop">Stop!</button>
      <button id="speak">Speak</button>
    </div>

    <script>
      const msg = new SpeechSynthesisUtterance()
      let voices = []
      const voicesDropdown = document.querySelector('[name="voice"]')
      const options = document.querySelectorAll('[type="range"], [name="text"]')
      const speakButton = document.querySelector("#speak")
      const stopButton = document.querySelector("#stop")

      // Use the text in our input box
      msg.text = document.querySelector('[name="text"]').value

      function populateVoices() {
        voices = this.getVoices()

        // Build a dropdown list of voices that are available on the client system
        voicesDropdown.innerHTML = voices
          // Trim our list of voices to English only
          .filter((voice) => voice.lang.includes("en"))
          .map(
            (voice) =>
              `<option value="${voice.name}">${voice.name} (${voice.lang})</option>`
          )
          .join("")
      }

      function setVoice() {
        // We are bound to the this.value (e.g. "Alva") - use that to look up the voice.name
        msg.voice = voices.find((voice) => voice.name === this.value)
        toggle()
      }

      function toggle(startOver = true) {
        // Cancel any speech in progress
        speechSynthesis.cancel()
        if (startOver) {
          speechSynthesis.speak(msg)
        }
      }

      function setOption() {
        console.log(this.name, this.value)
        msg[this.name] = this.value
        toggle()
      }

      speechSynthesis.addEventListener("voiceschanged", populateVoices)
      voicesDropdown.addEventListener("change", setVoice)
      options.forEach((option) => option.addEventListener("change", setOption))
      speakButton.addEventListener("click", toggle)
      stopButton.addEventListener("click", () => toggle(false))
    </script>
  </body>
</html>
```
