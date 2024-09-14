---
modified: 2024-09-07T14:23:21-06:00
---

Let's start with the world's simplest test.

```js
test('a super simple test', () => {
	expect(true).toBe(true);
});
```

Okay, well this works—but, it's a little ridiculous. Let's actually test some expressions or maybe even a function.

```js
test('another test, but with some logic', () => {
	expect(1 + 1).toBe(2);
});
```

```js
test('a test with a function', () => {
	const add = (a, b) => a + b;
	expect(add(1, 2)).toBe(3);
});
```
