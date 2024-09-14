---
modified: 2024-09-14T10:23:45-06:00
---

Vitest is a fast and lightweight unit testing framework built on top of Vite, designed to provide a seamless testing experience for modern JavaScript applications. One of its powerful features is the use of hooks, which allow you to set up and manage the testing environment efficiently.

This post explores how to use hooks in Vitest to write clean, maintainable, and effective tests.

## What Are Hooks in Vitest?

Hooks are functions that run at specific stages of your test execution lifecycle. They allow you to perform setup and teardown operations, ensuring that each test runs in a consistent environment. Vitest provides several hooks that mirror those found in other testing frameworks:

- `beforeAll`: Runs once before all tests in a suite.
- `afterAll`: Runs once after all tests in a suite.
- `beforeEach`: Runs before each test in a suite.
- `afterEach`: Runs after each test in a suite.

## Why Use Hooks?

- **Setup and Teardown**: Prepare the testing environment before tests run and clean up afterward.
- **Avoid Code Duplication**: Share common setup code among multiple tests.
- **Test Isolation**: Ensure that tests do not interfere with each other by resetting the state before each test.
- **Improved Readability**: Keep tests focused on assertions rather than setup details.

## Examples in Practice

### Example 1: Using `beforeEach` and `afterEach`

Suppose you're testing a simple counter object:

```javascript
// counter.js
export const counter = {
  value: 0,
  increment() {
    this.value += 1;
  },
  reset() {
    this.value = 0;
  },
};
```

**Test Suite Using Hooks:**

```javascript
// counter.test.js
import { expect, test, beforeEach, afterEach } from 'vitest';
import { counter } from './counter';

beforeEach(() => {
  // Arrange: Reset the counter before each test
  counter.reset();
});

afterEach(() => {
  // Cleanup: Additional teardown if necessary
});

test('counter starts at zero', () => {
  // Act: No action needed

  // Assert
  expect(counter.value).toBe(0);
});

test('increments the counter', () => {
  // Act
  counter.increment();

  // Assert
  expect(counter.value).toBe(1);
});
```

**Explanation:**

- The `beforeEach` hook resets the counter to zero before each test.
- Each test can assume the counter starts at zero, ensuring test isolation.

### Example 2: Using `beforeAll` and `afterAll`

Suppose you have tests that require setting up a database connection:

```javascript
// db.js
export const db = {
  connect: async () => { /* … */ },
  disconnect: async () => { /* … */ },
  clear: async () => { /* … */ },
};
```

**Test Suite Using Hooks:**

```javascript
// db.test.js
import { expect, test, beforeAll, afterAll, beforeEach } from 'vitest';
import { db } from './db';

beforeAll(async () => {
  // Arrange: Connect to the database once before all tests
  await db.connect();
});

afterAll(async () => {
  // Cleanup: Disconnect from the database after all tests
  await db.disconnect();
});

beforeEach(async () => {
  // Arrange: Clear the database before each test
  await db.clear();
});

test('fetches data from the database', async () => {
  // Act: Insert test data and fetch it

  // Assert: Verify the fetched data
});

test('updates data in the database', async () => {
  // Act: Update test data

  // Assert: Verify the data was updated
});
```

**Explanation:**

- `beforeAll` connects to the database once before the tests run.
- `afterAll` disconnects from the database after all tests have completed.
- `beforeEach` clears the database before each test to ensure a clean state.

### Example 3: Nesting Hooks with `describe`

When organizing tests into groups using `describe`, you can have hooks that apply only to that group.

```javascript
import { test, expect, describe, beforeEach } from 'vitest';

let data;

describe('Group 1', () => {
  beforeEach(() => {
    data = { value: 1 };
  });

  test('data value is 1', () => {
    expect(data.value).toBe(1);
  });
});

describe('Group 2', () => {
  beforeEach(() => {
    data = { value: 2 };
  });

  test('data value is 2', () => {
    expect(data.value).toBe(2);
  });
});
```

**Explanation:**

- Each `describe` block has its own `beforeEach` hook.
- The `data` variable is set differently for each group, isolating the tests.

### Example 4: Asynchronous Hooks

Hooks can be asynchronous, allowing you to perform operations like fetching data or waiting for promises.

```javascript
import { test, expect, beforeEach } from 'vitest';

let user;

beforeEach(async () => {
  // Simulate fetching user data
  user = await fetchUser();
});

test('user name is Alice', () => {
  expect(user.name).toBe('Alice');
});

async function fetchUser() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve({ name: 'Alice' });
    }, 100);
  });
}
```

**Explanation:**

- The `beforeEach` hook is marked as `async` and waits for `fetchUser`.
- Tests can rely on `user` being available and populated.

## Best Practices with Hooks in Vitest

- **Keep Hooks Lean**: Avoid putting too much logic in hooks. They should set up the environment, not perform complex operations.
- **Use the Appropriate Hook**: Choose `beforeAll` or `beforeEach` based on whether the setup needs to run once or before every test.
- **Test Isolation**: Ensure that the state is reset between tests to prevent flaky tests.
- **Avoid Shared State**: Be cautious with global variables. If used, make sure they're properly managed within hooks.
- **Handle Asynchronous Code Properly**: Use `async`/`await` in hooks when dealing with asynchronous operations.
- **Error Handling in Hooks**: If a hook fails, Vitest will skip the tests in that suite. Make sure to handle errors appropriately.

## Common Pitfalls

- **Forgetting to Return or Await Promises**: If you have asynchronous code in your hooks, ensure you're returning the promise or using `async`/`await`.
- **Overusing Global Hooks**: Placing too much logic in `beforeAll` can lead to tests that are hard to understand or debug.
- **Not Cleaning Up**: Failing to reset or clean up resources can cause tests to interfere with each other.

## Conclusion

Hooks in Vitest are powerful tools that help manage the setup and teardown of your testing environment. By using hooks effectively, you can write cleaner, more maintainable tests that are easier to understand and less prone to errors. Remember to keep your hooks focused, handle asynchronous operations correctly, and maintain test isolation to get the most out of your Vitest testing experience.
