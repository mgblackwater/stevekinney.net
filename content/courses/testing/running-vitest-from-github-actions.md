---
title: Set Up Vitest With GitHub Actions
description: Learn how to set up Vitest testing with GitHub Actions.
modified: 2024-09-28T11:31:15-06:00
---

## Set Up Vitest with GitHub Actions

Alright, folks. You've written your glorious Vitest tests, and now you're thinking, "I need these bad boys running on every push, like a well-oiled machine." Enter GitHub Actions—a hands-free testing butler that does your bidding with every PR. Let’s get Vitest hooked into your GitHub Actions workflow. It’s easier than you might think.

### Step 1: GitHub Actions 101

In case you’re new to Actions, here's how it works. You basically create a `.yml` file, tell it what you want it to do—like run your Vitest tests—and GitHub takes care of the rest. You can watch your CI pipeline run straight from the repo’s "Actions" tab, which makes debugging failure a little less soul-crushing.

### Step 2: Create Your Action Workflow

First off, you need to create a directory where GitHub Actions will look for it. In the root of your project, navigate to:

```bash
mkdir -p .github/workflows
```

Now, create a workflow file. Let’s call it `ci.yml`, and open it up:

```bash
touch .github/workflows/ci.yml
```

This `.yml` file is your workflow config. Think of it like giving GitHub a playbook for what it should do when someone pushes to the repo or opens a pull request.

### Step 3: Configure the Workflow

Here comes the boilerplate. Hey, it might look like a lot, but most of it’s copy-paste—once you set it up, you won’t have to think too hard about it again.

```yaml
name: CI
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - '*'

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Setup Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '16'

      - name: Install dependencies
        run: npm install

      - name: Run Vitest
        run: npm run test
```

Let's break it down:

1. **name**: The name of your workflow. Think of it like a title, so you know what’s going on when looking at GitHub Actions logs. We’re boring here—just calling it "CI".
2. **on**: This is where you specify when you want the workflow to run. We’re saying, "Run my tests when someone pushes to `main` OR when a pull request is made against _any_ branch."
3. **jobs**: This is where the magic happens—this job runs your test suite.
   - **runs-on**: This specifies the virtual machine that will run the job—in our case, an Ubuntu VM.
4. **steps**: These are the actions performed during the job:

   - Checkout the code from your repo.
   - Set up Node.js (make sure you’re matching whatever version you're using locally, FYI).
   - Install dependencies with `npm install`.
   - Run your Vitest suite, which assumes you’ve got something like a `test` script in your `package.json`, as in:

     ```json
     {
     	"scripts": {
     		"test": "vitest run"
     	}
     }
     ```

### Step 4: Commit and Push

Alright. Save that `.yml` file, commit it, and push it up to your `main` branch—or whichever branch you’re working on.

```bash
git add .
git commit -m "Set up GitHub Actions for Vitest"
git push origin main
```

### Step 5: Watch the Magic

Once that’s all up, head to your repo on GitHub, click on the **Actions** tab, and you should see your workflow running! If everything’s wired up right, it’ll install your dependencies and run your tests against every push and PR. If not, you’ll be digging through logs, but hey, that's part of the journey, right?

### Conclusion

There you have it—Vitest running every time you push to GitHub like clockwork. Now every time your tests pass, you can breathe a little easier knowing that automation’s got your back. Did I hear someone say “merge with confidence”?

Who knew that writing tests could feel so…good?

Now go forth, and keep shipping that bug-free code. Or at least fewer-bug code. 😉

```ts

```
