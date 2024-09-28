---
modified: 2024-09-16T11:36:35-06:00
---

If we take what we learned in the previous section, then we might assume that these tests should pass.

```js
test.fails('a test with an array', () => {
	const numbers = [1, 2, 3];
	expect(numbers).toBe([1, 2, 3]);
});

test.fails('a test with an object', () => {
	const someObject = { a: 1, b: 2 };
	expect(someObject).toBe({ a: 1, b: 2 });
});
```

But, they don't. Why?

In JavaScript, the `toBe()` assertion checks for **referential equality**, meaning that it compares the memory references of the objects being compared. In the tests above, the arrays and objects are not considered equal because they are different instances in memory, even though their contents are the same.

To fix this, you can use the `toEqual()` assertion instead, which performs a deep comparison of the objects' properties. This will ensure that the values within the arrays and objects are compared, rather than their memory references.

Here's an updated version of the test cases:

```js
test('a test with an array', () => {
	const numbers = [1, 2, 3];
	expect(numbers).toEqual([1, 2, 3]);
});

test('a test with an object', () => {
	const someObject = { a: 1, b: 2 };
	expect(someObject).toEqual({ a: 1, b: 2 });
});
```

By using `toEqual()`, the tests will now pass because the values of the arrays and objects are compared instead of their memory references. Simple enough, right?
