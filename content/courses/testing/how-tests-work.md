---
title: Insist That a Test Fails
description: Explore techniques to ensure tests fail when expected.
modified: 2024-09-28T18:32:10.879Z
---

Because it didn't fail.

These seems banal, but it'll be important later, so I'm going to say it again and again: There is *no such thing* as a passing test. There are only tests that didn't fail.

If we think about how a test suite works, it's something that runs your code and keeps track of the times where an error was thrown. An `expect` that didn't get what it expects throws an error, which fails your test. If your test passes, it's not because there is something inherently correct about your code. There just isn't anything inherently *wrong*.

## Insist that a Test Fails

I don't ever use this one, but I'm sure it's there for a reason and maybe it'll be useful for you. You can explicitly call out that you want a test to fail.

```ts
test.fails('works with "test" as well', () => {
	expect(true).toBe(false);
});
```

If you want me to provide a rationale for why and when you'd use this, here's the best that I have for you. Your tests will pass for **one reason**: *None of your assertions failed*. This is by design, so there is no use complaining about it.

There is a catch of course; did your test pass because:

- all of your assertions passed,
- or because *none* of them failed?

All a test runner really does is run your code and keep track of every time an error is thrown. A failing test throws an error. So, the logic stands that if no error was thrown, then the test passed.

For example, this test passes:

```ts
test('works with "test" as well', () => {
	console.log("I don't do anything!");
});
```

But, like maybe we don't want it to, right?

```ts
test.fails('works with "test" as well', () => {
	console.log("I don't do anything!");
});
```

A test that we *expect* to fail, might look something like this contrived example:

```ts
it.fails('should be able to expect a test to fail', () => {
	expect(false).toBe(true);
});
```

This is particularly insidious with asynchronous code.

```ts
test('works with "test" as well', () => {
	setTimeout(() => {
		expect('This should fail.').toBe('Totally not the same.');
	}, 1000);
});
```

It doesn't fail because asynchronous code is hard. But, if we annotate with the specific expectation that it fails, we can *at least* get some feedback.

```ts
test.fails('works with "test" as well', () => {
	setTimeout(() => {
		expect('This should fail.').toBe('Totally not the same.');
	}, 1000);
});
```

We'll talk more about this in the section on [testing asynchronous code](testing-asynchronous-code.md), but if your code returns a promise, then Vitest will wait for that promise to resolve.

```ts
test('works with "test" as well', () => {
	return new Promise((done) => {
		setTimeout(() => {
			expect('This should fail.').not.toBe('Totally not the same.');

			done(null);
		}, 500);
	});
});
```

## Expect Expectation

If you *really* want to make sure that your tests aren't just passing because they're not failing, you can expect that there should be one or more assertions in your test.

```ts
expect.hasAssertions();
```

Alternatively, you can expect a given number of assertions.

```ts
expect.assertions(1);
```

This will come in handy when we talk about [testing asynchronous code](testing-asynchronous-code.md), but first let's dig a little bit deeper in [Beyond Strict Equality](beyond-strict-equality.md).
