# CSS and JS Clock

This project displays an animated clock based on the current time in JavaScript.

There were some interesting points for me to consider - which I have included below for reference.

```css
.hand {
  /* ... */

  /* We want this element to start transforming at the far right, so use 100%. Default is to transform around the center - which would be the same as setting this to 50% */
  transform-origin: 100%;

  /* By default, the line appears to be at nine o'clock. Rotate 90 degrees, so it starts in the twelve o'clock position */
  transform: rotate(90deg);

  /* Let's define our transition to give the effect of snapping the hand just past the point it's intended */
  transition: all 0.05s;
  transition-timing-function: cubic-bezier(0.1, 2.7, 0.58, 1);
  /* FUN FACT: I didn't know you can click to the left of the cubic-bezier and drag and drop your shape! */
}
```
