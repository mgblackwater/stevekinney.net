---
title: "Example 7: Testing With Custom Matchers"
description: Create custom matchers in Vitest for specialized assertions.
modified: 2024-09-28T11:31:16-06:00
---

## Example 7: Testing with Custom Matchers

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

```ts
```
