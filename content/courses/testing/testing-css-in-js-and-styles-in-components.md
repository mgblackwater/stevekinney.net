---
title: Testing CSS-In-JS With Vitest
description: Learn how to test components using CSS-in-JS with Vitest.
modified: 2024-09-28T11:31:14-06:00
---

## 1. Testing CSS-in-JS with Vitest

Ah, testing CSS in JavaScript! It’s not something many of us jump for joy about, but we’ve gotta do what we’ve gotta do, right? When you have components with styles baked right into them using CSS-in-JS (think styled-components, Emotion, or even just some inline styles you've sprinkled in), you might wonder, "Uh… how the heck do I test this?"

Good news! It’s totally possible, and even easy when we break it down. Let’s go on a little adventure and learn how we can verify that our stylish little components are wearing the right outfits.

For this walkthrough, I'll assume you're using something like `styled-components` or `Emotion`, both popular CSS-in-JS libraries, but the principles apply just as well to inline styles or any home-brewed flavor of CSS-in-JS.

Okay, enough prelude. Let’s jump right in!

## 2. Setting Up Vitest with a Styled Component

First things first, if you haven’t already got `Vitest` set up in your project, do that. It’s quick and relatively painless. Here’s a basic install for your project:

```sh
npm install vitest --save-dev
```

Then, add a script to your `package.json` to run the tests:

```json
{
	"scripts": {
		"test": "vitest"
	}
}
```

Now let’s say we’ve got a component that looks something like this:

```jsx
import styled from 'styled-components';

const Button = styled.button`
	background-color: ${(props) => (props.primary ? 'green' : 'gray')};
	color: white;
	border: none;
	padding: 10px 20px;
	cursor: pointer;
`;

const MyButton = ({ primary, children }) => {
	return <Button primary={primary}>{children}</Button>;
};

export default MyButton;
```

This button is super simple—it’s either green if it’s primary, or gray if it’s not. Let’s see how to test that the right styles are being applied.

## 3. Writing Your First Test

How do we check that ticking `primary={true}` paints the button green? Vitest to the rescue, along with something like `@testing-library/react` to help us interact with the rendered component.

First, slap down the required libraries:

```sh
npm install @testing-library/react @testing-library/jest-dom --save-dev
```

Okay, now the test:

```jsx
import { render, screen } from '@testing-library/react';
import '@testing-library/jest-dom';
import MyButton from './MyButton';

test('renders a primary button with green background', () => {
	render(<MyButton primary>Click me</MyButton>);

	const button = screen.getByRole('button', { name: 'Click me' });
	expect(button).toBeInTheDocument();

	// Now for the fun part — checking those styles!
	expect(button).toHaveStyle('background-color: green');
});
```

That’s it! We’re rendering the button, grabbing it by its role (side note: `getByRole` is your BFF for accessibility goodness), and checking the background color matches what we expect when the `primary` prop is passed.

## 4. Testing Non-primary Styles

But we’re not done yet. We should also check what happens when the button _isn’t_ primary:

```jsx
test('renders a non-primary button with gray background', () => {
	render(<MyButton>Secondary action</MyButton>);

	const button = screen.getByRole('button', { name: 'Secondary action' });
	expect(button).toBeInTheDocument();
	expect(button).toHaveStyle('background-color: gray');
});
```

Boom! You see what we're doing here? Render, grab the element, and check the style. Rinse and repeat. If all goes well, Vitest will give you the green checkmark of approval.

## 5. Bonus: Inline Styles

Maybe you’re doing things old school and just chucking some inline styles into your JSX. That’s cool; Vitest can handle that too. Let’s pretend we’ve got this kind of component:

```jsx
const MyInlineButton = () => {
	return <button style={{ backgroundColor: 'blue', color: 'white' }}>Inline Style</button>;
};
```

You can test this exactly like the previous examples:

```jsx
test('renders a button with blue background using inline styles', () => {
	render(<MyInlineButton />);

	const button = screen.getByRole('button', { name: 'Inline Style' });
	expect(button).toHaveStyle('background-color: blue');
});
```

Easy stuff, right? Same process, whether you’re using `styled-components`, inline styles, or something similar.

## 6. Mocking Your Way to Happiness (Advanced Tip)

Now, if you’re using something that dynamically generates class names (like `Emotion` or other libraries), the `.toHaveStyle` may not always be your best bet. In those cases, you might want to test if the correct class is applied or even mock the styles entirely for more complex scenarios. But honestly? For 95% of use-cases, just checking styles directly is gonna get you where you need to go.

So don’t go stressing yourself out over mocks unless you really, really need them!

## 7. Conclusion

Testing CSS-in-JS? Not as bad as it sounds. Vitest and `@testing-library/react` make it feel _almost_ fun. Almost.

Just don’t forget to test all your possible style variants (`primary`, `secondary`, whatever else you’ve got) and ensure your components actually _look_ like they're supposed to. Your future self will thank you when some sneaky little PropMonster™ breaks a style rule, and a test catches it early.

Now, go forth and make the world a better, more well-tested place!

```ts

```
