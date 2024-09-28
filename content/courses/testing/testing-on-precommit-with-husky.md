# Testing on Precommit with Husky

Alright, friend, let's talk about something that we all wish we could wave a magic wand for: keeping bad code out of `main`. You know what I'm talking about. Code that _works on my machine_ but crashes the staging server faster than you can say "merge conflict."

The answer? **Precommit Hooks**. Specifically, using **Husky** to run your tests right before you even commit that code. This is like an invisible shield that checks if your code is legit before taking its first step into the Git ecosystem.

Let's dive in and break it down. In this guide, we’ll set up **Husky** to run our **Vitest** tests (or any tests, really) in the smoothest way possible.

## Step 1: Install Husky

So, Husky is like a bouncer at a nightclub—but for your Git commits. Before the code can get in, Husky will make sure it's passing.

You can add it to your project with the following:

```bash
npm install husky --save-dev
```

Or if you're rollin' with yarn:

```bash
yarn add husky --dev
```

## Step 2: Set Up Husky

Now that Husky’s standing at the door, we need to teach it how to bounce the riffraff. First, enable Husky hooks:

```bash
npx husky install
```

This command will create a `.husky/` directory where Husky keeps all its files. It’s important. Don’t delete it. Don’t ignore it. This is where the magic lives.

Next, let's tell Husky to actually work its magic _every time_ we run `git commit`. How? Let’s add a pre-commit hook!

```bash
npx husky add .husky/pre-commit "npm test"
```

This command does two things:

1. Creates a `pre-commit` file inside `.husky/`.
2. Makes sure Husky is running `npm test` before you’re allowed to commit anything. So if your tests are broken, you get a terrifying red error screen and sad trombone noises… but no commit.

If you're using Yarn, just replace `npm test` with `yarn test`.

## Step 3: Make Sure Vitest Runs

Normally, Vitest just works out of the box with `npm test`, but if you haven't set up your test command for Vitest, let’s make sure that's looking good.

In your `package.json` file:

```json
{
  "scripts": {
    "test": "vitest"
  }
}
```

Now, when Husky runs `npm test`, it’s really running your Vitest tests.

## Step 4: Create Some Tests

Alright, you’ve got Husky all geared up, but what if your code always passes because you don't have tests (yet)? Gotcha. Let’s make sure we have a simple test in place.

In your test file, something like this should work:

```javascript
// src/math.js
export function add(a, b) {
  return a + b;
}

// test/math.test.js
import { describe, it, expect } from 'vitest';
import { add } from '../src/math';

describe('add', () => {
  it('should add two numbers', () => {
    expect(add(1, 2)).toBe(3);
  });
});
```

We're testing a basic `add` function. So simple! But let's be honest—_everyone_ writes bugs in even the simplest code. So let's make sure this runs every time!

## Step 5: Test the Precommit Hook

It’s time for the moment of truth. Let’s make some changes and try to commit those bad boys.

1. First, stage your changes:

   ```bash
   git add .
   ```

2. Then commit:

   ```bash
   git commit -m "Adding some killer math!"
   ```

If everything works out, tests will run, pass, and your commit will sail into the Git history without a scratch. 🎉

But if something fails—and, let’s face it, that _always happens_ when we want to show off in front of a coworker—your commit will be blocked. Don’t worry, though; Husky has your back. Fix your tests first, and then try committing again. You’ll thank future you.

## Bonus: Linting and Prettifying

Why stop at testing? You can also throw in some linting and prettier checks before commits, like it’s a party. If you’re using eslint and prettier:

```bash
npx husky add .husky/pre-commit "npm run lint && npm run format && npm test"
```

A perfect world: linted, prettified, and tested code _before it even leaves your machine_.

## Conclusion

There you have it! With **Husky** by your side, and your tests running every time you commit code, you won’t accidentally blow up production because you typo’d a variable name. It happens to the best of us—better to catch it before your code gets shipped.

The beauty of Husky is that it’s fast, it protects your team (and you, let’s be honest), and it makes sure no bad code gets committed. Now go forth and ship with confidence… or at least not with broken tests.
