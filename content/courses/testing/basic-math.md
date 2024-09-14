---
modified: 2024-09-14T09:35:51-06:00
---

Test-Driven Development (TDD) is a software development approach where tests are written before writing the actual code. This ensures that the codebase is thoroughly tested and helps in designing better software. In this guide, we'll build a simple calculator application using TDD with **Vitest**, a fast and modern JavaScript testing framework.

We'll focus on writing pure functions for arithmetic operations and use Vitest to write and run our unit tests. There will be no user interface; the goal is to practice TDD principles and unit testing.

## Understanding Test-Driven Development

Test-Driven Developer (TDD) is way simpler than a lot of people want to make it sound. You basically follow these steps:

1. **Write a Test**: Write a test for the next bit of functionality you want to add.
2. **Run the Test and See It Fail**: This confirms that the test is detecting the absence of the desired functionality.
3. **Write the Minimal Code to Pass the Test**: Implement just enough code to make the test pass.
4. **Refactor**: Improve the code while keeping the tests passing.
5. **Repeat**: Continue with the next functionality.

This cycle is often referred to as **Red-Green-Refactor**:

- **Red**: Write a failing test.
- **Green**: Write code to pass the test.
- **Refactor**: Improve the code.

## Writing Tests and Code

We'll implement the following calculator functions:

- `add(a, b)`
- `subtract(a, b)`
- `multiply(a, b)`
- `divide(a, b)`

We're going to be good citizens and start by writing the tests first and then take it from there.

### Addition Function

#### Step 1: Write the Test (Red)

Create `src/calculator.test.js` and write the test for the `add` function:

```javascript
// src/calculator.test.ts
import { describe, it, expect } from 'vitest';
import { add } from '../calculator';

describe('Calculator', () => {
  describe('add', () => {
    it('adds two positive numbers', () => {
      expect(add(2, 3)).toBe(5);
    });

    it('adds two negative numbers', () => {
      expect(add(-2, -3)).toBe(-5);
    });

    it('adds a positive and a negative number', () => {
      expect(add(5, -3)).toBe(2);
    });
  });
});
```

#### Step 2: Run the Test and See It Fail

Run the test:

```bash
npm test
```

You'll see an error because `add` is not defined.

#### Step 3: Write Minimal Code to Pass the Test (Green)

Implement the `add` function in `src/calculator.js`:

```javascript
// src/calculator.js
export function add(a, b) {
  return a + b;
}
```

#### Step 4: Run the Test Again

Run the test:

```bash
npm test
```

All tests should pass.

#### Step 5: Refactor

In this case, the code is straightforward. No refactoring needed. We're off the hook here.

### 2. Subtraction Function

**Exercise**: This one is your job.

#### Step 1: Write the Test (Red)

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

#### Step 2: Run the Test and See It Fail

Run the test:

```bash
npm test
```

You'll get an error: `subtract` is not defined.

#### Step 3: Write Minimal Code to Pass the Test (Green)

Implement `subtract` in `calculator.js`:

```javascript
// src/calculator.js
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}
```

#### Step 4: Run the Test Again

Run the test:

```bash
npm test
```

All subtraction tests should pass.

#### Step 5: Refactor

Again, the code is simple and needs no refactoring.

### 3. Multiplication Function

#### Step 1: Write the Test (Red)

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

#### Step 2: Run the Test and See It Fail

Run the test:

```bash
npm test
```

Error: `multiply` is not defined.

#### Step 3: Write Minimal Code to Pass the Test (Green)

Implement `multiply` in `calculator.js`:

```javascript
// src/calculator.js
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}

export function multiply(a, b) {
  return a * b;
}
```

#### Step 4: Run the Test Again

Run the test:

```bash
npm test
```

All multiplication tests should pass.

#### Step 5: Refactor

No refactoring needed.

### 4. Division Function

#### Step 1: Write the Test (Red)

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

#### Step 2: Run the Test and See It Fail

Run the test:

```bash
npm test
```

Error: `divide` is not defined.

#### Step 3: Write Minimal Code to Pass the Test (Green)

Implement `divide` in `calculator.js`:

```javascript
// src/calculator.js
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}

export function multiply(a, b) {
  return a * b;
}

export function divide(a, b) {
  return a / b;
}
```

#### Step 4: Run the Test Again

Run the test:

```bash
npm test
```

All division tests should pass.

#### Step 5: Refactor

No refactoring needed.
