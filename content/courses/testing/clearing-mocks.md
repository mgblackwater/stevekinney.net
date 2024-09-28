---
modified: 2024-09-17T11:27:06-06:00
---

Generally speaking, you want to put stuff back the way you found it in order to make sure that you have good test isolation things don't get weird when tests have long-lasting side effects that cause other tests to fail for no particularly good reason.

## Object Methods

For a moment, let's assume `const fn = vi.fn()`.

- `fn.mockClear()`: Clears out all of the information about how it was called and what it returned. This is effectively the same as setting `fn.mock.calls` and `fn.mock.results` back to empty arrays.
- `fn.mockReset()`: In addition to doing what `fn.mockClear()`, this method replaces the inner implementation with an empty function.
- `fn.mockRestore()`: In addition to doing what `fn.mockReset()` does, it replaces the implementation with the original functions.

## Mock Lifecycle Methods

You'd typically put these in an `afterEach` block within your test suite.

- `vi.clearAllMocks`: Clears out the history of calls and return values on the spies, but does _not_ reset them to their default implementation. This is effectively the same as calling `.mockClear()` on each and every spy.
- `vi.resetAllMocks`: Calls `.mockReset()` on all the spies. It will replace any mock implementations with an empty function.
- `vi.restoreAllMocks`: Calls `.mockRestore()` on each and every mock. This one returns the world to it's original state.
