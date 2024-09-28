# Mocks vs Stubs vs Spies in Testing

Alright, let's set the scene. You're writing some tests for your app, and you've got this gnarly dependency that does _stuff_—maybe it hits an API, maybe it reads a file, maybe it calls some complex function deep in your codebase’s dark corner. You don't wanna invoke this thing every time you run your test suite. That's a one-way ticket to Flaky Test City, and no one wants to live there.

This is where **mocks**, **stubs**, and **spies** come in. These are your test BFFs for controlling and observing how your dependencies behave without actually, you know, depending on them.

Let’s break 'em down simply, because we don’t have time for fancy textbook definitions—only results.

## Stubs

Stub is the stand-in. It's like the understudy in your school play, except you’re making them return _exactly_ the value you want. You control what the method or function returns when called, regardless of what it should do during the actual runtime of your app.

### Real Talk

Think of a stub as, “I don’t care what this would normally return—just give me X so I can focus on testing Y.”

### Example Time:

```javascript
import { vi } from 'vitest';

const dependency = {
	getData: () => 'real data from server', // Pretend this does something real expensive.
};

test('should handle data correctly', () => {
	const stub = vi.spyOn(dependency, 'getData').mockReturnValue('stubbed data');

	// Your actual test logic
	const result = dependency.getData();
	expect(result).toBe('stubbed data');

	stub.mockRestore(); // Clean up after yourself
});
```

In this example, you're replacing the real `getData` with your fake, predictable value: `'stubbed data'`. Your test isn't dependent on whether the server’s up or not, and you’re in control.

## Mocks

Mocks are like that friend who says, "Hey, I'll not only stand in for you, but also pretend I did the whole thing.” It goes a step beyond stubs. With mocks, we not only control function return values (_like_ a stub), but we can also make sure it was called with specific arguments, a specific number of times, or… _however we want_.

### Example Time:

```javascript
import { vi } from 'vitest';

const dependency = {
	getData: () => 'real data from server',
};

test('should call getData once', () => {
	const mock = vi.spyOn(dependency, 'getData').mockReturnValue('mocked data');

	// Act
	dependency.getData();

	// Assert
	expect(mock).toHaveBeenCalledTimes(1); // Ensure it got called exactly once
	expect(mock).toHaveBeenCalledWith(); // Maybe also care about arguments?

	mock.mockRestore(); // Same as with stubs—clean up after tests
});
```

Notice how we _mocked_ the `getData` method, asserting it was called exactly once. Here, mocks give you behavior control _and_ spy-like superpowers (we'll get to spies next, don't worry).

## Spies

Ahh, spies. Spies are sneaky little things. They poke their heads in and observe while pretending nothing changed. They don’t alter the behavior of functions—they just let you peek behind the curtain and see how many times it was called, with which arguments, etc. They’re perfect for when you care less about _what_ it returns and more about _how_ it’s used.

### Example Time:

```javascript
import { vi } from 'vitest';

const dependency = {
	getData: () => 'real data from server',
};

test('should spy on getData and check its behavior', () => {
	const spy = vi.spyOn(dependency, 'getData');

	// Act
	dependency.getData();

	// Assert
	expect(spy).toHaveBeenCalledTimes(1); // Watch getData in action
	expect(spy).toHaveBeenCalledWith(); // Check what it’s been called with

	spy.mockRestore(); // Gotta restore it back to its original self
});
```

In this case, we didn’t mess with _what_ `getData` returns at all. We just watched, like a good spy, how it was used.

## Quick Recap (For the "Long Story Short" Peeps)

1. **Stub**: You control the output of a function (force it to return whatever you want) and move on.
2. **Mock**: Like a stub, but also gives you the ability to verify if a function was called, and how. So it's both a stand-in _and_ a checker.
3. **Spy**: Doesn't mess with the function's behavior, just watches it. No judgement, just gathering data.

## When To Use What

- **Use a Stub** when you just need specific return values and don’t care how many times it's called.
- **Use a Mock** when you want both to control behavior and monitor usage (like, "Did my method call this other method? How many times? Did it pass the right args?").
- **Use a Spy** when you trust the function to work but just want to know how it’s used (no behavior changes, just a bystander).

## Final Thoughts

The real deal here is control and verification. You don't have to burn time testing a bunch of outside dependencies or worrying about inconsistent results creeping into your tests. Whether you’re doing a stub, mock, or spy, you get to keep things reliable. Plus, it saves precious test run time and developer sanity. 🧠💡

Start experimenting with stubs, mocks, and spies in your next project, and you’ll see the headaches _disappear_. Well, at least the test-related ones...
