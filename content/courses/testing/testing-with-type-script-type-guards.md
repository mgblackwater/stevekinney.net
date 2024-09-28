---
title: Testing Typescript Type Guards With Vitest
description: Learn how to test TypeScript type guards using Vitest.
modified: 2024-09-28T11:31:14-06:00
---

## 1. Testing TypeScript Type Guards with Vitest

You’ve got your TypeScript project chugging along beautifully, types are saving you from 90% of the worst mistakes, and you're feeling good. But now you’ve dipped your toes into **type guards**, and you realize—”Wait, how do I even test these things?”

Good news! Testing type guards with Vitest is pretty straightforward (and dare I say, fun). Let's walk through it together.

### 2. What’s A Type Guard Again?

First, let's take a hot second to refresh our memory. A **type guard** is just a fancy name for a function that checks if a variable is a certain type. Sure, it’s not the most exciting thing you’ll ever do in JavaScript, but it's **super helpful** in making TypeScript a bit smarter.

Example:

```ts
type Animal = { species: string };
type Car = { make: string };

function isAnimal(obj: any): obj is Animal {
	return obj && typeof obj.species === 'string';
}
```

Alright, **`isAnimal`** here is a type guard that checks whether a given object is of type `Animal`. If you feed it trash, it returns false. If you give it an `Animal`, it returns true. Now, let's walk through testing this.

### 3. Setting Up Vitest

You’re already using TypeScript, so let’s use Vitest with it. Go ahead and install it:

```sh
npm install vitest --save-dev
npm install tsx --save-dev
```

Next, throw this into your `package.json` to hook everything up:

```json
"scripts": {
  "test": "vitest"
}
```

After that, you’re rolling. Let's write some tests for our type guard, shall we?

### 4. Writing Tests for Your Type Guard

Here comes the fun part: actually seeing if our type guard passes the sniff test! We want to make sure `isAnimal` correctly identifies animals. So let's start simple.

Here's a test case for `isAnimal`:

```ts
// src/isAnimal.test.ts
import { describe, it, expect } from 'vitest';
import { isAnimal } from './path-to-your-type-guard';

describe('isAnimal', () => {
	it('should return true if the object is an Animal', () => {
		const obj = { species: 'Dog' };
		expect(isAnimal(obj)).toBe(true);
	});

	it('should return false if the object is not an Animal', () => {
		const obj = { make: 'Toyota' };
		expect(isAnimal(obj)).toBe(false);
	});

	it('should return false for null and undefined', () => {
		expect(isAnimal(undefined)).toBe(false);
		expect(isAnimal(null)).toBe(false);
	});

	it('should return false for nonsense data', () => {
		expect(isAnimal(42)).toBe(false);
		expect(isAnimal('species')).toBe(false);
		expect(isAnimal({})).toBe(false);
	});
});
```

Whoa, look at that logic! In three or four simple tests, you’ve got this thing pinned down from every angle—animals, cars, nulls, numbers, strings, and random empty objects—it’s ready for anything!

Let’s break this down real quick:

- The first test checks the happy path: an actual `Animal`. Dogs, cats, whatever—they're animals.
- The next tests some decidedly **un-animal-like** things: what if someone tries to pass a car? You better make sure it returns `false`.
- The third and fourth tests make sure your type guard handles edge cases that **always** pop up in the real world—stuff like `undefined`, `null`, and random garbage people accidentally (or purposefully) pass in.

### 5. Running Your Tests

You’ve written those awesome tests. Hands on your keyboard, run them now:

```sh
npm run test
```

Vitest should give you a friendly green confirmation that your type guard is as solid as a house with bricks for walls. If something fails? Debug it, but you knew that already, right?

### 6. Adding a Little Extra

Ready for a bonus round? Sometimes it’s not enough to check if an object is an `Animal`; you might need to discern **what kind of animal**. For that, you’d extend your type guard to check further properties:

```ts
type Dog = { species: 'Dog'; bark: () => void };

function isDog(obj: any): obj is Dog {
	return isAnimal(obj) && obj.species === 'Dog' && typeof obj.bark === 'function';
}
```

Test this the same way, adding specific cases for **dogs** while leveraging the original `isAnimal`. The key is incremental thinking: **add specificity, test that specificity**. Developers build confidence in their code this way.

### 7. Wrapping Up

TypeScript type guards might seem like just another syntax rule, but they’re a powerful ally when writing bulletproof code. And testing them? It’s **pretty chill**—Vitest makes it easy to check those tricky edge cases that will inevitably arise.

So if you ever catch yourself wondering: "Do I really need to test this?" The answer is always a resounding **yes**, and it’s not half as hard as you think.

Now, go break some TypeScript! (Or better yet, make sure it's **not** broken).

```ts
```
