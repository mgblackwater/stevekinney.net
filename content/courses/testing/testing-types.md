```js
import { defineConfig } from 'vitest';

export default defineConfig({
	test: {
		typecheck: {
			enabled: true,
		},
	},
});
```

A type test might look something like this:

```ts
import { expect, expectTypeOf, test } from 'vitest';

test('type', () => {
	expectTypeOf(1).toEqualTypeOf(2);
	expect(1).toBe(2); // not executed
});
```
