---
title: Getting Started With BDD
description: A guide to behavior-driven development (BDD) using Vitest.
modified: 2024-09-28T18:32:11.031Z
---

## Getting Started with BDD in Vitest

Alright, let’s talk about **Behavior-Driven Development**, or BDD, with **Vitest**. BDD is all about writing tests that describe the *behavior* of your application in a way that both devs and non-devs can understand. So instead of saying, “this function returns `x`,” we’re saying, “the user should see `x` when they click this button.” The tests end up being more like a specification for what your app *should* do, not just what *does* happen under the hood.

Thankfully, **Vitest** makes working with BDD pretty dang easy. Vitest works really well with **Jest-styled syntax**, which is great because it leans into a readable testing flow using **describe** and **it** blocks. It even has the **expect** assertion framework baked right in, so you can focus on that glorious behavior rather than setup.

Let’s dive in!

## Setting Up Vitest

First up, we need Vitest in our project. Head over to your terminal and give it the old:

```bash
npm install vitest --save-dev
```

Cool. Now let's get to writing tests.

You’ll also want to set up a basic config if you don’t have one already. Throw this in a `vite.config.js`:

```js
import { defineConfig } from 'vite';
import vue from '@vitejs/plugin-vue';
import { configDefaults } from 'vitest/config';

export default defineConfig({
	plugins: [vue()],
	test: {
		environment: 'jsdom', // This is important if you're dealing with DOM stuff
		exclude: [...configDefaults.exclude], // Exclude node_modules and other stuff by default
	},
});
```

Once that’s in place, you can run your tests with:

```bash
npx vitest
```

## Writing Our First BDD Test

Let’s say we’ve got a function called `add`. Here’s what the simple behavior might look like:

```js
function add(a, b) {
	return a + b;
}
```

In a strictly academic world, we’d just test that `add(2, 3)` gives us `5`. But in BDD-land, we care more about *what behavior* our function exhibits.

Here’s what that test could look like:

```js
import { describe, it, expect } from 'vitest';
import { add } from './math';

describe('Math functions', () => {
	it('should correctly calculate the sum of two numbers', () => {
		expect(add(2, 3)).toBe(5);
	});

	it('should handle adding negative numbers', () => {
		expect(add(2, -3)).toBe(-1);
	});

	it('should handle adding zero', () => {
		expect(add(2, 0)).toBe(2);
	});
});
```

Ah, clean and readable. Even just reading these tests, you're gathering what the `add` function should *do*, rather than just how it fulfills a functional contract.

**`describe`**: This block groups the tests logically. Here, we group things around “Math functions.” A `describe` block basically describes the suite of tests we’re running.

**`it`**: Inside each describe, you’ve got `it` blocks. These outline specific behaviors or requirements. It follows the format: **“it should do X behavior”**. This is *way* more readable for anyone who’s trying to grok your code.

## Testing User Behavior (A More "Real-World" Example)

Okay cool, `add` is working, but let’s up our game. Let’s assume we’ve got a button that increments a counter when clicked. We want to test that the user sees the count increase.

We’ll need to adjust our setup a bit for the DOM:

```html
<!-- index.html -->
<html lang="en">
	<head>
		<title>Counter</title>
	</head>
	<body>
		<div id="app">
			<p id="counter">0</p>
			<button id="increment">Increment</button>
		</div>

		<script>
			const counterElement = document.getElementById('counter');
			const buttonElement = document.getElementById('increment');
			let count = 0;

			buttonElement.addEventListener('click', () => {
				count += 1;
				counterElement.textContent = count;
			});
		</script>
	</body>
</html>
```

Here’s the test for that behavior:

```js
import { describe, it, expect } from 'vitest';

describe('Counter app', () => {
	it('should increment the count when the button is clicked', () => {
		// Set up DOM
		document.body.innerHTML = `
      <p id="counter">0</p>
      <button id="increment">Increment</button>
    `;

		const button = document.getElementById('increment');
		const counter = document.getElementById('counter');

		// Fake our state
		let count = 0;
		button.addEventListener('click', () => {
			count += 1;
			counter.textContent = count;
		});

		// Simulate the button click
		button.click();

		// Check if the behavior is what we expect
		expect(counter.textContent).toBe('1');
	});
});
```

Rather than testing the internal state directly, we’re checking **the thing we care about most**—what the user sees: that number goes up when the button is clicked. Now when some poor soul is maintaining your app in the future, they can *immediately tell* what’s going on—oh, button click equals counter change, cool. They don’t have to dig into how the inner state of `count` works.

## Mocking DOM Methods

Sometimes you need to mock up DOM methods or third-party services—stuff that gets a little trickier in tests. Let’s mock the `getElementById` function to see how easy it is in Vitest:

```js
import { describe, it, vi } from 'vitest';

describe('DOM tests', () => {
	it('should call getElementById', () => {
		const spy = vi.spyOn(global.document, 'getElementById').mockReturnValue({
			textContent: '0',
		});

		const element = document.getElementById('counter');

		expect(spy).toHaveBeenCalled();
		expect(element.textContent).toBe('0');

		spy.mockRestore();
	});
});
```

Bam! You just mocked `getElementById` and verified it was called. Vitest comes with a mocking library built-in, so you don’t need to yank in extra dependencies just for the basics like spies, mocks, and stubs.

## Conclusion

And there you have it—a quick taste of BDD using Vitest! The big idea here is that BDD is about writing **tests that reflect real-world behavior**. Instead of getting bogged down in the implementation details, you’re checking if the thing *works* the way users and other devs expect it to.

At the end of the day, you want your tests to make sense at a glance. They shouldn’t feel like they’re written in some abstract machine language that only you, right now with your eyes squinted just the right way, can read. They should **tell a story**—what is the thing, and *how* is it supposed to behave.

Now, we’ve just scratched the surface here, but even with this little setup, you’re off to the races with BDD in Vitest. Go on, test behaviors like a boss!

```ts
```
