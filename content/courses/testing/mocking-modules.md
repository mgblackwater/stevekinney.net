---
title: Understanding Vitest Module Mocking
description: Learn how to mock modules in Vitest using vi.mock and vi.doMock.
modified: 2024-09-28T11:31:15-06:00
---

> \[!warning] A Word on Module Systems
> `vi.mock` works only for modules that were imported with the `import` keyword. It doesn't work with `require`.

Vitest provides `vi.mock`, which allows you to mock any import that you provide a path for. It's got the following signature:

```ts
(path: string, factory?: () => unknown) => void
```

The `factory` is a function that you provide as a substitute for whatever *really* resides at the file path. You'll see that it's optional. Here is how it goes down:

1. If you provided a `factory` function, it will use the return value of that function as the replacement for whatever module lives at `path`.
2. If you don't provide a factory, but you do have a `__mocks__ ` directory at the same location and an alternative file in that `__mocks__ ` directory, then it will substitute that in.
3. Vitest will use its automocking algorithm.

## Auto-Mocking Algorithm

If you don't provide a factory, Vitest will employ its [automocking algorithm](https://vitest.dev/guide/mocking.html#automocking-algorithm):

- All arrays will be emptied.
- All primitives and collections will stay the same.
- All objects will be deeply cloned.
- All instances of classes and their prototypes will be deeply cloned.

## `vi.doMock`

[`vi.doMock`](https://vitest.dev/api/vi.html#vi-domock) is basically the same as `vi.mock` except for the fact that it's *not* hoisted to the top, which means you have access to variables. The *next* import of that module will be mocked.
