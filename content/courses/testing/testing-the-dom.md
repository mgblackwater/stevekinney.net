---
modified: 2024-09-07T15:13:46-06:00
---
Yes. Node runs JavaScript just like the browser. It's also missing a bunch of stuff that you'll normally find in your browser of choice—namely, the DOM.

## Using JSDOM with Vitest

```js
import { defineConfig } from 'vite';

export default defineConfig({
	test: {
		environment: 'jsdom',
	},
});
```
