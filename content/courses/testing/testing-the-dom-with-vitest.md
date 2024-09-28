---
title: Testing The DOM With Vitest
description: Learn how to test DOM interactions using Vitest and DOM Testing Library.
modified: 2024-09-28T11:31:14-06:00
---

## Testing the DOM with Vitest

Ah, the DOM. It’s like the wild west of your JavaScript application. Everything can be going smoothly until _boom_, your carefully crafted divs and spans suddenly refuse to cooperate. But hey, no worries—Vitest has your back here. If you’ve been avoiding testing the DOM because it feels like a labyrinth of event handlers, attributes, and deeply nested elements, today is the day you conquer that fear.

Let’s test some DOM interactions and make sure your UI behaves like a reasonable piece of software should. We’re going to use Vitest and a handy-dandy library called `@testing-library/dom`. Buckle up; it’s time to get our hands dirty.

### Setup Vitest and DOM Testing Library

First things first—you gotta have the tools in place. If you don’t have Vitest installed yet, here’s how you do it:

```ts

npm install vitest --save-dev

```

Now, because we’re dealing with DOM stuff, we’ll need some help from `@testing-library/dom`. It provides a lot of useful utilities so we’re not writing crazy verbose `document.createElement` and `appendChild` code every three lines.

Install this bad boy:

```ts

npm install @testing-library/dom --save-dev

```

Now let’s get cracking.

### A Simple DOM Test

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

#### Writing the Test

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

#### What's Happening Here?

- **beforeEach** and **afterEach**: We clean up the DOM before and after every test so no icky states leak between tests. If you don’t clean up, welcome to Flaky Test City—nobody likes it there.
- **fireEvent**: This handy method lets us simulate user interactions. No more manually triggering events. fireEvent lets you simulate clicks, typing, mouseovers—it's your one-stop-shop for mocking user behavior.
- **Assertions**: We’re using **expect** to check if that sneaky button is doing what it’s supposed to. We’re making sure the initial state says, "Click Me." Then, once a user interaction happens (the click), we expect it to say, "Clicked!"

If you run your test suite and everything passes, congratulations! You’ve just successfully tamed the DOM in a test. 🎉

### An Event Listener Gone Rogue?

You know how sometimes, you swear you clicked the button but _nothing happens_ in your UI? Yeah, we've all been there. Let’s test for that scenario too—what if the click event doesn’t fire, or the wrong element is clicked? Mocking out event listeners to ensure they trigger is gold.

Here’s an example where you check if your event listener gets called:

```javascript
import { vi } from 'vitest';
import { createButton } from './your-button-module';

it('should call the event listener on click', () => {
	// Spy on addEventListener
	const spy = vi.spyOn(document, 'addEventListener');

	createButton();

	// Trigger that event
	const button = document.querySelector('button');
	fireEvent.click(button);

	// Assert the spy got called for click events
	expect(spy).toHaveBeenCalledWith('click', expect.any(Function));

	// Might as well clean up that spy so other tests don't get confused
	spy.mockRestore();
});
```

#### Key Takeaway?

With **spyOn**, you can track how many times an event listener got attached, which is super useful for ensuring your code doesn't, say, accidentally add listeners 5 times or forget to attach them altogether. Isolating problems like this makes your code much easier to debug when something goes wrong down the line.

### Wrap it Up, Will Ya?

And there you have it—some simple, but powerful, techniques for testing DOM interactions with Vitest. When you know how to simulate user actions and check DOM state seamlessly, your components become a whole lot less mysterious and a lot more reliable.

Go forth and write those DOM tests. Your future self—that person trying to debug a UI issue at 2 a.m.—will thank you. 🚀

```ts

```
