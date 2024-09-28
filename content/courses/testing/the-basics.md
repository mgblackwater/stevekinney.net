---
title: Starting With Simple Tests In Vitest
description: Learn how to test basic expressions and functions using Vitest.
modified: 2024-09-28T11:31:14-06:00
---

Let's start with the world's simplest test.

```js
import { test, expect } from 'vitest';

test('a super simple test', () => {
	expect(true).toBe(true);
});
```

At the highest level, we can see the following:

- There is a functioned called `test` that takes two arguments
  - A string that represents the name of the test.
  - A function that contains the body of the test.
- Inside of that function, we use an [assertion library](vitest-assertions.md) to make a statement about what we expect to be the way the world works.
  - In this case, we're expecting this two things to be equal and they are.

Okay, well this works—but, it's a little ridiculous. Let's actually test some expressions or maybe even a function.

```js
test('another test, but with some logic', () => {
	expect(1 + 1).toBe(2);
});
```

```js
test('a test with a function', () => {
	const add = (a, b) => a + b;
	expect(add(1, 2)).toBe(3);
});
```

The selling point here is that when we write tests, we can make a bunch of statements about how we expect our code to work. Our test suite's job is to save us the hassle of having to manually check on all of these things. Instead, the suite will grab our code and make sure that everything still works the way that we expect as we go about our business adding features and refactoring code.

Generally speaking, it's unlikely that our `add` function would live inside of a test. More likely, it's a utility function of some kind that we'd use in our application. A _slightly_ more realistic example would look something like the example below.

```javascript
// calculator.js
export const add = (a, b) => {
	return a + b;
};
```

```javascript
// calculator.test.js
import { it, describe, expect } from 'vitest';
import { add } from './calculator.js';

describe('add', () => {
	it('adds two positive numbers', () => {
		expect(add(1, 2)).toBe(3);
	});
});
```

> [!TIP] `test` and `it` are aliases of each other.
> You can use `test` and `it` interchangeably. They're just stylistic.

## Adding Tests

Every test you write comes at some infinitesimally small cost, which I suppose can add up over time. The unhelpful rule for how many tests you should write is "as many as you need." But, generally speaking—we want to at least cover some of the edge cases.

```javascript
// src/calculator.test.js
import { describe, it, expect } from 'vitest';
import { add } from './calculator.js';

describe('add', () => {
	it('adds two positive numbers', () => {
		expect(add(1, 2)).toBe(3);
	});

	it('adds two negative numbers', () => {
		expect(add(-2, -3)).toBe(-5);
	});

	it('adds a positive and a negative number', () => {
		expect(add(5, -3)).toBe(2);
	});
});
```

We'll add a few more in a later section, but this is a pretty good start for now. Especially because we haven't looked at [running your tests](running-tests.md) just yet.

```ts

```
