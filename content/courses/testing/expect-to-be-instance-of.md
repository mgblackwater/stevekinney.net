### Reasons to Use `toBeInstanceOf`

- **Checking Object Instances:** Use `toBeInstanceOf` to confirm that a value or object is an instance of a specific class, such as when checking if a Green Day album is represented by the correct class.
- **Ensuring Correct Types:** It’s helpful for ensuring that your objects are constructed correctly, like making sure a band member is an instance of a `Musician` class.
- **Validating Prototypes:** This assertion helps in verifying the inheritance chain of objects, ensuring they follow the intended prototype structure.

### Example

Here’s an example where we use `toBeInstanceOf` to check if a Green Day album is an instance of the `Album` class:

```ts
import { expect, test } from 'vitest';

class Album {
	constructor(
		public title: string,
		public releaseYear: number,
	) {}
}

function getGreenDayAlbum() {
	return new Album('Insomniac', 1995);
}

test('album is an instance of Album class', () => {
	const album = getGreenDayAlbum();
	expect(album).toBeInstanceOf(Album);
});
```

In this test, we're verifying that the `album` object is an instance of the `Album` class.

### Challenge

**Challenge:** Write a test using `toBeInstanceOf` to check that the `vocalist` in the object returned by the `getGreenDayBandMember` function is an instance of the `Musician` class. The function and class are provided below:

```ts
class Musician {
	constructor(
		public name: string,
		public role: string,
	) {}
}

function getGreenDayBandMember() {
	return new Musician('Billie Joe Armstrong', 'vocalist');
}
```

Your task:

- Write a test to assert that the `vocalist` is an instance of the `Musician` class.
- Use `toBeInstanceOf` to ensure the test passes.
