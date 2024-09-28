---
title: Resetting VITE_ENV With Vitest
description: Learn how to reset VITE_ENV in Vitest using beforeEach and vi.stubEnv.
modified: 2024-09-28T18:32:11.149Z
---

```ts
import { beforeEach, expect, it } from 'vitest';

// you can reset it in beforeEach hook manually
const originalViteEnv = import.meta.env.VITE_ENV;

beforeEach(() => {
	import.meta.env.VITE_ENV = originalViteEnv;
});

it('changes value', () => {
	import.meta.env.VITE_ENV = 'staging';
	expect(import.meta.env.VITE_ENV).toBe('staging');
});
```

Here is a better way.

```ts
import { expect, it, vi } from 'vitest';

// before running tests "VITE_ENV" is "test"
import.meta.env.VITE_ENV === 'test';

it('changes value', () => {
	vi.stubEnv('VITE_ENV', 'staging');
	expect(import.meta.env.VITE_ENV).toBe('staging');
});

it('the value is restored before running an other test', () => {
	expect(import.meta.env.VITE_ENV).toBe('test');
});
```

```ts
```
