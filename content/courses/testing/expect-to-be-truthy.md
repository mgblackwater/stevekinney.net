### Reasons to Use `toBeTruthy`

- **Ensuring Values Are Considered True:** Use `toBeTruthy` when you want to check that a value is "truthy," meaning it’s not `false`, `0`, `null`, `undefined`, or an empty string.
- **Validating Conditional Logic:** It’s helpful for confirming that properties or function returns, like the status of Green Day band members or their songs, are truthy in condition checks.
- **Checking Non-Falsey Values:** Sometimes you want to confirm that a value exists in a way that it can be used in logical flows, ensuring it's "truthy" even if not explicitly `true`.

### Example

Here’s an example where we use `toBeTruthy` to check if a Green Day album was released (i.e., the release year is a truthy value):

```ts
import { expect, test } from 'vitest';

function getGreenDayAlbum() {
	return {
		title: '21st Century Breakdown',
		releaseYear: 2009,
	};
}

test('album release year is truthy', () => {
	const album = getGreenDayAlbum();
	expect(album.releaseYear).toBeTruthy();
});
```

In this test, we're verifying that the `releaseYear` property is a truthy value.

### Challenge

**Challenge:** Write a test using `toBeTruthy` to check that the property `vocalist` in the object returned by the `getGreenDayBandMembers` function is truthy. The function is provided below:

```ts
function getGreenDayBandMembers() {
	return {
		vocalist: 'Billie Joe Armstrong',
		drummer: 'Tré Cool',
	};
}
```

Your task:

- Write a test to assert that the `vocalist` property is truthy.
- Use `toBeTruthy` to ensure the test passes.
