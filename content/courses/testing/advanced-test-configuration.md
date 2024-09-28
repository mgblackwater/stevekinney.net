---
title: Advanced Test Configuration With Vitest
description: Dive into advanced Vitest configurations for better efficiency.
modified: 2024-09-28T11:31:42-06:00
---

Okay, so you’ve been writing tests, everything runs smoothly, you feel like a testing ninja… and then BAM! You hit this point where **running one test at a time** just isn't cutting it. Or maybe, you’ve got some tests that behave a little… _flaky_. Or even worse, some parts of your app pull in external services that aren’t behaving. Ugh. Don’t worry, we’ve been there too.

In this tutorial, we're going to dive into some more advanced Vitest configurations that’ll make you feel like you're back _in control_. Maybe you've got a large codebase or complex dependencies—don’t freak out! Let’s power up and go deeper.

## Customizing the `vitest.config.ts`

Look, Vitest is awesome right out of the box, but sometimes you need to customize things to fit your project. This brings us to the `vitest.config.ts` file. Here’s a basic one as a refresher:

```ts
// vitest.config.ts
import { defineConfig } from 'vitest/config';

export default defineConfig({
	test: {
		globals: true,
		environment: 'jsdom',
		coverage: {
			reporter: ['text', 'json', 'html'],
		},
	},
});
```

This gives you a decent setup—global variables available across all tests, using `jsdom` (hello virtual DOM testing!)—but Vitest's true power shines through when you start to flex this config.

### 2.1 Setting up Test Aliases

Let’s knock out something simple but powerful: **path aliases**. If you're deep into a project, your imports are probably starting to look like a tangled mess of relative paths.

```ts
import MyComponent from '../../../../../component/MyComponent';
```

_Yikes_, right? Time to leverage Vitest's configurability. You can set up path aliases to tidy things up.

```ts
// vitest.config.ts
import { defineConfig } from 'vitest/config';
import path from 'path';

export default defineConfig({
	resolve: {
		alias: {
			'@components': path.resolve(__dirname, 'src/components'),
			'@utils': path.resolve(__dirname, 'src/utils'),
		},
	},
});
```

Now in your tests, it’s much cleaner:

```ts
import MyComponent from '@components/MyComponent'; // Phew.
```

Neat and readable. Plus, we’re already one step closer to a maintainable codebase.

### 2.2 Handling ESM and CommonJS Together

Ah yes, the modern mess of JavaScript modules. You're probably encountering both in your project, and it can get nasty when you try to test them. Sometimes, you need to treat CommonJS and ESM _differently_. No problem—Vitest’s got you covered.

```ts
export default defineConfig({
	test: {
		deps: {
			fallbackCJS: true, // Handle CommonJS dependencies that break ESM resolution
		},
	},
});
```

This little addition can save you headaches when testing packages that work with `require()` while your code lives in the glorious land of `import`.

### 2.3 Isolating Test Environments

Here's a quick scenario. Let’s say you're testing a **Node.js app**. You’re using actual **file reads**, and boy is it sloooow. But you know about mocking, right? But wait! Before you even get into mocking (which is another can of worms), Vitest lets you run each test file in complete **isolation**—sort of like a mini-reset between each run.

```ts
// vitest.config.ts
export default defineConfig({
	test: {
		isolate: true, // Ensure each test file runs in its own VM context
	},
});
```

This turns each test file into its own bubble. That means if you're changing `global variables`, messing with servers, or tweaking API states, Vitest will restore the peace and clean up the neighborhood automatically after each file.

## 3. Test Timeouts and Reruns

So, what happens when one of your tests drags on, like… for-ev-er. Maybe it depends on network latency or takes a second to spin up resources. You don’t want your test suite hanging just because one test feels a little lazy, right? Vitest lets you set **global timeouts**, but you can configure them on a _test-by-test_ basis too. Get ready to save precious minutes of your life:

### 3.1 Global Timeouts

```ts
export default defineConfig({
	test: {
		testTimeout: 5000, // 5 seconds, if it takes longer, something is wrong.
	},
});
```

That’s right, 5000 milliseconds. More than enough time for most tests. If anything runs longer than that, Vitest will scream at you, and maybe it’s time to investigate **why**.

### 3.2 Test Rerun: Flaky Test Insurance

Speaking of flaky! That occasional test that just decides it wants some attention by failing randomly? (Yeah, we ALL have that ONE test.) Let’s tell Vitest to rerun it automatically a few times before calling it a failure:

```ts
export default defineConfig({
	test: {
		retry: 2, // Will rerun a failing test 2 times before marking it as failed
	},
});
```

Boom—no more "it failed on CI but passed locally" headbanging. But hey, if it's failing more than twice, it's time for **you** to shine your detective cap.

## 4. Fine-Tuning Watch Mode

Here's what usually happens after you start integrating **watch mode** into your life: you make a tiny code change, Vitest detects it, runs hundreds of tests, and suddenly your CPU is on fire. 🔥 Yeah, running everything might be overkill. Luckily, we can refine which tests are run on each watch cycle for better speed.

### 4.1 Only Run Changed Tests

```ts
export default defineConfig({
	test: {
		watch: {
			include: ['src/**'],
			exclude: ['node_modules/**', '*.spec.js'], // avoid triggering for these
		},
	},
});
```

This way, you're far more selective during your test runs—letting you focus only on the changes that matter.

## 5. Conclusion

Well, if you’ve hung with me til now, you’re _fully equipped_. We’ve touched on extending Vitest’s test configuration, smoothing out rough edges, and speeding up your development experience with more intelligent settings.

The goal here isn’t just about writing squeaky-clean tests—**it’s about staying sane** while doing it. Take these tips as your advanced toolbox to wield Vitest the next time complexity strikes. Headaches be gone.

Happy testing, my friend. Test smart, and test often! 🎉

```ts

```
