# Testing TypeScript Types with Vitest

So, you're writing TypeScript, and it's all fun and games until you realize: "_How do I test that my types are doing what I want them to do?_" Testing logic? Totally fine. You’ve got the whole `expect`-`toEqual` dance down. But testing **types**? That’s not as intuitive. That’s _mind-bending matrix stuff_, or so it seems. Don’t worry, my friend—we're in this together. Let’s crack the nut and figure out the best way to make sure our types are doing what they're supposed to.

## Why Test Types?

Before we get into the fun stuff, let’s ask ourselves a vital question. What on earth are we doing here? Why test types? I mean, TypeScript is supposed to _prevent_ us from messing up types, right?

Absolutely. TypeScript checks types at **compile-time**, but the TypeScript compiler isn’t infallible. We write our own types, sometimes pretty complex ones, and—surprise—mistakes happen. But even more importantly, if you're writing a library or sharing code in any way, you want to ensure that **the contracts you create with your types** stand the test of time. Your API’s functionality could change, and your types need to change accordingly. Skip testing your types and… boom… now everything's broken. Trust issues all around.

## Setting Up Vitest

Alright, so you know why you should test types. Let’s saddle up Vitest and get rollin'.

First, let's install Vitest and its necessary bits, especially if you've been living in "Jest-land":

```bash
npm install vitest --save-dev
npm install tsd
```

We’ve also installed **TSD**, a nifty tool for **asserting type definitions**. This duo turns us into type-testing warriors. Vitest checks runtimes, and TSD ensures we're not making bonkers type errors.

## Writing a Type Test with TSD

Let’s go old-school with a simple function typing test. Suppose we've got a `getUserName` function.

```ts
type User = { id: number; name: string };

const getUserName = (user: User) => user.name;
```

Easy enough, right? It takes a `User` and returns their `name`. Great! Let’s put on our testing hat and check that this is working as expected type-wise.

Create a file called `getUserName.test-d.ts`:

```ts
import { expectTypeOf } from 'vitest';
import { getUserName } from './getUserName';

type User = { id: number; name: string };

expectTypeOf(getUserName).toBeFunction();
expectTypeOf<User>().toHaveProperty('name', 'string');
expectTypeOf(getUserName).parameter(0).toMatchTypeOf<User>();
expectTypeOf(getUserName({ id: 1, name: 'Steve' })).toEqualTypeOf('Steve');
```

What’s happening here? This is **type assertion city**, my friend.

1. **We check that `getUserName` is a function.** Seems pointless until you realize TypeScript compilers can make functions look like objects—things occasionally get weird.
2. **Assertion on the `User` type**, ensuring it has a `name` property of type `string`. This could save your skin in the future if someone comes along and changes that property by accident.

3. **Verification on `getUserName`'s parameter**. It needs to match the `User` type, and we’re sticking to that.

4. Finally, **we assert that the result from `getUserName` is of type `'Steve'`**, which again should be a `string`. If our inferencing fails or types change, we’ll catch it right here.

## Running Your Type Tests

TSD is what actually runs your type checks. You can't really “run” these tests with Vitest alone since they happen at **compile-time**, but TSD hooks into everything nicely:

```bash
npx tsd
```

You'll get a nice, calm response (hopefully) implying your types haven't exploded. If something does go wrong, TSD will show where the type error failed.

## Testing Edge Cases

Okay, so we’ve covered the basics. Let’s up our game and deal with the fun stuff: **edge cases.**

Imagine you’ve got a gnarly type function that _conditionally_ derives types (probably written at 2 a.m. while fueled by coffee and blind ambition). You'd want to test every possible branch of that type function to avoid catastrophe.

Let’s say you over-engineer a type:

```ts
type MaybeUser<T> = T extends true ? User : null;
```

This function says, “If you give me `true`, I’ll give you a `User`. If you don’t, here’s a `null`—good luck building your app now.” 😅

We can just as easily test a type like this:

```ts
expectTypeOf<MaybeUser<true>>().toEqualTypeOf<User>();
expectTypeOf<MaybeUser<false>>().toEqualTypeOf<null>();
```

When running `npx tsd`, this ensures TypeScript’s conditional wizardry actually processes things the way we think it does. If TypeScript spits a compiler error, we missed something. Without this test, we might have gone blissfully into production and then out comes the flood of GitHub Issues. 🎉

## Final Thoughts

You don't need to test every single type manually (that's the TypeScript compiler’s job in day-to-day coding). But for critical things—like your API contracts or type utilities—it’s a great idea to _assert_ those definitions in your tests. It keeps your **codebase more reliable** and your **future self 100% happier**.

Vitest combined with TSD gives you this sweet setup where you can test not only your runtime logic but the **very structure** of the data flowing through your app. This is like juggling two flaming swords instead of just one lightsaber. You’re a bit of a testing ninja now.

Good luck out there, and happy type hacking. 🙌
