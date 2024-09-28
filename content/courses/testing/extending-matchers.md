---
title: Extending Matchers In Vitest
description: Learn how to extend custom matchers in Vitest with examples.
modified: 2024-09-28T11:31:15-06:00
---

## Extending Matchers in Vitest

Alright, let's talk **extending matchers** in Vitest, because while the built-in matchers are solid, sometimes you just need your test assertions to feel a little more _you_. Maybe you're tired of writing elaborate logic in every `expect` call, or perhaps you want to sharpen your abstractions to better match your app or business needs.

**No worries, custom matchers to the rescue!** When Vitest doesn't have exactly what you need, it's pretty darn easy to add your own.

### Why Extend Matchers?

Let’s paint a real-world scenario. Imagine you’ve got a test where you’re repeatedly checking if a user’s score is within a certain range. You could always write multiple assertions, but there's something clunky about seeing the same logic scattered across a dozen tests. Instead, you can define a custom matcher like `toBeWithinRange`.

### How to Extend Matchers in Vitest

First off, Vitest lets you extend `expect` by defining your own custom matchers. These are basically new functions that you can chain onto `expect`.

```js
import { expect } from 'vitest';

// Define your custom matcher
expect.extend({
	toBeWithinRange(received, floor, ceiling) {
		const pass = received >= floor && received <= ceiling;
		if (pass) {
			return {
				message: () => `expected ${received} not to be within range ${floor} - ${ceiling}`,
				pass: true,
			};
		} else {
			return {
				message: () => `expected ${received} to be within range ${floor} - ${ceiling}`,
				pass: false,
			};
		}
	},
});
```

### Breaking It Down

1. **First, tap into `expect.extend`** and pass an object. The key here is the name you want for your custom matcher, in this case, `toBeWithinRange`. It’s the function that will get called for whatever assertion you’re cooking up.
2. **The function signature looks like this**:

   - `received`: The actual value that your test is receiving.
   - `floor` and `ceiling`: These are extra arguments you pass in while calling the matcher.

3. **The `pass` variable** determines if the test should pass or fail depending on whether `received` falls within the range.
4. **Return an object** with a super friendly `message` for when things blow up, and a `pass` flag to indicate whether the test is a success.

### Using Your New Matcher

```js
test('numeric ranges', () => {
	expect(10).toBeWithinRange(5, 15); // 🤌 Smooth passing
	expect(20).toBeWithinRange(5, 15); // 💥 This one fails!
});
```

Look at that! Now, instead of doing repeated, clunky comparisons like `expect(val).toBeGreaterThanOrEqual(floor)` paired with `expect(val).toBeLessThanOrEqual(ceiling)`, you can just say exactly what you mean with `toBeWithinRange`.

### A More Interesting Example

Let’s say you’re working with **dates**. Testing for "is this date after that date?" could be a little annoying with vanilla matchers.

Here’s a custom matcher for checking if one date is later than another:

```js
expect.extend({
	toBeLaterThan(received, comparedDate) {
		const pass = new Date(received) > new Date(comparedDate);
		if (pass) {
			return {
				message: () => `expected ${received} not to be later than ${comparedDate}`,
				pass: true,
			};
		} else {
			return {
				message: () => `expected ${received} to be later than ${comparedDate}`,
				pass: false,
			};
		}
	},
});
```

Boom! Now you can write some crisp, clear date comparisons:

```js
test('date comparison', () => {
	expect('2023-10-10').toBeLaterThan('2023-09-01'); // ✅ True.
	expect('2023-05-15').toBeLaterThan('2023-07-01'); // 💣 False.
});
```

### Best Practices

1. **Keep the matcher simple**. You don’t want to write a novel for each matcher. Remember, code is read more often than it’s written, so make sure that your custom matcher is easy to understand at a glance.
2. **Detailed error messages**. When a test fails, the developer (spoiler: future you) needs to understand _why_ things exploded. A good message makes debugging way less painful.
3. **Leverage context for your app**. Extend matchers when you find yourself repeating specific checks. In the land of testing, **DRY** doesn’t just stand for "Don’t Repeat Yourself", it stands for "**Don’t Rage Yet**", because tests are meant to stay calm and concise.

That’s it! Extending matchers in Vitest is not only painless, but dare I say, _kinda fun_? It allows you to tailor your tests so they make sense not just to the test suite but also to you, the very busy developer who would prefer fewer `console.logs` and more passing green tests.

Happy testing! 🎉

```ts

```
