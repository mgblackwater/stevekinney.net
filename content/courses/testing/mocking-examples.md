---
title: Examples In Practice
description: Testing API calls with error handling and spying on console logs.
modified: 2024-09-28T11:31:15-06:00
---

## Examples in Practice

### Testing API Calls with Error Handling

```javascript
// apiService.js
export async function getData() {
	const response = await fetch('/api/data');
	if (!response.ok) {
		throw new Error('Network response was not ok');
	}
	return response.json();
}
```

**Test:**

```javascript
// apiService.test.js
import { test, expect, vi } from 'vitest';
import { getData } from './apiService';

test('handles successful API call', async () => {
	// Arrange
	const mockData = { value: 42 };
	global.fetch = vi.fn(() =>
		Promise.resolve({
			ok: true,
			json: () => Promise.resolve(mockData),
		}),
	);

	// Act
	const data = await getData();

	// Assert
	expect(data).toEqual(mockData);
	expect(global.fetch).toHaveBeenCalledWith('/api/data');

	// Cleanup
	global.fetch.mockRestore();
});

test('handles API error response', async () => {
	// Arrange
	global.fetch = vi.fn(() =>
		Promise.resolve({
			ok: false,
		}),
	);

	// Act & Assert
	await expect(getData()).rejects.toThrow('Network response was not ok');
	expect(global.fetch).toHaveBeenCalledWith('/api/data');

	// Cleanup
	global.fetch.mockRestore();
});
```

**Explanation**:

- We test both successful and failed API calls by mocking `fetch` to return different responses.

### Spying on Console Output

```javascript
// logger.js
export function logMessage(message) {
	console.log(message);
}
```

**Test:**

```javascript
// logger.test.js
import { test, expect, vi } from 'vitest';
import { logMessage } from './logger';

test('logs the correct message', () => {
	// Arrange
	const consoleSpy = vi.spyOn(console, 'log');

	// Act
	logMessage('Hello, Vitest!');

	// Assert
	expect(consoleSpy).toHaveBeenCalledWith('Hello, Vitest!');

	// Cleanup
	consoleSpy.mockRestore();
});
```

**Explanation**:

- We spy on `console.log` to verify that `logMessage` calls it with the correct argument.

```ts
```
