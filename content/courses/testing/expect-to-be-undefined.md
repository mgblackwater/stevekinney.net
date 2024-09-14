### Reasons to Use `toBeUndefined`

- **Checking for Uninitialized Values:** You might need to verify that a value, such as a band member’s role or information, has not been initialized or assigned yet.
- **Ensuring Correct Defaults:** In some cases, certain information, like an unreleased album title, should remain `undefined` until it is explicitly assigned.
- **Testing Fallback Logic:** If a piece of logic relies on something being undefined (like an unused band instrument), you can test for that with `toBeUndefined`.

### Example

Here’s an example where we use `toBeUndefined` to check if a property is not set on a Green Day album:

```ts
import { expect, test } from 'vitest';

function getGreenDayAlbum() {
	return {
		title: 'Dookie',
		releaseYear: 1994,
	};
}

test('album does not have a producer defined', () => {
	const album = getGreenDayAlbum();
	expect(album.producer).toBeUndefined();
});
```

In this test, we're verifying that the `producer` property is `undefined` on the returned Green Day album object.

### Challenge

**Challenge:** Write a test using `toBeUndefined` to check that the property `guitarist` is not defined in the object returned by the `getGreenDayBandMembers` function. The function is provided below:

```ts
function getGreenDayBandMembers() {
	return {
		vocalist: 'Billie Joe Armstrong',
		drummer: 'Tré Cool',
	};
}
```

Your task:

- Write a test to assert that the `guitarist` property is `undefined`.
- Use `toBeUndefined` to ensure the test passes.
