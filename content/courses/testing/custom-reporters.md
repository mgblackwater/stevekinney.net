---
title: Vitest Custom Reporters
description: Learn how to create and implement custom reporters in Vitest.
modified: 2024-09-28T11:31:16-06:00
---

## 1. Vitest Custom Reporters

Alright, you’ve got your tests running with Vitest, and everything looks shiny… except maybe the default output doesn’t scratch that developer itch, right? Maybe you’ve got a VIP watching over your CI pipeline who's like, "It's fine, but I wish it looked *cooler*."

Or maybe you're staring at your terminal and thinking, "Could this have more info—OR, could it bless me with *less* info?" Time to roll up your sleeves and whip up a **custom reporter**.

Vitest lets you create custom reporters to control what gets spit out in your command line. Let’s walk through crafting a custom reporter that’ll make your tests feel more at home, whether that’s jazzed up or minimalistic.

## 2. Why Custom Reporters Matter

Imagine this: you’ve got a CI pipeline breaking *occasionally* (ugh), and you need to get to the root cause ASAP. The default reporter's output is okay, but what if you could give it just a *bit* more context? Custom reporters allow you to surface specific information that’s helpful in your scenario—whether that’s cleaner logs, specific metrics, or a creative touch that livens up your test reports.

Also useful for:

- Making CEOs and managers feel *something* when they see test results.
- Addressing special requirements for audit or compliance reasons.
- Controlling the noise level—sometimes you just want the deets, not the fanfare.

## 3. Setting Up a Custom Reporter

Great, the “why” is clear. Now let’s get our hands dirty and write one.

Vitest gives you a hook into the test runner through a **Reporter API**. The basic structure involves creating a class with methods that correspond to certain test events—like when a test starts, finishes, or (heaven help us) fails.

Let’s craft a simple custom reporter that says, "Hello, testing world," whenever a test starts.

```js
// hello-world-reporter.js
export class HelloWorldReporter {
	onTestStart(test) {
		console.log(`Hello from ${test.name}`);
	}

	// Optional: here's where all your reporting dreams can come true
	onTestPass(test) {
		console.log(`🎉 Hooray! Test passed: ${test.name}`);
	}

	onTestFail(test) {
		console.log(`💥 Oh no! Test failed: ${test.name}`);
	}
}
```

## 4. Hooking It Into Vitest

Okay, we’ve got the *HelloWorldReporter* locked and loaded. Now we just need to tell Vitest to use it.

Here’s how you connect it in your `vitest.config.js` file.

```js
import { defineConfig } from 'vitest/config';
import { HelloWorldReporter } from './hello-world-reporter';

export default defineConfig({
	test: {
		reporters: [new HelloWorldReporter()],
	},
});
```

In this case, `reporters` expects an array. You could add multiple reporters to combine custom ones with Vitest’s built-ins (like `['default', new HelloWorldReporter()]`). It’s basically like building a superhero team of test output handlers.

## 5. Running Your Tests with the Custom Reporter

Boom! That’s it. Now, when you run `vitest`, you’ll start seeing your custom messages come through.

```bash
$ vitest

 Hello from Sample Test
 🎉 Hooray! Test passed: Sample Test
```

If a test fails? Expect some fireworks:

```bash
 💥 Oh no! Test failed: Crazy Edge Case
```

## 6. Where to Take This Next

Custom reporters vary *wildly* depending on what you’re trying to achieve. Maybe you want to generate a file with summary stats, spit out a more compact report for quicker reads, or build a visual dashboard from the results.

You can go as simple or complex as you need. Heck, you could even pipe your results into a `Slack` webhook or have them broadcast in Morse code over the office speakers (not that I’d recommend it, but *hey*, it's your world).

Key things you can hook into:

- `onTestStart(test)` – The moment a test begins
- `onTestPass(test)` – When a test sails through
- `onTestFail(test)` – Your test hits the *failure iceberg*
- `onRunComplete(testResults)` – Everything’s done and dusted

## 7. Wrapping Up

Take some power back and own what your test output looks like. Even though Vitest‘s “default” reporting is decent, what’s acceptable when you’re midway through a sprint at 3 AM? **Whatever feels right to you**. Custom reporters give you that freedom.

Now, go forth—build custom experiences for your test output that’ll make your future self want to shake your hand or throw a party when all those green checkmarks roll in.
