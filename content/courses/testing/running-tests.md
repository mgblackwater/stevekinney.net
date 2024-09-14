You can use `vitest` to run your tests.

```sh
npx vitest

# Or, if you wanted to run your tests with Vitest UI.
npx vitest --ui
```

For our use cases, we have it defined in our `package.json`. For example, you might have the following script in your `package.json`.

```json
{
	// … other stuff
	"scripts": {
		"test": "vitest"
	}
	// more stuff…
}
```

This means that you can do run `npm test` to run our tests. If you wanted to run it with the UI, you could run `npm test -- --ui` to forward the flag to the `npm` script.
