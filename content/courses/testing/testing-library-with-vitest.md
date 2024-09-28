# Testing Library + Vitest: A Dream Team for Testing UIs

Alright, so you’ve moved past the days of manually clicking every button and scrolling every list to make sure your app didn’t spontaneously combust after your last code change. Good choice. Now you’re ready to supercharge your UI testing with a little help from **Testing Library** and **Vitest**. It's like Batman and Robin—both awesome on their own, but together… well, that’s where you build your test-driven UI Gotham.

Let’s break down how to get **Testing Library** set up with **Vitest**, and write some tests for real-world scenarios—things you’ll actually face when you just want to make sure the user experience behaves as expected without curling into the fetal position when faced with a bug.

## Step 1: Setting Things Up

Before we write tests, we need to stage the environment. You probably already have Vitest installed (if not, what kind of renegade are you? Get on that!). We’ll also need **@testing-library/vue** (or React, depending on your framework), which is geared up to help us render components and interact with them in ways the user would.

Let’s do the installs:

```bash
npm install vitest @testing-library/vue @testing-library/jest-dom --save-dev
```

Notice that **@testing-library/jest-dom** sneaks in here? It's got handy assertions like `toBeInTheDocument()` and `toHaveTextContent()` that make our tests super readable, but don't worry—it plays **very** nicely with Vitest.

Once installed, we’ll slip a little **vitest.config.js** refinement under the hood:

```javascript
import { defineConfig } from 'vitest/config';

export default defineConfig({
  test: {
    environment: 'jsdom',
  },
});
```

This 'jsdom' pseudo-browser environment means Vitest can pretend it's running in a browser, which is where our UI lives. It’s like method acting for test environments.

## Step 2: Let’s Write That First Test

Let’s say we have a **Button** component that shows a counter. When you click the button, the counter goes up. Super simple, right?

Here's what our Vue button looks like. (React folks, feel free to translate—this is as basic as it gets):

```vue
<template>
  <button @click="increment">{{ count }}</button>
</template>

<script setup>
import { ref } from 'vue';

const count = ref(0);
const increment = () => {
  count.value++;
};
</script>
```

Now we want to test it. Specifically, we want to ensure that:

1. The button renders.
2. The `count` starts at zero.
3. Clicking the button increases the count.

That’s it. Here's how that test would look:

```javascript
import { render, fireEvent } from '@testing-library/vue';
import { describe, it, expect } from 'vitest';
import Button from './Button.vue';

describe('Button component', () => {
  it('should render and increment when clicked', async () => {
    const { getByText } = render(Button);

    // The button should show "0" at start
    const button = getByText('0');
    expect(button).toBeInTheDocument();

    // Click it and see what happens
    await fireEvent.click(button);
    expect(button).toHaveTextContent('1');

    // Click it again, because who doesn't love clicking buttons?
    await fireEvent.click(button);
    expect(button).toHaveTextContent('2');
  });
});
```

Let’s break down what just happened:

- **render(Button)**: This uses Testing Library to spin up an instance of our Vue component. No need to manually mount anything—Testing Library’s got your back.
- **getByText('0')**: We’re saying, “Hey, does this button initially display a ‘0’?” If not, Vitest will let us know (probably in red—prepare for some eye strain).
- **fireEvent.click(button)**: You're simulating a user's interaction—they’ve got their mouse primed and ready to click.
- **expect(button).toHaveTextContent('1')**: After one click, the counter goes up, and the button now says "1". If the test passes, you see green. If it fails, well… time to debug.

## Step 3: Assert Like a Pro with jest-dom

Remember that **@testing-library/jest-dom** package? Here’s where it shines. Normally, with vanilla JavaScript, we’d be writing assertions that look like `expect(button.textContent).toBe('0')`. But with **jest-dom**, we can go even further for more readable assertions like `toBeInTheDocument()` and `toHaveTextContent()`. These are like giving your test assertions a nice, ergonomic office chair—better all-around experience.

## Step 4: Mocking and Stubbing (When the World Gets Messy)

Sometimes you end up in a situation where you don’t want to actually make API calls during testing or trigger certain side effects (timers, network requests, etc.). That’s where **Vitest’s mocking** capabilities hit the scene with their best attempt at damage control.

For example, you’ve got a component making an API request inside a `fetchData()` method. We don’t need `fetchData()` calling our backend every time we run the test—that’d be like pinging an actual pizza delivery every time you clicked "Order." Let’s mock it:

```javascript
import { vi, describe, it, expect } from 'vitest';
import { render } from '@testing-library/vue';
import MyComponent from './MyComponent.vue';

describe('MyComponent', () => {
  it('should call fetchData on mount', () => {
    const fetchData = vi.fn(); // Mock it.
    render(MyComponent, {
      methods: {
        fetchData,
      },
    });

    expect(fetchData).toHaveBeenCalled();
  });
});
```

Here, **vi.fn()** gives us a mock version of the `fetchData()` method. We didn’t need the real one for the test—we just wanted to verify that it was called on mount, and **voilà**, Vitest made that painless.

## Wrapping Up

With **Vitest** + **Testing Library**, you’re not just slapping a test together. You’re testing like a pro by thinking about things from your user's perspective and ensuring the frontend is bullet-proof. The combination lets you build simple, clear, maintainable tests without drowning in boilerplate.

Now go ahead and click intentionally broken buttons with a smug grin. Your tests have you covered.
