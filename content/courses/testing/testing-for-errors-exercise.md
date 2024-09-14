---
modified: 2024-09-14T10:34:34-06:00
---
## Testing for Errors

Let's jump back to our basic calculator example from earlier.

### Handling Division by Zero

Dividing by zero is undefined in mathematics. We should handle this case.

### Step 1: Write the Test (Red)

Add a test for division by zero in `calculator.test.js`:

```javascript
// src/calculator.test.js
describe('divide', () => {
  // … previous tests …

  it('throws an error when dividing by zero', () => {
    expect(() => divide(5, 0)).toThrow('Cannot divide by zero');
  });
});
```

### Step 2: Run the Test and See It Fail

Run the test:

```bash
npm test
```

The test fails because an error is not thrown.

### Step 3: Write Minimal Code to Pass the Test (Green)

Modify the `divide` function in `calculator.js`:

```javascript
// src/calculator.js
export function divide(a, b) {
  if (b === 0) {
    throw new Error('Cannot divide by zero');
  }
  return a / b;
}
```

### Step 4: Run the Test Again

Run the test:

```bash
npm test
```

All tests, including the division by zero test, should pass.
