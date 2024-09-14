### Reasons to Use `toBeDefined`

- **Validation of Function Return:** You can use `toBeDefined` to ensure that a function or object properly returns expected values, such as band member information.
- **Ensuring Variable Initialization:** In cases where critical properties, like a band's album title, need to be present, `toBeDefined` helps verify that they are properly initialized.
- **Preventing Unintended `undefined`:** When you expect values like the name of a Green Day song to always be present, `toBeDefined` ensures you aren't unintentionally working with `undefined`.

### Example

Here’s an example where we use `toBeDefined` to check if the album title is properly set:

```ts
import { expect, test } from 'vitest';

function getGreenDayAlbum() {
	return {
		title: 'American Idiot',
		releaseYear: 2004,
	};
}

test('album title is defined', () => {
	const album = getGreenDayAlbum();
	expect(album.title).toBeDefined();
});
```

In this test, we're verifying that the `title` property of the returned Green Day album is defined.

### Challenge

**Challenge:** Write a test using `toBeDefined` to check that the `vocalist` property exists in the object returned by the `getGreenDayBandMembers` function. The function is provided below:

```ts
function getGreenDayBandMembers() {
	return {
		vocalist: 'Billie Joe Armstrong',
		drummer: 'Tré Cool',
	};
}
```

Your task:

- Write a test to assert that the `vocalist` property is defined.
- Use `toBeDefined` to ensure the test passes.
