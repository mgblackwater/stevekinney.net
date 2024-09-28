---
title: "Reporters : Getting The Dirt On Your Tests"
description: Learn how to use Vitest reporters for tailored test output.
modified: 2024-09-28T18:32:10.839Z
---

## Reporters in Vitest: Getting the Dirt on Your Tests

Alright, let’s talk about one of the lesser-appreciated, but totally cool things in Vitest: **Reporters**.

Now, when you're running your tests, Vitest is like that friend who has no problem telling you exactly what’s happening—who passed, who didn’t, and all the details in between. But, Vitest also understands that not everyone likes to get that feedback in the exact same way. Enter **reporters**. Reporters are basically the "output formatters" of your test results. They control what information gets displayed when you run your tests and how it’s presented, like a news anchor reading out your test’s latest hits and misses.

Maybe you like clean, slick outputs. Maybe you’re an over-the-top text junkie who wants every little detail. Or, perhaps you need your test results in a format that some automated tool or CI system can ingest. Either way, Vitest has a feature just for you.

### Default Reporter

By default, Vitest goes with a pretty simple format in your terminal. It's essentially the *classic newsroom feel*: you get a list of all test suites, along with which tests passed or failed.

Let’s simplify this with a quick setup and run:

#### Install Vitest (if You Haven't already):

```bash
npm install vitest --save-dev
```

#### Create a Simple Test File (`example.test.js`):

```js
import { expect, test } from 'vitest';

test('math works', () => {
	expect(1 + 1).toBe(2);
});

test('this one will fail', () => {
	expect(2 + 2).toBe(5);
});
```

#### Run Your Tests:

```bash
npx vitest
```

Here’s the output from your terminal:

```ts
 PASS  example.test.js > math works
 FAIL  example.test.js > this one will fail
     expect(received).toBe(expected) // Object.is equality

     Expected: 5
     Received: 4

   ● example.test.js:6:20

Test Files  1 failed | 1 passed
```

Simple and to the point, right? But sometimes we want **more** or **less**.

### Built-in Reporters

Vitest comes with a bunch of built-in reporters like your standard "news anchors," each with a different style of reporting the testing situation. Here are some of the most commonly used ones:

#### Dot Reporter

The **dot** reporter is for people who think the terminal’s main job is to stay as quiet and non-verbose as possible. It gives you a dot for every test, and a failure means the dot will turn red. Think of it like the minimalist approach to testing:

```bash
npx vitest --reporter dot
```

And here’s what it'll output:

```ts
  ..F
```

Translation: Two tests passed (.), and one failed (F). Short, sweet, and ready for your brain to decode it.

#### Verbose Reporter

Maybe you're more of a *give-me-everything* kinda person. If you want your test output to feel like a full log of basically every tiny thing going on, the **verbose** reporter is your jam.

```bash
npx vitest --reporter verbose
```

With this, you’ll get a blow-by-blow account including the test suites, individual tests, and detailed results. Verbose is like the sports commentator that describes the entire game in full detail.

#### JSON Reporter

Sometimes, we’re not even interested in human-readable formats. Who needs it, right? Maybe you're plotting to feed your test results into some external dashboard or CI/CD pipeline as part of an automated process. The **json** reporter will output your test results in a JSON object:

```bash
npx vitest --reporter json
```

Here’s a simplified version of the output:

```json
{
	"numFailedTests": 1,
	"numPassedTests": 1,
	"testResults": [
		{
			"name": "math works",
			"status": "passed"
		},
		{
			"name": "this one will fail",
			"status": "failed"
		}
	]
}
```

#### Tap Reporter

For old-school engineers or teams using certain CI systems, the **tap** format (Test Anything Protocol) is a time-tested tool you'll recognize. This spits out the results in a format which is designed for other systems to process:

```bash
npx vitest --reporter tap
```

You’ll get output like this:

```ts
TAP version 13
1..2
ok 1 math works
not ok 2 this one will fail
```

Yep, it's that minimal and direct. It’s all about the machines, man.

### Combining Reporters

What if you want the best of both worlds? Say you want the **dot** reporter for your command-line scanning, and the **json** reporter to send results to CI?

Good news, you can totally combine them together, like ketchup and mustard. Just pass them both:

```bash
npx vitest --reporter dot --reporter json
```

Boom. Now you’ve got dots in your terminal and JSON data flying wherever you want it!

### Custom Reporters

Listen, maybe you’re someone who's never satisfied with the built-ins that come with a tool. I get that. You like **control**, you like **tweaks**. Can’t leave anything factory-made. Well, Vitest gives you the power to **write your own reporter**.

You can create a custom reporter by tapping into Vitest’s API, aiming it to collect exactly the test data you’re hungry for and presenting it *just the way you like*.

Here’s a simple example of a custom reporter:

```js
class CustomReporter {
	onInit(ctx) {
		console.log('Starting tests!');
	}

	onTestFinished(test) {
		console.log(`${test.name} finished! Status: ${test.result?.state}`);
	}

	onFinished(results) {
		console.log(`All tests done. Passed: ${results.passed}, Failed: ${results.failed}`);
	}
}

export default CustomReporter;
```

Run it by passing it as a reporter:

```bash
npx vitest --reporter ./path/to/CustomReporter.js
```

Now you’ve got a fully customized reporting system. Go ahead—tinker with it until you create the test output experience of your dreams!

### Wrapping Up

So that’s the deal with Vitest reporters. They’re simple but **powerful** tools for how you organize and view your testing results. Whether you’re a **dot minimalist**, a **verbose maximalist**, or just someone who craves more **JSON in their life**, Vitest has it all and can be tailored exactly to how you (or your CI/CD environment) like to work.

And next time you’re stuck hunting a bug at 2 AM and questioning all your life choices, at least your test suite will be spitting out exactly the results you need—**in the format of your choosing.** Stick around for that satisfaction.

```ts
```
