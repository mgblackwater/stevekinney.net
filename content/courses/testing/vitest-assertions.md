---
modified: 2024-09-16T11:48:21-06:00
---

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
- `toThrow(error)`: Checks if a function throws an error when called. We covered this one in the section on [testing errors](testing-errors.md).

You can see a full list of assertions in [Vitest's documentation](https://vitest.dev/api/expect.html#expect-assertions). But, here are a few quick examples while we're on the topic.

### Testing Arrays That Arrays Contain an Element

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

### Testing to See if a String Matches a Pattern

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

### Using `.not` for Negative Assertions

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

## Some Best Practices with Assertions in Vitest

- **Use the Most Appropriate Matcher:** Choose matchers that provide the most meaningful assertion messages. For example, use `toBeNull` instead of `toBe(null)` for clarity.
- **Test One Thing at a Time:** Each test should assert a single behavior or outcome.
- **Provide Clear Error Messages:** Custom matchers or assertions should return messages that help identify why a test failed.
- **Avoid Implementation Details:** Focus on the output and side effects rather than the internal workings of the code.
- **Clean Up After Tests:** Restore any mocked functions or state to prevent tests from affecting each other.
