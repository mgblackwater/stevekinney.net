### Reasons to Use `toBeFalsy`

- **Ensuring Values Are Falsey:** Use `toBeFalsy` to confirm that a value is considered "falsey" (i.e., it evaluates to `false`, `0`, `null`, `undefined`, or an empty string).
- **Testing Unmet Conditions:** It’s useful for checking whether certain conditions or values, like the existence of an unreleased Green Day album, are falsey and don't fulfill a condition.
- **Validating Absence or Empty Values:** Sometimes you need to ensure that a property or variable is either unset or empty, making `toBeFalsy` ideal for these scenarios.

### Example

Here’s an example where we use `toBeFalsy` to check if a Green Day album is not released (i.e., the release year is falsy):

```ts
import { expect, test } from 'vitest';

function getGreenDayAlbum() {
	return {
		title: 'Nimrod',
		releaseYear: null,
	};
}

test('album release year is falsy', () => {
	const album = getGreenDayAlbum();
	expect(album.releaseYear).toBeFalsy();
});
```

In this test, we're verifying that the `releaseYear` property is falsy (because it's `null`).

### Challenge

**Challenge:** Write a test using `toBeFalsy` to check that the property `bassist` in the object returned by the `getGreenDayBandMembers` function is falsy. The function is provided below:

```ts
function getGreenDayBandMembers() {
	return {
		vocalist: 'Billie Joe Armstrong',
		drummer: 'Tré Cool',
	};
}
```

Your task:

- Write a test to assert that the `bassist` property is falsy.
- Use `toBeFalsy` to ensure the test passes.
