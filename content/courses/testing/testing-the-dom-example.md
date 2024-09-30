---
title: Testing the DOM with Vitest
description: Learn how to test DOM interactions using Vitest and DOM Testing Library.
modified: 2024-09-28T16:08:44-06:00
---

Imagine you have a button, and when you click it, something happens. Classic case, right? Let’s whip up a basic test for that interaction.

Here’s a simple function that creates our DOM structure and adds some behavior:

```javascript
function createButton() {
	const btn = document.createElement('button');
	btn.textContent = 'Click Me';

	btn.addEventListener('click', () => {
		btn.textContent = 'Clicked!';
	});

	document.body.appendChild(btn);
}
```

Nothing fancy—just a button that says "Click Me," and when you click it, it changes to "Clicked!" It’s like a microwave tutorial level for DOM testing.

## Writing the Test

Now let's test this sucker and make sure it's not lying to us. We’ll add a new test case to ensure the button successfully changes text on click.

```javascript
import { beforeEach, afterEach, describe, it, expect } from 'vitest';
import { fireEvent } from '@testing-library/dom';
import { createButton } from './your-button-module'; // adjust as per your file structure

describe('Button DOM interaction', () => {
	beforeEach(() => {
		// Clean slate. Make sure nothing weird from previous tests is hanging around.
		document.body.innerHTML = '';
	});

	afterEach(() => {
		// Clean up after ourselves.
		document.body.innerHTML = '';
	});

	it('should change button text on click', () => {
		// Step 1: Set up the DOM.
		createButton();

		// Step 2: Select the button you created.
		const button = document.querySelector('button');
		expect(button).not.toBeNull();
		expect(button.textContent).toBe('Click Me');

		// Step 3: Simulate the click event.
		fireEvent.click(button);

		// Step 4: Assert the text has changed.
		expect(button.textContent).toBe('Clicked!');
	});
});
```

## What's Happening Here?

- **beforeEach** and **afterEach**: We clean up the DOM before and after every test so no icky states leak between tests. If you don’t clean up, welcome to Flaky Test City—nobody likes it there.
- **fireEvent**: This handy method lets us simulate user interactions. No more manually triggering events. fireEvent lets you simulate clicks, typing, mouseovers—it's your one-stop-shop for mocking user behavior.
- **Assertions**: We’re using **expect** to check if that sneaky button is doing what it’s supposed to. We’re making sure the initial state says, "Click Me." Then, once a user interaction happens (the click), we expect it to say, "Clicked!"

If you run your test suite and everything passes, congratulations! You’ve just successfully tamed the DOM in a test.
