---
modified: 2024-09-28T15:42:15-06:00
title: Testing Asynchronous Errors
description: Learn how to write unit tests that test for asynchronous errors.
---

When working with asynchronous functions that throw errors, you need to handle promises and rejected errors. In Vitest, you can test asynchronous error handling using `async/await` or the `.rejects` matcher for promises.

## Using `async/await` to Test Rejected Errors

```js
// Asynchronous function to be tested
async function fetchData() {
	throw new Error('Network error');
}

describe('fetchData', () => {
	it('should throw an error when the promise is rejected', async () => {
		// Use try/catch to verify the error thrown by the asynchronous function
		await expect(fetchData()).rejects.toThrow('Network error');
	});
});
```

In this example:

- The `fetchData` function throws an error asynchronously (in a rejected promise).
- The test uses `await expect(fetchData()).rejects.toThrow('Network error')` to verify that the error is correctly thrown and matches the expected message.

## Using `.rejects` for Promise-Based Error Handling

Vitest provides the `.rejects` matcher to test whether a promise is rejected with an error. This is a concise way to check asynchronous errors without needing `try/catch` blocks.

```js
// Asynchronous function to be tested
async function fetchUserData() {
	return Promise.reject(new Error('User not found'));
}

describe('fetchUserData', () => {
	it('should reject with an error', () => {
		// Test that the promise rejects with the correct error message
		return expect(fetchUserData()).rejects.toThrow('User not found');
	});
});
```

In this example:

- `fetchUserData()` returns a rejected promise.
- The test checks that the promise is rejected and the error message matches `'User not found'`.

## Ensuring a Function Does Not Throw

In some cases, you might want to test that a function **does not** throw an error under certain conditions. In Vitest, you can check that a function doesn’t throw using `not.toThrow()`.

```js
describe('divide', () => {
	it('should not throw an error for valid division', () => {
		// Ensure the function does not throw
		expect(() => divide(10, 2)).not.toThrow();
	});
});
```

Here, `expect(() => divide(10, 2)).not.toThrow()` checks that dividing by a non-zero number does not result in an error.

## Combining Error Testing with Other Assertions

You can combine error checking with other assertions to ensure that the function behaves correctly after throwing or catching an error.

```js
describe('divide', () => {
	it('should throw an error for division by zero and return undefined', () => {
		try {
			divide(10, 0);
		} catch (e) {
			expect(e.message).toBe('Division by zero');
		}
	});
});
```

In this example, the `try/catch` block is used to manually catch the error and assert the error message.
