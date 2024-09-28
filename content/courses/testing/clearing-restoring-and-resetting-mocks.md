# Clearing, Restoring, and Resetting Mocks in Vitest

Ah, mock functions. They’re the unsung heroes when it comes to testing. But, here's the deal: once you’ve mocked something, you want to make sure that those mocks don’t stick around like a bad code smell. That's why **clearing**, **restoring**, and **resetting** mocks are vital parts of your testing toolkit. So, let’s break it down like refactoring a confusing callback hell into promise chains.

## Mocking in Vitest (Quick Refresher)

First, let’s revisit what mocking is all about—because nobody wants to be confused by lingo while debugging at 2 a.m.

Mocks allow you to replace real implementations of functions or modules with fake ones during your tests. This is incredibly helpful when you want to:

- Stop your tests from making real network requests (nobody wants to charge a credit card in a unit test, right?),
- Control logic that’s outside your function (like what happens in setTimeout or our old friend, Math.random),
- And verify that specific functions were called, with the right arguments.

Okay, now that we’ve got that covered, let's move on to the cleanup needed after all our mockery. In Vitest, we have three primary ways to handle mocks post-shenanigans: clearing, resetting, and restoring.

---

## Clearing Mocks

Clearing a mock removes all _invocation history_. Let's say you've got a mock that's been called a few times, and you want to reset the call count back to zero. That’s _clearing_ in action.

```js
const myFunc = vi.fn();

// Call it a few times
myFunc('arg1');
myFunc('arg2');

// Clear the mock's call history
myFunc.mockClear();

console.log(myFunc.mock.calls.length); // Ah, nice! Back to 0.
```

Use `mockClear()` when you want to ensure that your tests are checking fresh invocation data. This way, you’re not being haunted by any ghost calls from previous tests. Common use case? When you're running the same mock across multiple assertions but want a clean slate for each.

> **Pro tip:** If your tests rely on the **number of times** a function is called, reset that call count before each test—so you're always getting fresh numbers.

---

## Resetting Mocks

While **clearing** wipes out call history, **resetting** is like hitting the factory reset button on your microwave—it clears not only the previous calls but also any custom mock implementations.

```js
const myFunc = vi.fn(() => 'I got mocked');

// Call the function
console.log(myFunc()); // “I got mocked”

// Reset the mock to its initial state
myFunc.mockReset();

// Now, it's just an empty mock function again
console.log(myFunc()); // undefined
```

When should you reset? Think of it as hitting _undo_. If you’ve changed what the mock returns, and you’re like, "Nope, never should've done that,” then this is your ticket to sanity.

> **Heads-up:** With reset, you'll still _have_ the mock object, but it behaves like it did out of the box—call count set to `0`, and no return values or side effects.

---

## Restoring Mocks

Now, here's the nuclear option: **restoring** mocks. This completely removes the mock's implementation and puts things back the way they were before you even touched it. If you’re mocking a native function or something from a shared library, this is how you avoid accidentally breaking things later.

```js
// Let's mock Math.random
vi.spyOn(Math, 'random').mockReturnValue(0.5);

console.log(Math.random()); // 0.5

// Restore the original implementation
Math.random.mockRestore();

console.log(Math.random()); // Now we're back to good ol' randomness
```

When you restore a mock, it brings the original, un-mocked function back. This is super useful, especially when you're messing with native APIs—because, trust me, leaving `Math.random()` mocked throughout your app is guaranteed to produce test nightmares later.

---

## When To Use Each

Alright, quick recap:

- **Clear**: You’ve created some complex mock logic, and now you're retracing steps, clearing call history to test cleanly.
- **Reset**: You made a mess with return values or .mockImplementation—and now you just want to start over without rebuilding the mock.
- **Restore**: You’re done mocking, you want to reinstate the original functionality, and walk away like nothing ever happened.

---

## Conclusion

Vitest gives you the tools you need to keep your tests clean, your mocks simple, and your frustrations (mostly) in check. The key is knowing when to clean up after yourself. Mocks can be powerful, but if you don’t clear, reset, or restore them properly, you’ll end up wondering why things are on fire and you're dealing with mocking baggage from previous tests.

Happy testing! And may you _always_ remember to restore your mocks before they haunt another test file. 🌕👻
