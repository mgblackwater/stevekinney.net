---
title: "Browser Mode : Testing Like It's 2023"
description: Learn how to set up Vitest for testing in a browser environment.
modified: 2024-09-28T18:32:10.907Z
---

## Browser Mode in Vitest: Testing Like It's 2023

Alright folks, we know Vitest does an incredible job making testing in Node environments a breeze. You write tests, Vitest runs 'em, you look brilliant at your next code review—simple, right? Well, things were going great until the day came where I had a **browser-specific bug** blow up my week. Testing the logic wasn’t enough anymore; I had to test it in actual browser conditions. "Cool, easy," I said (lying). Spoiler: Testing in browsers used to be annoying, and I'm probably being gentle there.

But here’s where **Vitest Browser Mode** rides in like a hero. You can now run your Vitest tests *in an actual browser* but without the headache of setting up a complete Selenium nightmare or manually copying code into devtools. Let’s talk about how to get set up, how it works, and most importantly—how to save yourself several hours of hair-pulling.

### Why Browser Mode?

There are certain situations where tests in Node just won't cut it. Some APIs, like the DOM or local storage, are only available in browser environments. Or, sometimes, you just want to be absolutely sure your code behaves in an actual browser environment—maybe you're curious what happens when some absolutely ancient copy of Chrome hits your beautiful new feature.

Vitest gives you a way to write tests for that code, but in a **real browser environment**. The good news? You already know how the tests themselves come together with Vitest—it looks almost exactly the same as your Node-based tests.

### How to Set It Up

Let’s make browser mode work in your project because, really, the faster we’re running these tests in a browser, the less likely we are to break production code on build day. Open up that terminal and let's get started.

#### Step 1: Install Vitest

If you're not already using Vitest (first off, rude), it’s super easy to install:

```bash
npm install vitest --save-dev
```

You’ll also need to install **jsdom** if it's a pure DOM test environment:

```bash
npm install jsdom --save-dev
```

This sets you up to run your regular ol’ tests. But hey, what if you want to flip that switch for real browser testing?

#### Step 2: Configure Vitest for Browser Mode

To make Vitest aware that you want to run in *browser mode*, you just need to tweak the config a bit. If you haven't set up a `vitest.config.js` file—it’s time:

```javascript
// vitest.config.js
export default {
	test: {
		environment: 'jsdom', // Options: 'node', 'jsdom'
		browser: true, // THIS is what tells Vitest: "Yo, we're in the browser now."
	},
};
```

In this case, `jsdom` is our simulated DOM environment. Sure, it’s still Node, but now it acts like a browser with standard DOM APIs (aka the stuff you’re actually testing). If you’re running tests that rely on, say, `document` or `window`, or if you need to test local storage, cookies, or a dozen other front-end things—the browser just works better for these.

#### Step 3: Running the Tests

Okay, you wrote your test. Now the moment of truth. Here’s how you actually kick things off in browser mode:

```bash
npx vitest --mode=browser
```

Boom. The tests will now actually be run in a browser environment! Legit DOM, real window objects—we’re in business.

### What’s Next? Writing Some Tests

Write some simple browser-based tests—maybe something that’s not complex but that a browser should handle differently than Node. Let’s check it out.

#### Example: Testing Something DOM-ey

Here’s a quick-and-dirty example. Let’s say you want to ensure that grabbing an element from the DOM works properly:

```javascript
test('should find an element by ID', () => {
	document.body.innerHTML = `<div id="myElement"></div>`;

	const el = document.getElementById('myElement');

	expect(el).not.toBeNull();
});
```

Look at that beauty. Try running it in Node—guess what? It’ll fail—Node doesn’t know from DOM APIs. Switch to browser mode, and pow! It works, no problem.

Maybe you’re testing some other fun front-end interaction:

```javascript
test('should properly assign to localStorage', () => {
	localStorage.setItem('bestFramework', 'Vitest');

	expect(localStorage.getItem('bestFramework')).toBe('Vitest');
});
```

Node’s not going to know what to do with `localStorage`. But in browser mode? Heck yeah, it’ll act just like your browser does when the user opens Inspect Element for absolutely no reason and criticizes your CSS.

### A Couple of Gotchas

Alright, real talk—nothing’s perfect. There are a few things to keep in mind when running tests in browser mode:

1. **It’s still not a real browser.** You're not getting every subtlety of a specific Chrome or Firefox version. Remember, this is JSDOM-ified, meaning it's designed to act like a browser rather than being one.
2. **Performance.** Running tests with `jsdom` can be a bit slower than straight-up Node tests. It’s the cost of emulating browser stuff.
3. **Browser-specific issues.** Just because something works in Vitest browser mode doesn’t mean it’ll work in *all* browsers. (I’m looking at you, Internet Explorer.)

### Conclusion

Boom! You’ve now got yourself the ability to test in an actual browser environment. You’re no longer stuck hoping that your code for DOM manipulation works correctly—*you'll know it does*. Tell the Node environment to sit down while you flex your browser-testing muscles.

The best part about this? Once you’ve set it up, you’re golden for most front-end testing scenarios without having to complicate your testing setup with third-party tools or weird CI integrations. Let Vitest handle the fuss so you can write code that, you know, *works* in the browser too.

Happy testing!

```ts
```
