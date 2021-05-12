# Key Sequence Detection (KONAMI CODE)

An example demonstrating handling key sequence detection - similar to easter eggs that may exist in web pages like [BuzzFeed](https://www.buzzfeed.com).

The real magic lies here:

```js
const pressed = []
const secretCode = "wesbos"

window.addEventListener("keyup", (e) => {
  console.log(e.key)
  pressed.push(e.key)
  pressed.splice(-secretCode.length - 1, pressed.length - secretCode.length)
  if (pressed.join("").includes(secretCode)) {
    console.log("DING DING!")
    cornify_add()
  }
  console.log(pressed)
})
```

This is the first time I've heard of [Cornify.js](https://www.cornify.com). Who knew adding unicorns and rainbows to your web application with JavaScript would be so much fun? 🤣
