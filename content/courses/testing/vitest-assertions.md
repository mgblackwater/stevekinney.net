---
modified: 2024-09-14T10:19:56-06:00
---

## Understanding Assertions in Vitest

Vitest is a fast unit testing framework built on top of Vite, designed to provide a seamless testing experience for modern JavaScript applications. One of the core components of writing effective tests in Vitest is the use of assertions. Assertions are statements that check whether a specific condition is true. If the condition is false, the test fails, indicating a potential issue in the code.

This post explores how to use assertions in Vitest to write meaningful and reliable tests.

## The Basics of Assertions

In Vitest, assertions are made using the `expect` function, which is similar to other testing libraries like Jest. The general syntax is:

```javascript
expect(actualValue).matcher(expectedValue);
```

- `actualValue`: The result of the code or function you're testing.
- `matcher`: A method that checks the actual value against the expected value.
- `expectedValue`: The value you expect the actual value to be.

## Common Matchers

Vitest provides a variety of matchers to handle different types of comparisons:

- `toBe(value)`: Checks if the actual value is exactly equal to the expected value (using `===`).
- `toEqual(value)`: Checks if the actual value is deeply equal to the expected value.
- `toBeTruthy()`: Checks if the actual value is truthy.
- `toBeFalsy()`: Checks if the actual value is falsy.
- `toBeNull()`: Checks if the actual value is `null`.
- `toBeUndefined()`: Checks if the actual value is `undefined`.
- `toContain(item)`: Checks if an array or string contains a specific item.
- `toHaveLength(length)`: Checks if an array or string has a specific length.
- `toMatch(regexOrString)`: Checks if a string matches a regular expression or contains a substring.
- `toThrow(error)`: Checks if a function throws an error when called.

## Examples in Practice

### Example 1: Testing a Simple Function

Let's test a simple function that adds two numbers:

```javascript
// sum.js
export function sum(a, b) {
  return a + b;
}
```

**Test Using Vitest Assertions:**

```javascript
// sum.test.js
import { test, expect } from 'vitest';
import { sum } from './sum';

test('adds two numbers correctly', () => {
  const result = sum(2, 3);
  expect(result).toBe(5);
});
```

**Explanation:**

- The `sum` function is imported and called with arguments `2` and `3`.
- The `expect` function checks if the result is exactly `5` using the `toBe` matcher.

### Example 2: Testing Object Equality

Consider a function that creates a user object:

```javascript
// createUser.js
export function createUser(name, age) {
  return { name, age };
}
```

**Test Using `toEqual`:**

```javascript
// createUser.test.js
import { test, expect } from 'vitest';
import { createUser } from './createUser';

test('creates a user object', () => {
  const user = createUser('Alice', 30);
  expect(user).toEqual({ name: 'Alice', age: 30 });
});
```

**Explanation:**

- The `toEqual` matcher is used to perform a deep equality check between the `user` object and the expected object.

### Example 3: Testing for Exceptions

Suppose we have a function that throws an error when dividing by zero:

```javascript
// divide.js
export function divide(a, b) {
  if (b === 0) throw new Error('Cannot divide by zero');
  return a / b;
}
```

**Test Using `toThrow`:**

```javascript
// divide.test.js
import { test, expect } from 'vitest';
import { divide } from './divide';

test('throws an error when dividing by zero', () => {
  expect(() => divide(10, 0)).toThrow('Cannot divide by zero');
});
```

**Explanation:**

- The `expect` function wraps the `divide` call in an arrow function.
- The `toThrow` matcher checks if the function throws the specified error.

### Example 4: Testing Arrays and Strings

Testing if an array contains a specific item:

```javascript
// fruits.js
export const fruits = ['apple', 'banana', 'orange'];
```

**Test Using `toContain`:**

```javascript
// fruits.test.js
import { test, expect } from 'vitest';
import { fruits } from './fruits';

test('contains banana in the fruits array', () => {
  expect(fruits).toContain('banana');
});
```

**Explanation:**

- The `toContain` matcher checks if `'banana'` is an element in the `fruits` array.

Testing if a string matches a pattern:

```javascript
// greet.js
export function greet(name) {
  return `Hello, ${name}!`;
}
```

**Test Using `toMatch`:**

```javascript
// greet.test.js
import { test, expect } from 'vitest';
import { greet } from './greet';

test('greets the user with their name', () => {
  const message = greet('Bob');
  expect(message).toMatch(/Hello, Bob!/);
});
```

**Explanation:**

- The `toMatch` matcher checks if the `message` string matches the regular expression `/Hello, Bob!/`.

### Example 5: Testing Asynchronous Code

Testing an asynchronous function that fetches data:

```javascript
// fetchData.js
export async function fetchData() {
  const response = await fetch('/api/data');
  return response.json();
}
```

**Test Using Async/Await and Assertions:**

```javascript
// fetchData.test.js
import { test, expect, vi } from 'vitest';
import { fetchData } from './fetchData';

test('fetches data successfully', async () => {
  const mockData = { id: 1, value: 'Test Data' };

  global.fetch = vi.fn(() =>
    Promise.resolve({
      json: () => Promise.resolve(mockData),
    })
  );

  const data = await fetchData();
  expect(data).toEqual(mockData);
  expect(global.fetch).toHaveBeenCalledWith('/api/data');

  global.fetch.mockRestore();
});
```

**Explanation:**

- The `fetch` function is mocked to return `mockData`.
- The test waits for `fetchData` to resolve and asserts that the returned data matches `mockData`.
- The `toHaveBeenCalledWith` matcher checks that `fetch` was called with the correct URL.

### Example 6: Using `.not` for Negative Assertions

Testing that a value does not meet a condition:

```javascript
test('value is not null or undefined', () => {
  const value = 'Hello';
  expect(value).not.toBeNull();
  expect(value).not.toBeUndefined();
});
```

**Explanation:**

- The `.not` modifier inverts the matcher, checking that the value is neither `null` nor `undefined`.

### Example 7: Testing with Custom Matchers

Vitest allows the creation of custom matchers for more specialized assertions.

**Creating a Custom Matcher:**

```javascript
// vitest.setup.js
import { expect } from 'vitest';

expect.extend({
  toBeWithinRange(received, min, max) {
    const pass = received >= min && received <= max;
    if (pass) {
      return {
        message: () => `expected ${received} not to be within range ${min} - ${max}`,
        pass: true,
      };
    } else {
      return {
        message: () => `expected ${received} to be within range ${min} - ${max}`,
        pass: false,
      };
    }
  },
});
```

**Using the Custom Matcher:**

```javascript
test('number is within range', () => {
  expect(10).toBeWithinRange(5, 15);
});
```

**Explanation:**

- A custom matcher `toBeWithinRange` is defined to check if a number is within a specified range.
- The matcher is then used in a test to assert that `10` is between `5` and `15`.

## Best Practices with Assertions in Vitest

- **Use the Most Appropriate Matcher:** Choose matchers that provide the most meaningful assertion messages. For example, use `toBeNull` instead of `toBe(null)` for clarity.
- **Test One Thing at a Time:** Each test should assert a single behavior or outcome.
- **Provide Clear Error Messages:** Custom matchers or assertions should return messages that help identify why a test failed.
- **Avoid Implementation Details:** Focus on the output and side effects rather than the internal workings of the code.
- **Clean Up After Tests:** Restore any mocked functions or state to prevent tests from affecting each other.
