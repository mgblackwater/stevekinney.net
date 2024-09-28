---
title: Debugging Vitest Tests
description: Learn essential tools for debugging tests in Vitest effectively.
modified: 2024-09-28T11:31:16-06:00
---

## Debugging Vitest Tests

Alright, you've just written some tests in Vitest, hit `npm run test`, and oof—something's broken. But now the real fun begins: _debugging_. It's the mix of detective work and head-scratching developers sign up for when they add tests to their app. Don’t worry, though. We’re going to walk through the essential tools Vitest offers for debugging, and I promise, it's not as scary as figuring out why your CSS grid isn't lining up.

### Error Messages in Vitest: Your First Clue

Before we dive into fancy debugging tools, let’s talk about reading Vitest’s error messages. That might seem basic, but it’s huge. You run the test, and it fails. Vitest is _really_ good at telling you _what_ went wrong, which line, and, often, why. So when you see the red "FAIL," slow down and carefully read through the stack trace.

Let’s say you have this failing test:

```javascript
import { expect, test } from 'vitest';
import { addNumbers } from './addNumbers';

test('adds two numbers', () => {
	expect(addNumbers(2, 2)).toBe(5);
});
```

Vitest tells you exactly where it went sideways:

```ts
FAIL  src/addNumbers.test.js > adds two numbers
      Expected: 5
      Received: 4
```

Look! The math went wrong. Sometimes, tests fail for simple reasons—a typo, wrong arguments, or a function behaving badly. Fix the implementation and move on.

But other times, you need to level up your debugging game.

### Running Only One Test (You’d Be Crazy to Skip This)

When you’ve got a gnarly error and, say, 135 tests awaiting execution, there’s absolutely no reason to run everything. That’s pure chaos. Focus on **one** test at a time. Vitest to the rescue with `.only`.

```javascript
test.only('adds two numbers', () => {
	expect(addNumbers(2, 2)).toBe(5);
});
```

Boom—Vitest just runs that single test. Now, _all_ your attention is on the suspect. Narrow it down and debug faster.

### Enable Watch Mode (A Developer’s Best Friend)

Ever run a test, tweak some code, and then realize you need to run it again… manually… like a caveman? With **watch mode**, you can let Vitest automatically re-run tests as you update your code.

```bash
npx vitest --watch
```

Now you're living in the future. Every time you save your file, Vitest will kick off your tests again. You'll find yourself in a quick cycle of fix-run-fix until the test works (or you rage quit—whatever comes first).

### Using `console.log` (Yes, It's Still Okay!)

Okay, if we’re being real here, most debugging is just sprucing up the joint with `console.log`. Sometimes, the fastest way to figure out what’s going wrong is to pepper _a few `console.log`s into your code_. If you're adding a million log statements to your actual test files—hey, no judgment—we all do it:

```javascript
test('adds two numbers', () => {
	const result = addNumbers(2, 2);
	console.log('The result is:', result);
	expect(result).toBe(5);
});
```

This quick logging gives you an immediate sense of what’s going on inside your functions. Stack that up with **watch mode**, and you’ll be fixing bugs in no time.

### Using Vitest’s `inspect` Mode for Debugging in Chrome DevTools

Here’s the fancy trick everyone loves. Vitest has an **inspect mode** that allows you to debug with Chrome’s DevTools if `console.log` isn’t cutting it. You know, like you do when your frontend throws a giant tantrum in production. You can even add breakpoints and step through your tests.

To start in inspect mode:

```bash
npx vitest --inspect
```

This will spit out a URL like:

```ts
Debugger listening on ws://127.0.0.1:9229/xxxxx
```

Paste that in Chrome, open DevTools (`Cmd+Opt+I` or F12), and now you can set breakpoints, stalk variables, and generally live the debug life in style. No more pretending you're beyond `console.log`!

### Bonus Tip: Use `.skip()` to Route Around the Problem

If you’re in the weeds and need to move forward without deleting a test (don’t delete the test!), you can tell Vitest to _skip_ specific tests while you focus on something less painful.

```javascript
test.skip('complicated stuff I’ll deal with later', () => {
	expect(complexLogic()).toBe(true);
});
```

You’ll still see them in your test report, but they’ll be marked as "skipped" so you can return once you’ve got your sanity back.

### Conclusion: Debugging Like a Boss

Remember, debugging is less about fancy tools and more about whittling down the suspects. Use `.only` to isolate a test, leverage `console.log` like it’s going out of style, and when you need the big guns, flip to inspect mode and debug through Chrome DevTools.

And, hey, if things still aren’t working? Grab your favorite caffeine source, stretch a bit, and go back to reading those error messages. Tests are your friend, even when it feels like they're out to get you.

```ts

```
