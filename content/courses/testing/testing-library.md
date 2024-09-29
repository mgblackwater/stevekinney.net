---
title: Setting Up and Writing Tests with Testing Library and Vitest
description: Learn to write user-focused tests using Testing Library with Vitest.
modified: 2024-09-28T16:00:32-06:00
---

[Testing Library](https://testing-library.com/) is designed to help you out with writing tests that focus on user behavior rather than implementation details. It provides utilities to query and interact with the DOM in a way that's similar to how users would.

## Why Use Testing Library?

- **User-Centric Testing**: Focuses on testing components from the user's perspective.
- **Avoids Implementation Details**: Reduces coupling between tests and component internals.
- **Improved Test Reliability**: Leads to tests that are more robust and less prone to breakage due to refactoring.
- **Accessible Queries**: Encourages testing with queries that reflect accessibility best practices.

In your `vitest.config.js` file, set the `test` environment to `'jsdom'` (or `happy-dom`). Just like we did in [Testing the DOM](testing-the-dom.md).

```javascript
// vitest.config.js
export default {
	test: {
		environment: 'jsdom',
	},
};
```

## Writing Tests with Testing Library

Okay, let's imagine that we have this very, very simple component.

```jsx
// Greeting.js
import React from 'react';

export function Greeting({ name }) {
	return <h1>Hello, {name}!</h1>;
}
```

The tests might look something like this.

```jsx
// Greeting.test.jsx
import { render, screen } from '@testing-library/react';
import { expect, test } from 'vitest';
import { Greeting } from './Greeting';

test('renders greeting message', () => {
	const name = 'Alice';

	render(<Greeting name={name} />);

	expect(screen.getByText(`Hello, ${name}!`)).toBeInTheDocument();
});
```

## Best Practices with Testing Library

### Querying Elements the Right Way

Testing Library provides various queries:

- **Preferred Queries**: `getByRole`, `getByLabelText`, `getByPlaceholderText`, `getByText`, etc.
- **Less Preferred**: `getByTestId`

So, for this `<Button />` component:

```jsx
// Button.js
export function Button({ onClick, children }) {
	return <button onClick={onClick}>{children}</button>;
}
```

You'd maybe have a test that looks something like this:

```jsx
test('calls onClick when clicked', () => {
	const handleClick = vi.fn();
	render(<Button onClick={handleClick}>Click Me</Button>);

	const button = screen.getByRole('button', { name: /click me/i });
	button.click();

	expect(handleClick).toHaveBeenCalledTimes(1);
});
```

Using `getByRole('button')` is preferred as it reflects how users (including those using assistive technologies) interact with the application.

### Avoid Testing Implementation Details

Focus on what the component renders, not how it renders it. So, you'd want to **avoid** doing something like this:

```jsx
const { container } = render(<Component />);
const div = container.querySelector('.my-class');
```

Instead, you'd **prefer** to do something like this:

```jsx
const element = screen.getByText(/some text/i);
```

### Use `userEvent` for Simulating User Interactions

`userEvent` simulates real user interactions more accurately than `fireEvent`.

**Example:**

```jsx
import userEvent from '@testing-library/user-event';

test('checkbox toggles correctly', async () => {
	render(<CheckboxComponent />);
	const checkbox = screen.getByRole('checkbox');

	await userEvent.click(checkbox);
	expect(checkbox).toBeChecked();
});
```

### Async-Await with Asynchronous Updates

When testing components that update asynchronously, use `findBy` queries and `waitFor`.

**Example:**

```jsx
test('loads and displays data', async () => {
	render(<AsyncComponent />);
	expect(screen.getByText(/loading/i)).toBeInTheDocument();

	const dataElement = await screen.findByText(/loaded data/i);
	expect(dataElement).toBeInTheDocument();
});
```

### Clean Up After Tests

Testing Library automatically cleans up the DOM after each test, but if you have additional cleanup, handle it in `afterEach`.

### Use `jest-dom` Matchers

Enhance your assertions with matchers like `toBeInTheDocument`, `toBeVisible`, etc.

```jsx
import '@testing-library/jest-dom';

expect(element).toBeVisible();
```

## When is Testing Library Beneficial?

### Testing User Interactions

- **Benefit**: Ensures that components behave correctly when users interact with them.
- **Example**: Testing form submissions, button clicks, input changes.

### Ensuring Accessibility

- **Benefit**: Encourages the use of accessible selectors (`getByRole`, `getByLabelText`).
- **Example**: Verifying that all interactive elements are accessible via screen readers.

### Component Behavior Testing

- **Benefit**: Validates that components render the correct output based on props and state.
- **Example**: Testing conditional rendering, list outputs, error messages.

### Avoiding Fragile Tests

- **Benefit**: Tests are less likely to break due to changes in implementation details (like class names or DOM structure).
- **Example**: Refactoring a component without changing its external behavior won't break the tests.

## Potential Pitfalls and How to Avoid Them

### Over-Reliance on `getByTestId`

**Issue**: Using `data-testid` attributes can lead to tests that are more coupled to implementation details.

**Solution**:

- Prefer queries that reflect how users interact with the UI.
- Use `getByTestId` only when no other query is suitable.

### Ignoring Accessibility Considerations

**Issue**: Not using accessible queries can miss out on testing the component's accessibility.

**Solution**:

- Use queries like `getByRole`, `getByLabelText`, which encourage accessible coding practices.

### Not Waiting for Asynchronous Updates

**Issue**: Tests may pass or fail inconsistently if they don't account for asynchronous state updates.

**Solution**:

- Use `findBy` queries or `waitFor` to wait for asynchronous changes.

### Testing Implementation Details

**Issue**: Querying specific DOM nodes or component internals can make tests fragile.

**Solution**:

- Focus on the component's output and user interactions.
- Avoid reaching into component internals or relying on specific HTML structures.

### Mocking Too Much

**Issue**: Over-mocking components and modules can lead to tests that don't reflect actual behavior.

**Solution**:

- Mock external dependencies like network requests.
- Avoid mocking components unless necessary.

## Examples in Practice

### Testing Form Submission

```jsx
// LoginForm.js
export function LoginForm({ onSubmit }) {
	return (
		<form
			onSubmit={(e) => {
				e.preventDefault();
				const { username, password } = e.target.elements;
				onSubmit({
					username: username.value,
					password: password.value,
				});
			}}
		>
			<label>
				Username:
				<input name="username" />
			</label>
			<label>
				Password:
				<input name="password" type="password" />
			</label>
			<button type="submit">Login</button>
		</form>
	);
}
```

**Test:**

```jsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { expect, test, vi } from 'vitest';
import { LoginForm } from './LoginForm';

test('submits the form with username and password', async () => {
	const handleSubmit = vi.fn();
	render(<LoginForm onSubmit={handleSubmit} />);

	await userEvent.type(screen.getByLabelText(/username/i), 'myUser');
	await userEvent.type(screen.getByLabelText(/password/i), 'myPass');
	await userEvent.click(screen.getByRole('button', { name: /login/i }));

	expect(handleSubmit).toHaveBeenCalledWith({
		username: 'myUser',
		password: 'myPass',
	});
});
```

**Explanation:**

- Simulates user typing into inputs and clicking the submit button.
- Verifies that `onSubmit` is called with the correct data.

### Testing Component with Asynchronous Data

```jsx
// DataLoader.js
import React, { useEffect, useState } from 'react';

export function DataLoader() {
	const [data, setData] = useState(null);

	useEffect(() => {
		fetch('/api/data')
			.then((res) => res.json())
			.then(setData);
	}, []);

	if (!data) return <div>Loading…</div>;
	return <div>Data: {data.value}</div>;
}
```

**Test:**

```jsx
import { render, screen } from '@testing-library/react';
import { expect, test, vi } from 'vitest';
import { DataLoader } from './DataLoader';

test('loads and displays data', async () => {
	global.fetch = vi.fn(() =>
		Promise.resolve({
			json: () => Promise.resolve({ value: 'Test Data' }),
		}),
	);

	render(<DataLoader />);
	expect(screen.getByText(/loading/i)).toBeInTheDocument();

	const dataElement = await screen.findByText(/data: test data/i);
	expect(dataElement).toBeInTheDocument();

	global.fetch.mockRestore();
});
```

**Explanation:**

- Mocks `fetch` to return test data.
- Uses `findByText` to wait for the asynchronous update.

## Conclusion

Testing Library, when used with Vitest, provides a powerful combination for testing React components effectively. By focusing on user interactions and component behavior, you can write tests that are reliable, maintainable, and reflective of real-world usage.

## Key Takeaways

- **User-Focused Testing**: Emphasize testing how users interact with your components.
- **Accessible Queries**: Use queries that reflect accessibility best practices.
- **Avoid Implementation Details**: Prevent tests from breaking due to internal changes.
- **Handle Asynchronous Code**: Properly wait for updates with `findBy` and `waitFor`.
- **Leverage `userEvent`**: Simulate user interactions more accurately.

By adopting these practices, you can create a robust test suite that enhances the quality and reliability of your React applications.

## When to Use Testing Library

- **Component Interaction Testing**: When you need to test how components respond to user actions.
- **Accessibility Compliance**: Ensuring components are accessible and interact as expected.
- **Behavior Verification**: Validating that components render and update correctly based on props and state.

## Potential Limitations

- **Not for Snapshot Testing**: Testing Library focuses on behavior, not on exact DOM structures.
- **Over-Mocking Can Reduce Test Effectiveness**: Be cautious not to mock too much, which can lead to tests that don't reflect actual behavior.

By understanding these aspects, you can use Testing Library effectively in your testing strategy, ensuring that your components meet the needs of your users and function correctly within your application.
