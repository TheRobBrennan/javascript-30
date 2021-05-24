# Native speech recognition

Use speech recognition in the browser - no external APIs or libraries needed.

When the user stops speaking or pauses, a new paragraph is created. Cool, right?

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Speech Detection</title>
  </head>
  <body>
    <div class="words" contenteditable></div>

    <script>
      window.SpeechRecognition =
        window.SpeechRecognition || window.webkitSpeechRecognition

      const recognition = new SpeechRecognition()
      // Set to true so we can see results as we go. Otherwise, nothing will display until the user pauses or ends speaking
      recognition.interimResults = true
      recognition.lang = "en-US"

      let p = document.createElement("p")
      const words = document.querySelector(".words")
      words.appendChild(p)

      recognition.addEventListener("result", (e) => {
        const transcript = Array.from(e.results)
          .map((result) => result[0])
          .map((result) => result.transcript)
          .join("")

        const poopScript = transcript.replace(/poop|poo|shit|dump/gi, "💩")
        p.textContent = poopScript

        // If isFinal is true, let's create a new <p> tag and append the words from the transcription
        if (e.results[0].isFinal) {
          p = document.createElement("p")
          words.appendChild(p)
        }
      })

      // IMPORTANT: We want to start listening after the user pauses or ends speaking
      recognition.addEventListener("end", recognition.start)

      recognition.start()
    </script>

    <style>
      html {
        font-size: 10px;
      }

      body {
        background: #ffc600;
        font-family: "helvetica neue";
        font-weight: 200;
        font-size: 20px;
      }

      .words {
        max-width: 500px;
        margin: 50px auto;
        background: white;
        border-radius: 5px;
        box-shadow: 10px 10px 0 rgba(0, 0, 0, 0.1);
        padding: 1rem 2rem 1rem 5rem;
        background: -webkit-gradient(
            linear,
            0 0,
            0 100%,
            from(#d9eaf3),
            color-stop(4%, #fff)
          ) 0 4px;
        background-size: 100% 3rem;
        position: relative;
        line-height: 3rem;
      }

      p {
        margin: 0 0 3rem;
      }

      .words:before {
        content: "";
        position: absolute;
        width: 4px;
        top: 0;
        left: 30px;
        bottom: 0;
        border: 1px solid;
        border-color: transparent #efe4e4;
      }
    </style>
  </body>
</html>
```