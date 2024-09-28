---
title: What Is Test-Driven Development
description: Exploring TDD with Vitest through the Red-Green-Refactor cycle.
modified: 2024-09-28T11:31:15-06:00
---

## What is Test-Driven Development

Alright, so you're a developer—and like me, you probably ship first and ask questions later. But hey, sometimes we gotta be grown-ups. Enter **Test-Driven Development**, or as the cool kids call it, **TDD**.

Here’s the deal: with TDD, you write your tests **before** you write the code. Yeah, I know—it sounds backwards. It’s like buying IKEA instructions and reading them before putting the chair together (which, let’s be honest, we rarely do).

But trust me—this weird workflow can be magical. TDD forces you to think about what your code should do before you start slinging it together. You write a test, watch it fail (because there's no code yet), then write just enough code to pass the test. Rinse and repeat. Boom.

Since we’re using **Vitest**—our trusty sidekick for this adventure—let’s dive in.

## Setting Up Vitest

You probably already have a project. If not, spin up a new one however you like: `npm init` or whatever floats your boat. Then, install **Vitest**:

```bash
npm install vitest --save-dev
```

Awesome. Now, let’s create a `vitest.config.js` because we want our tests tucked neatly into a `tests` folder instead of scattered across the wild:

```js
// vitest.config.js
export default {
	test: {
		include: ['tests/**/*.test.js'],
	},
};
```

Let’s create a simple directory structure:

```ts
- src/
  - mathFuncs.js
- tests/
  - mathFuncs.test.js
- vitest.config.js
```

We're all set! Now, on to the good stuff—writing tests before code like the productive wizards we know we can be.

## The Red-Green-Refactor Cycle

TDD lives by this principle: **Red-Green-Refactor**.

1. **Red**: Write a test, run it, and watch it fail.
2. **Green**: Write the minimum amount of code to make the test pass.
3. **Refactor**: Clean up the code while keeping tests green.

Let's apply this to a simple function—a very sophisticated `add(a, b)` function because calculators are hard.

### Step 1: Write a Failing Test

Time to break things intentionally. Let’s write a test for our function that doesn’t even exist yet.

```js
// tests/mathFuncs.test.js
import { describe, it, expect } from 'vitest';
import { add } from '../src/mathFuncs';

describe('add()', () => {
	it('should return the sum of two numbers', () => {
		expect(add(2, 3)).toBe(5);
	});

	it('should return a negative sum for negative numbers', () => {
		expect(add(-1, -1)).toBe(-2);
	});
});
```

Run **Vitest**:

```bash
npx vitest
```

Result? Red. You’ll see something like this:

```ts
FAIL tests/mathFuncs.test.js > add() should return the sum of two numbers
ReferenceError: add is not defined
```

Good. I like seeing a good ol' failure. It means we're on the right track.

### Step 2: Make It Pass

Alright, now it’s time to create the world’s most basic `add()` function.

```js
// src/mathFuncs.js
export const add = (a, b) => a + b;
```

Run **Vitest** again:

```bash
npx vitest
```

If you did everything right (and I trust you did), the tests are now **green**:

```ts
 PASS  tests/mathFuncs.test.js > add() should return the sum of two numbers
```

Congrats—you’ve written a failing test and made it pass. Bravo, take a bow.

### Step 3: Refactor

Surprise, surprise—there’s not much we can refactor here. But the important thing to take away is this: you can **safely refactor your code without fear**, because you have tests to back you up. If you did something wrong in your refactor, the tests will catch it.

## Let’s TDD Some Edge Cases

The party’s not over. What happens if someone tries to use our `add` function with non-numbers?

Let’s break it again by adding a new test case:

```js
it('should throw an error if inputs are not numbers', () => {
	expect(() => add('a', 2)).toThrow();
});
```

Run the tests:

```ts
FAIL  tests/mathFuncs.test.js > add() should throw an error if inputs are not numbers
AssertionError: Expected function to throw an error
```

Next, we write the minimum amount of code to make this test pass:

```js
export const add = (a, b) => {
	if (typeof a !== 'number' || typeof b !== 'number') {
		throw new Error('Parameters must be numbers');
	}
	return a + b;
};
```

Run the test again, and boom, we’re green again.

## Recap of TDD with Vitest

There you have it! We journeyed from failure to success using the magic formula of **Red-Green-Refactor**. We wrote the test **first** (which, let’s be real—that’s usually the hard part), and only then did we write the actual logic.

With Vitest handling the messy parts (like running all the tests for you), we got to focus purely on writing and refining our code.

Testing might feel like a chore, but with **TDD**, it’s like having a safety net while you’re trapezing through code. You can ship with confidence and maybe—just maybe—spend fewer hours debugging.

```ts
```
