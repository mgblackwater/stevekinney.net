---
title: Integrating Vitest With Your CI/CD Pipeline
description: Learn how to integrate Vitest into a CI/CD pipeline using GitHub Actions.
modified: 2024-09-28T11:31:15-06:00
---

## 1. Integrating Vitest with Your CI/CD Pipeline

Ah, Continuous Integration and Continuous Deployment (CI/CD)! You know, it’s like having that one responsible roommate who always remembers to lock the door, do the dishes, and make sure your code isn’t breaking everything on the way out to production. Let’s talk about how to hit that sweet deploy button *knowing your tests* are keeping things smooth (or at least flagging the landmines before they go boom).

Today, we’ll get Vitest running in a CI/CD pipeline using GitHub Actions, but you can take these concepts and adapt them to whatever tool your team is using. CI/CD tools all do pretty much the same thing behind the scenes: run your tests, and tell you if things are broken (hopefully before your users do).

### 2. Step 1: Configuring Vitest for CI

Alright, you probably already have Vitest hooked up locally. If you don’t, no worries—I’ll wait while you get it installed. Just:

```bash
npm install vitest --save-dev
```

Now, before you rush into adding tests to the CI, there’s one thing you need to know: **the default Vitest config might run in watch mode** when executed on your local machine, which is *awesome* when you're dev'ing, but it’s pretty pointless in a CI environment. CI systems don’t want to watch for file changes—they want to run through everything once and get you results.

So, let’s make sure Vitest ain’t hanging around looking for changes when we fire it up in CI. We’ll need to add a `test:ci` script to your `package.json`:

```json
{
	"scripts": {
		"test": "vitest",
		"test:ci": "vitest run"
	}
}
```

The key here is the `vitest run` command. This does exactly what you expect: runs the tests once and exits. Perfect for CI.

### 3. Step 2: Writing a GitHub Actions Workflow

Time to bring home the bacon with GitHub Actions (though most CI tools behave very similarly, as I mentioned). In the root of your project, create a directory called `.github/workflows`. Inside, create a `ci.yml` file with the following content:

```yaml
name: CI Pipeline

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: '16'

      - run: npm install
      - run: npm run test:ci
```

Breakdown, layer-by-layer:

- **on**: Automatically run the workflow every time you push something to the `main` branch or open a pull request targeting `main`. Because who doesn’t want their tests running at the earliest opportunity?
- **jobs > test > runs-on**: Sets up the environment. In this case, we’re going with the latest and greatest Ubuntu.
- **steps**: Pretty self-explanatory. This is where we:
  - Checkout the code.
  - Install Node (specific version here is `16`, but you can change that).
  - Install all the dependencies (a.k.a. your `node_modules` lifeline).
  - Run our `npm run test:ci` command.

Boom. That’s it. Once you push this file to your repository, GitHub will start running your tests on every push or pull request. If they pass, green lights all the way! Something goes wrong? Well, GitHub will mercilessly scream red at your face, but better here than in production, right?

### 4. Step 3: (Optional) Adding Coverage Data

Something to consider: **test coverage**. There’s no friendlier reminder than a code coverage report to show how thoroughly (or not) you’re testing your project. If you want to run Vitest with coverage reporting in CI, here’s the game plan.

First, make sure to install the `@vitest/coverage-c8` dependency:

```bash
npm install -D @vitest/coverage-c8
```

And then update your `vitest.config.js` with:

```js
export default {
	test: {
		coverage: {
			provider: 'c8',
		},
	},
};
```

Finally, update your `ci.yml` file to gather coverage data:

```yaml
- run: npm run test:ci -- --coverage
```

Now your CI will also collect and report coverage information. One step closer to superhero status.

### 5. Conclusion

And just like that, you’ve injected a bit more confidence into your deploys. Good tests run automatically and give you feedback before your mistakes hit production. Plus, you'll look super pro in front of your team (or at least you won’t be the one causing the 3 A.M. outage… again).

There you have it: Vitest happily playing nice with a CI/CD pipeline via GitHub Actions. You can rest easy knowing that a robot watchdog has your back, ready to block anything that’s not up to testing snuff.
