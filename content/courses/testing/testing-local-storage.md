---
title: Testing Local Storage
description: We can emulate the DOM for those times when we need to test stuff that is not available in Node
modified: 2024-09-28T14:01:05-06:00
---

Let's say we have some code that touches `localStorage`. Normally, our tests run in Node, but `localStorage` isn't in Node. Sure, we _could_ use [mocks](mocks.md) or [stubs](stubs.md), but we could also just emulate the browser environment.

```javascript
test('should properly assign to localStorage', () => {
	localStorage.setItem('bestFramework', 'Vitest');
	expect(localStorage.getItem('bestFramework')).toBe('Vitest');
});
```
