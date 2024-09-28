---
title: Type Testing With Vitest
description: Example configurations for type testing using Vitest.
modified: 2024-09-28T18:32:11.058Z
---

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

```ts
```
