---
title: Testing if a Function Throws an Error in Vitest
description: Learn how to test error handling in Vitest for synchronous and asynchronous functions.
modified: 2024-09-28T11:31:14-06:00
---

When testing JavaScript functions, it’s important to verify how your code handles errors, particularly in cases where errors are expected to be thrown under certain conditions. Vitest provides a robust set of tools for testing both synchronous and asynchronous error handling, allowing you to check if a function throws an error and whether the correct error message or type is produced.

## Why Test Error Throwing?

- **Ensure Proper Error Handling**: Verifying that functions throw errors correctly helps ensure your application behaves as expected when things go wrong.
- **Prevent Silent Failures**: Testing for thrown errors helps avoid situations where the code fails silently, making debugging difficult.
- **Enforce Validation**: Testing functions that are expected to throw errors for invalid input or failed operations ensures your code properly validates and guards against incorrect behavior.

## Basic Error Testing with `toThrow`

For synchronous functions that may throw errors, you can use Vitest’s `toThrow` matcher. This matcher checks whether a function throws an error and can be used to check the error message as well.

Here’s how to test that a function throws an error:

```js
// Function to be tested
function divide(a, b) {
	if (b === 0) {
		throw new Error('Division by zero');
	}
	return a / b;
}

describe('divide', () => {
	it('should throw an error when dividing by zero', () => {
		// Check if the function throws an error
		expect(() => divide(10, 0)).toThrow();

		// Check that it throws with the correct message
		expect(() => divide(10, 0)).toThrow('Division by zero');
	});
});
```

In this example:

- The function `divide(10, 0)` throws an error when dividing by zero.
- The `expect(() => divide(10, 0)).toThrow()` assertion checks that an error is thrown.
- The `expect(() => divide(10, 0)).toThrow('Division by zero')` ensures that the error message is correct.

## Testing Specific Error Types

You can also check for specific error types using `instanceof` checks within the `toThrow` matcher. This is useful when your function may throw custom error types or when you want to verify that a specific kind of error is thrown.

```js
// Custom error type
class InvalidArgumentError extends Error {
	constructor(message) {
		super(message);
		this.name = 'InvalidArgumentError';
	}
}

// Function to be tested
function squareRoot(x) {
	if (x < 0) {
		throw new InvalidArgumentError('Negative numbers are not allowed');
	}
	return Math.sqrt(x);
}

describe('squareRoot', () => {
	it('should throw InvalidArgumentError for negative numbers', () => {
		// Check that the function throws the specific error type
		expect(() => squareRoot(-1)).toThrow(InvalidArgumentError);

		// Check the error message
		expect(() => squareRoot(-1)).toThrow('Negative numbers are not allowed');
	});
});
```

In this example:

- `squareRoot(-1)` throws an `InvalidArgumentError`.
- The test verifies that the specific error type is thrown and that the message matches the expected error message.

## Testing Asynchronous Functions That Throw Errors

When working with asynchronous functions that throw errors, you need to handle promises and rejected errors. In Vitest, you can test asynchronous error handling using `async/await` or the `.rejects` matcher for promises.

### Using `async/await` to Test Rejected Errors:

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

## Conclusion

Testing whether a function throws an error in Vitest is an essential part of validating error handling in your code. With matchers like `toThrow`, `rejects.toThrow`, and `not.toThrow`, Vitest provides powerful tools for both synchronous and asynchronous error testing. These tools allow you to ensure that your code handles errors properly, preventing silent failures and ensuring that invalid inputs or edge cases are properly managed.

```ts

```
