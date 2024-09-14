### Reasons to Use `toBeTypeOf`

- **Checking Primitive Data Types:** Use `toBeTypeOf` to ensure that a value is of a specific primitive type, such as checking if a Green Day album’s title is a string.
- **Validating Input and Return Types:** It’s helpful for confirming that function outputs or variable assignments match the expected type (e.g., ensuring a band's song duration is a number).
- **Preventing Type Mismatches:** Using `toBeTypeOf` helps prevent unexpected type issues in your code by asserting the correct types.

### Example

Here’s an example where we use `toBeTypeOf` to check if the title of a Green Day album is a string:

```ts
import { expect, test } from 'vitest';

function getGreenDayAlbum() {
	return {
		title: 'Warning',
		releaseYear: 2000,
	};
}

test('album title is a string', () => {
	const album = getGreenDayAlbum();
	expect(album.title).toBeTypeOf('string');
});
```

In this test, we're verifying that the `title` property of the album is of type `string`.

### Challenge

**Challenge:** Write a test using `toBeTypeOf` to check that the `releaseYear` property in the object returned by the `getGreenDayAlbum` function is a number. The function is provided below:

```ts
function getGreenDayAlbum() {
	return {
		title: 'Kerplunk',
		releaseYear: 1991,
	};
}
```

Your task:

- Write a test to assert that the `releaseYear` property is of type `number`.
- Use `toBeTypeOf` to ensure the test passes.
