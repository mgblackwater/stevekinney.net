---
title: Testing Event Listeners in Vitest
description: Learn how to test event listeners and user interactions using Vitest.
modified: 2024-09-28T15:44:18-06:00
---

Let’s say we have a simple button that increments a counter every time it’s clicked. Sounds innocent enough, right?

```html
<button id="incrementBtn">Click me!</button>
<div id="counter">0</div>
```

And here’s some JavaScript to handle that click:

```javascript
let count = 0;

const btn = document.getElementById('incrementBtn');
const counterDiv = document.getElementById('counter');

btn.addEventListener('click', () => {
	count++;
	counterDiv.textContent = count;
});
```

Now, that seems to work just fine… until it doesn’t. Let’s drop some tests in there using Vitest, so you can sleep easy at night. Sound good? Cool. Let’s go.

## Writing the Test

First up, we need to simulate this user interaction. When that button is clicked, we want to make sure the counter increments, and we also need to assert that it's updating the DOM. Vitest, especially when paired with [`jsdom`](https://www.npmjs.com/package/jsdom), makes this relatively painless. But I can't promise you won't need some snacks for the road.

```javascript
import { describe, it, expect } from 'vitest';

// Mock the HTML environment with jsdom
document.body.innerHTML = `
  <button id="incrementBtn">Click me!</button>
  <div id="counter">0</div>
`;

// Here would be the JavaScript that adds the event listener (aka business logic)
let count = 0;
const btn = document.getElementById('incrementBtn');
const counterDiv = document.getElementById('counter');

btn.addEventListener('click', () => {
	count++;
	counterDiv.textContent = count;
});

describe('Button Click functionality', () => {
	it('increments the counter on click', () => {
		// Grab the button and div that will be changing
		const btn = document.getElementById('incrementBtn');
		const counterDiv = document.getElementById('counter');

		// Simulate the click event
		btn.click();

		// Our expectation: the counter should increment to 1
		expect(counterDiv.textContent).toBe('1');
	});
});
```

### What’s Going on Here?

– **`document.body.innerHTML`**: This mocks up our DOM so we’re not relying on an actual browser. We’re just saying, “Hey, Vitest, treat this string like it’s the real deal.”

– **`btn.click()`**: Simulates the user dragging that mouse over and clicking the button. In real life, they'd probably miss the button on the first few tries, but the test is more precise.

– **`expect(counterDiv.textContent).toBe('1')`**: This is the main attraction. We clicked the button, so that counter div should’ve updated from "0" to "1". If it hasn’t, something’s wrong, and we’ll need to bust that out.

## Handling More Clicks

Let’s one-up our previous test and make sure this actually handles multiple clicks, not just the first one. Maybe the user gets click-happy, and we want to ensure each click is doing its job.

```javascript
it('increments the counter correctly on multiple clicks', () => {
	const btn = document.getElementById('incrementBtn');
	const counterDiv = document.getElementById('counter');

	// Simulate three clicks
	btn.click();
	btn.click();
	btn.click();

	// Check the value of the counter
	expect(counterDiv.textContent).toBe('3');
});
```

Nice and simple, right? Click three times; expect the number to be 3. This is how you keep your sanity when mysterious bugs creep into your UI after you hand it off to users.

## Troubleshooting Common Pitfalls

You **will** run into weirdness. I’m not psychic, but it’s just the way life goes. Here are a few things to watch out for:

- **DOM Elements Not Found**: If your test is failing with errors like "Cannot read property 'click' of null,” double-check that your `document.body.innerHTML` setup in the test matches what’s in your app.
- **State Management**: If you’ve got **more complex state** (hey, not everyone’s UI is counting buttons), use libraries like **reactive state stores** or **Redux**. Testing click effects might get trickier, but the logic is the same.
