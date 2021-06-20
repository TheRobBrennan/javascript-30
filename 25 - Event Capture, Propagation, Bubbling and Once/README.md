# Event Capture, Propagation, Bubbling, and Once

Let's explore `addEventListener`

This example is a very concise and excellent overview of how events cascade upwards through nested elements and their wrappers by default.

What is `capture`? `capture` set to `true` will send events down through a nested element instead of its default of `false` - which will bubble events upward.

What is `once`? It's a new feature that will listen for an event one time and then unbind itself so that no future events will be handled. This would be super useful for handling clicks on the checkout button, for example.
