---
modified: 2024-09-14T09:46:42-06:00
---

Most of us have been living in the "everything should be immutable" world long enough to know that there is a difference between comparing objects by reference and comparing it with object that _looks_ the same in terms of its value, but has a difference reference in memory.

## toBe

So far, we're only used `.toBe`, which checks for strict equality (e.g. `===` or `Object.is`).

This works like we expect for simple types:

```ts
test('strings should be strictly equal', () => {
	expect('string').toBe('string');
});

test('numbers should be strictly equal', () => {
	expect(2).toBe(2);
});

test('booleans should be strictly equal', () => {
	expect(true).toBe(true);

	expect(false).toBe(false);
});

test('undefined should be strictly equal to itself', () => {
	expect(undefined).toBe(undefined);
});

test('null should be strictly equal to itself', () => {
	expect(null).toBe(null);
});

test('BigInts should be strickly equal', () => {
	expect(BigInt(Number.MAX_SAFE_INTEGER)).toBe(BigInt(Number.MAX_SAFE_INTEGER));
});
```

But, things get a little trickier when comparing objects (arrays _and_ functions are objects in JavaScript).

```ts
describe('toBe', () => {
	test.fails('objects should not be strictly equal', () => {
		expect({ a: 1 }).toBe({ a: 1 });
	});

	test.fails('arrays should be strictly equal', () => {
		expect([1, 2, 3]).toBe([1, 2, 3]);
	});

	test.fails('functions should to be strictly equal', () => {
		expect(() => {}).toBe(() => {});
	});
});
```

## toEqual

Instead, when we're comparing two objects that are not referentially equal, we can user `toEqual`. I'm just going to [quote the documentation](https://vitest.dev/api/expect.html#toequal) for a hot minute:

> `toEqual` asserts if actual value is equal to received one or has the same structure, if it is an object (compares them recursively).

Looking further down in `examples/great-expectations/objects.test.ts`, you'll see the following:

```ts
describe('toEqual', () => {
	test('similar objects should pass with #toEqual', () => {
		expect({ a: 1 }).toEqual({ a: 1 });
	});

	test('similar nested objects should pass with #toEqual', () => {
		expect({ a: 1, b: { c: 2 } }).toEqual({ a: 1, b: { c: 2 } });
	});

	test('similar arrays should pass with #toEqual', () => {
		expect([1, 2, 3]).toEqual([1, 2, 3]);
	});

	test('similar multi-dimensional arrays should pass with #toEqual', () => {
		expect([1, [2, 3]]).toEqual([1, [2, 3]]);
	});

	test('functions should to be strictly equal if compared by reference', () => {
		const fn = () => {};

		expect(fn).toBe(fn);
	});
});
```

Here are some assertions that you _may_ want to consider:

- [`toBe`](https://vitest.dev/api/expect.html#tobe)
- [`toBeCloseTo`](https://vitest.dev/api/expect.html#tobecloseto)
- [`toBeInstanceOf`](https://vitest.dev/api/expect.html#tobeinstanceof)
- [`toBeUndefined`](https://vitest.dev/api/expect.html#tobeundefined)
- [`toContain`](https://vitest.dev/api/expect.html#tocontain)
- [`toThrow`](https://vitest.dev/api/expect.html#tothrow)
- [`toThrowError`](https://vitest.dev/api/expect.html#tothrowerror)

You can see the full API for `expect` here:

- [Vitest's Expect API](https://vitest.dev/api/expect.html).
- [Jest's Expect API](https://jestjs.io/docs/expect)
