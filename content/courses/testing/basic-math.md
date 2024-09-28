---
title: Implementing Calculator Functions With TDD
description: Learn to implement and test simple arithmetic functions with TDD.
modified: 2024-09-28T13:33:10-06:00
---

It turns out that there is a lot more to math than just adding numbers. Our product manager also wants us to be able to subtract, multiple, and divide numbers too, apparently. Talk about feature creep.

We'll focus on writing pure functions for arithmetic operations and use Vitest to write and run our unit tests. There will be no user interface; the goal is to practice [Test-Driven Development](test-driven-development.md) principles and unit testing.

We'll implement the following calculator functions:

- `add(a, b)`
- `subtract(a, b)`
- `multiply(a, b)`
- `divide(a, b)`

We're going to be good citizens and start by writing the tests first and then take it from there. We already wrote the `add` function. So, we'll pick up from the `subtract` function.

## Subtraction Function

**Exercise**: This one is your job.

### Step 1: Write the Test (Red)

Add tests for the `subtract` function in `calculator.test.js`:

```javascript
// src/calculator.test.js
import { subtract } from '../src/calculator';

describe('subtract', () => {
	it('subtracts two positive numbers', () => {
		expect(subtract(5, 3)).toBe(2);
	});

	it('subtracts a larger number from a smaller number', () => {
		expect(subtract(3, 5)).toBe(-2);
	});

	it('subtracts negative numbers', () => {
		expect(subtract(-5, -3)).toBe(-2);
	});
});
```

**Note:** Remember to import `subtract` from `calculator.js`.

### Step 2: Run the Test and See It Fail

Run the test:

```bash
npm test
```

You'll get an error: `subtract` is not defined.

### Step 3: Write Minimal Code to Pass the Test (Green)

Implement `subtract` in `calculator.js`:

```javascript
// src/calculator.js
export const add = (a, b) => {
	return a + b;
};

export const subtract = (a, b) => {
	return a - b;
};
```

### Step 4: Run the Test Again

Run the test:

```bash
npm test
```

All subtraction tests should pass.

### Step 5: Refactor

Again, the code is simple and needs no refactoring.

## Multiplication Function

### Step 1: Write the Test (Red)

Add tests for `multiply` in `calculator.test.js`:

```javascript
// src/calculator.test.js
import { multiply } from '../src/calculator';

describe('multiply', () => {
	it('multiplies two positive numbers', () => {
		expect(multiply(2, 3)).toBe(6);
	});

	it('multiplies by zero', () => {
		expect(multiply(5, 0)).toBe(0);
	});

	it('multiplies negative numbers', () => {
		expect(multiply(-2, -3)).toBe(6);
	});

	it('multiplies a positive and a negative number', () => {
		expect(multiply(-2, 3)).toBe(-6);
	});
});
```

### Step 2: Run the Test and See It Fail

Run the test:

```bash
npm test
```

Error: `multiply` is not defined.

### Step 3: Write Minimal Code to Pass the Test (Green)

Implement `multiply` in `calculator.js`:

```javascript
// src/calculator.js
export const add = (a, b) => {
	return a + b;
};

export const subtract = (a, b) => {
	return a - b;
};

export const multiply = (a, b) => {
	return a * b;
};
```

### Step 4: Run the Test Again

Run the test:

```bash
npm test
```

All multiplication tests should pass.

### Step 5: Refactor

No refactoring needed. Life is so simple right now.

## Division Function

### Step 1: Write the Test (Red)

Add tests for `divide` in `calculator.test.js`:

```javascript
// src/calculator.test.js
import { divide } from '../src/calculator';

describe('divide', () => {
	it('divides two positive numbers', () => {
		expect(divide(6, 3)).toBe(2);
	});

	it('divides a number by one', () => {
		expect(divide(5, 1)).toBe(5);
	});

	it('divides zero by a number', () => {
		expect(divide(0, 5)).toBe(0);
	});

	it('divides negative numbers', () => {
		expect(divide(-6, -3)).toBe(2);
	});

	it('divides a positive and a negative number', () => {
		expect(divide(-6, 3)).toBe(-2);
	});
});
```

### Step 2: Run the Test and See It Fail

Run the test:

```bash
npm test
```

Error: `divide` is not defined.

### Step 3: Write Minimal Code to Pass the Test (Green)

Implement `divide` in `calculator.js`:

```javascript
// src/calculator.js
export const add = (a, b) => {
	return a + b;
};

export const subtract = (a, b) => {
	return a - b;
};

export const multiply = (a, b) => {
	return a * b;
};

export const divide = (a, b) => {
	return a / b;
};
```

### Step 4: Run the Test Again

Run the test:

```bash
npm test
```

All division tests should pass.

### Step 5: Refactor

Again. No refactoring needed. I'm just trying to make a point here.

```ts
```
