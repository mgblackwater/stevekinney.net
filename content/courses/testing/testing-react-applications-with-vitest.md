---
title: Testing React Applications With Vitest
description: Learn how to test React apps using Vitest, from setup to mocks.
modified: 2024-09-28T11:31:14-06:00
---

## Testing React Applications with Vitest

Alright, folks, let’s dive into testing React applications using a tool that’s really been gaining steam: **Vitest**. If you’ve suffered through testing libraries before, wondering why your dev life needs to be as painful as it sometimes is—Vitest might just offer an oasis of sanity.

At its core, Vitest is **fast**, **simple**, and **integrated out of the box with Vite**, which means faster startup times, instant TypeScript support, and zero-config goodness. Let’s aim for clarity and focus on testing what matters in React.

### Setting up Vitest for a React Project

So, we’ve got a React app. Nothing fancy—maybe a simple button component. First things first, installing Vitest. Assuming you’re using Vite for your project, installation is pretty straightforward.

In your React-Vite project, just run:

```bash
npm install vitest @testing-library/react @testing-library/jest-dom --save-dev
```

If you’re not using Vite… well, **why**? Seriously though, it’ll still work, but the instructions might diverge slightly.

Next, in your `vite.config.js`, add this beautiful block of Vitest goodness:

```js
/// vite.config.js

export default {
	test: {
		environment: 'jsdom', // Required for React DOM testing
		globals: true, // This lets us use things like expect() without importing
		setupFiles: './setupTests.js', // Optional: If you've got setup requirements
	},
};
```

The `jsdom` environment is what gives us a “browser-like” experience, required for the kind of DOM manipulation that React components love to do. Without it, everything’s gonna blow up like the Death Star—and not in a cool way.

Finally, create a `setupTests.js` file if you want global mocks or imports like `@testing-library/jest-dom`:

```js
// setupTests.js
import '@testing-library/jest-dom';
```

Now, **buckle up!** Time to write our first React test.

### Writing a Basic React Test

Alright, imagine we have a simple React component that renders a button. The button increments a counter when you click it. Here's the component:

```jsx
// CounterButton.jsx

import { useState } from 'react';

export default function CounterButton() {
	const [count, setCount] = useState(0);

	return <button onClick={() => setCount(count + 1)}>Count is {count}</button>;
}
```

Let’s make sure this baby works with a test. Inside your `src` or `components` folder (really up to you), create a `CounterButton.test.jsx` file—because, **who doesn’t love putting `.test` in a file name?**

```jsx
// CounterButton.test.jsx

import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import CounterButton from './CounterButton';

describe('CounterButton Component', () => {
	it('increments counter on click', async () => {
		render(<CounterButton />);

		const button = screen.getByText(/count is/i); // Grab that button by its text

		expect(button).toHaveTextContent('Count is 0'); // Initial state check

		// Simulate a user click (not an *actual* click, don't hurt your screen)
		await userEvent.click(button);

		expect(button).toHaveTextContent('Count is 1'); // State has updated, test should pass
	});
});
```

Breakdown of the magic:

1. **render:** Renders the component as if it were in a DOM environment.
2. **screen.getByText:** Finds an element based on its text. Could also use other `getBy` queries, but let’s KISS (Keep It Super Simple).
3. **userEvent:** Simulates a user clicking the button, as if they were enthusiastically trying to crash your app (don’t worry—the test passes).

Vitest magically takes care of testing in a jsdom environment, and because of that global jest-dom goodness, we don’t even need to import jest-specific methods like `expect`. Those are already **wired up** for you.

### Running Our Tests

Now that we’ve crafted a pure work of art (the test, obviously), let’s run it. In your `package.json`, add a script:

```json
"scripts": {
  "test": "vitest"
}
```

And then run your tests:

```bash
npm test
```

Ahhh, the sweet sight of green checkmarks.

### Mocking in a React Test

Sometimes we have components that rely on external modules, or eeevil dependencies that you’d rather not deal with in your tests. Maybe your component grabs data from an API or processes payments (big yikes!). The way to ensure your test is fast and doesn't make unexpected network calls is **mocking**.

Vitest can handle mocks beautifully. Imagine our button had an additional backend interaction we want to control in our tests.

```jsx
// api.js

export function logClick() {
	console.log('Click logged!');
}
```

Let’s pretend that for some bizarre reason, our `CounterButton` logs every click to a backend server via `logClick`. Weird use case, but hey, YOLO.

```jsx
// CounterButton.jsx

import { logClick } from './api';

export default function CounterButton() {
	const [count, setCount] = useState(0);

	const handleClick = () => {
		logClick();
		setCount(count + 1);
	};

	return <button onClick={handleClick}>Count is {count}</button>;
}
```

Now, let's mock that `logClick` function in our test, so we don’t flood our imaginary logging service with test data:

```jsx
// CounterButton.test.jsx

import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import CounterButton from './CounterButton';
import * as api from './api'; // Import everything from that module

vi.mock('./api'); // This tells Vitest to mock the entire api module

describe('CounterButton with API mock', () => {
	it('increments counter and calls logClick on click', async () => {
		render(<CounterButton />);

		const button = screen.getByText(/count is/i);

		const logClickSpy = vi.spyOn(api, 'logClick'); // Spy on the logClick function

		await userEvent.click(button);

		expect(button).toHaveTextContent('Count is 1');
		expect(logClickSpy).toHaveBeenCalledTimes(1); // Make sure our API interaction would have happened once
	});
});
```

Vitest’s mocking is like a voodoo doll for your modules. You **zap** your real function, and the test just uses your mock. You’re in control. I mean, not in life, but in your tests—definitely.

### Conclusion

So there you have it—testing React apps with Vitest. We’ve covered installation, basic usage, state testing, mocking external dependencies—basically everything you need to get up and running. Testing doesn't have to suck. In fact, with tools like Vitest and **React Testing Library**, it can become an enjoyable (gasp) part of your workflow.

The key? Keep your tests small, focused, and fast. And always remember: green means go.

Now, go forth and write some awesome tests.

```ts

```
