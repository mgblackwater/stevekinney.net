---
modified: 2024-09-14T10:20:30-06:00
tags: [s**:]
---

Code coverage is useful for identifying how much your code is (or _isn't_) covered by tests. This can be useful for identifying blind spots and potential edge cases that are not covered by your test suite.

**A word of caution**: Aiming for 100% coverage—or, worse _mandating_ it—isn't the best use of your time and attention:

1. Consider the 80/20 principle, that last little bit of coverage is usually a lot more work than the majority of it. And frankly, you can hit the point of diminishing returns pretty quickly. Maybe you're better off with an integration test?
2. Speaking of integration tests: It's rare that any code coverage tool takes a holistic few of your application and its code. Usually, it's able to tell you about the coverage that one kind of test—typically your unit tests—provide. This means, that your code _could_ very well be covered by some other kind of test—or even your type system.

I hesitate to mandating a given number. If you do, keep it low. Sure, I'd say like less that 60% means you should probably pay some attention to your tests. Alternatively, you could choose to just monitor that a given change doesn't drastically reduce the amount of code coverage.

For me, the biggest advantage is to help as I'm working on a new function or feature. Code coverage allows me to see where I still need to add some tests and allows me get a high-level few as I'm working on something new.

## Installing a Code Coverage Tool

If you _don't_ have a coverage reporter installed, Vitest will prompt you to install the dependency.

```ts
> vitest exercise.test.ts --coverage

 MISSING DEP  Can not find dependency '@vitest/coverage-c8'

✔ Do you want to install @vitest/coverage-c8? … yes
```

## Running the Code Coverage Tool

You can do this via:

```ts
npm test -- --coverage
npx vitest --coverage
```

You'll likely get a new `./coverage` directory. Go take a look. You can spin up a quick web server using:

```ts
vite preview  --outDir coverage
```

This will allow you see where you code is _not_ being tested. (Source: [The documenation for c8](https://github.com/bcoe/c8#ignoring-uncovered-lines-functions-and-blocks).)

## Ignoring Lines

You can ignore lines from your coverage report:

```ts
const something = 'lol';
/* c8 ignore next */
if (process.platform === 'win32') console.info('hello world');

/* c8 ignore next 3 */
if (process.platform === 'darwin') {
  console.info('hello world');
}

/* c8 ignore start */
function dontMindMe() {
  // ...
}
/* c8 ignore stop */
```

## Configuring Your Coverage Report

You can add a `coverage` key to the `test` configuration in your `vitest.config.ts`:

```ts
import path from 'node:path';
import { defineConfig, defaultExclude } from 'vitest/config';
import configuration from './vite.config';

export default defineConfig({
  ...configuration,
  resolve: {
    alias: {
      ...configuration?.resolve?.alias,
      test: path.resolve(__dirname, './test'),
    },
  },
  test: {
    globals: true,
    setupFiles: path.resolve(__dirname, 'test/setup.ts'),
    exclude: [...defaultExclude, '**/*.svelte**'],
    environmentMatchGlobs: [
      ['**/*.test.tsx', 'jsdom'],
      ['**/*.component.test.ts', 'jsdom'],
    ],
    coverage: {
      include: ['src/**/*'],
      exclude: [
        'test/**',
        'vite.*.ts',
        '**/*.d.ts',
        '**/*.test.{ts,tsx,js,jsx}',
        '**/*.config.*',
        '**/snapshot-tests/**',
        '**/*.solution.tsx',
        '**/coverage/**',
      ],
      all: true,
    },
  },
});
```

You can see all of the options [here]([GitHub - bcoe/c8: output coverage reports using Node.js' built in coverage](https://github.com/bcoe/c8#cli-options--configuration)).

The cool one here is the ability to set thresholds at which your build will fail if you dip below a certain amount.

```ts
statements: 54.92,
thresholdAutoUpdate: true,
```

These options will stop you from dropping at the very least and if you go up, it sets that as the new baseline.

## Longer Version

## Code Coverage Analysis

### Code Coverage in Vitest: A Comprehensive Guide

Code coverage is a metric used to measure the proportion of your source code that is executed during testing. High code coverage can increase confidence in your tests and help identify untested parts of your application. When using **Vitest**, a fast and modern JavaScript testing framework, you can easily integrate code coverage analysis into your testing workflow.

This post explores how to set up and use code coverage in Vitest. We'll discuss the importance of code coverage, how to configure it, interpret the results, and best practices to maximize its effectiveness.

#### What is Code Coverage?

Code coverage quantifies the amount of code executed during automated tests. It provides insights into:

- **Statements Coverage**: Percentage of executed statements.
- **Branches Coverage**: Percentage of executed control flow branches (e.g., `if`/`else` blocks).
- **Functions Coverage**: Percentage of executed functions or methods.
- **Lines Coverage**: Percentage of executed lines in the source code.

#### Why Use Code Coverage?

- **Identify Untested Code**: Highlights areas of the codebase not covered by tests.
- **Improve Test Quality**: Encourages writing comprehensive tests.
- **Maintain Code Health**: Helps prevent regressions by ensuring critical code paths are tested.
- **Metric for Progress**: Serves as a measurable indicator of testing efforts.

#### Setting Up Code Coverage in Vitest

Vitest uses **C8**, a code coverage tool based on **V8 JavaScript engine's** built-in code coverage capabilities. Here's how to set it up.

##### 1. Install Vitest (if Not Already installed)

```bash
npm install --save-dev vitest
```

##### 2. Configure Vitest

Create a `vitest.config.js` file in your project's root directory if you haven't already.

```javascript
// vitest.config.js
import { defineConfig } from 'vitest/config';

export default defineConfig({
  test: {
    // Configuration options go here
  },
});
```

##### 3. Enable Code Coverage

You can enable code coverage by adding the `--coverage` flag when running Vitest.

```bash
npx vitest --coverage
```

Alternatively, update your `vitest.config.js` to include coverage options:

```javascript
// vitest.config.js
export default defineConfig({
  test: {
    coverage: {
      provider: 'c8', // or 'istanbul'
      reporter: ['text', 'html'],
      // include: ['src/**/*.{js,jsx}'], // Specify files to include
      // exclude: ['node_modules'], // Specify files to exclude
    },
  },
});
```

**Explanation:**

- **`provider`**: Specifies the coverage provider (`'c8'` is the default).
- **`reporter`**: Determines the output formats (e.g., `'text'`, `'html'`, `'lcov'`).
- **`include`/`exclude`**: Controls which files are included or excluded from coverage analysis.

##### 4. Run Tests with Coverage

Execute Vitest with the coverage option:

```bash
npx vitest --coverage
```

This command runs your tests and generates a coverage report.

#### Interpreting Coverage Reports

Vitest generates coverage reports in the specified formats. Commonly used reporters include:

- **Text**: Outputs coverage summary to the console.
- **HTML**: Generates an interactive HTML report.
- **LCOV**: Produces an `lcov.info` file, useful for integrating with coverage services.

##### Console Output Example

```bash
---------------|---------|----------|---------|---------|-------------------
File           | % Stmts | % Branch | % Funcs | % Lines | Uncovered Line #s
---------------|---------|----------|---------|---------|-------------------
All files      |    85.7 |    66.66 |      80 |    85.7 |
 src           |     100 |      100 |     100 |     100 |
  math.js      |     100 |      100 |     100 |     100 |
 src/utils     |   66.66 |       50 |      50 |   66.66 |
  helper.js    |   66.66 |       50 |      50 |   66.66 | 12-14
---------------|---------|----------|---------|---------|-------------------
```

**Explanation:**

- **% Stmts**: Percentage of statements executed.
- **% Branch**: Percentage of branches executed.
- **% Funcs**: Percentage of functions executed.
- **% Lines**: Percentage of lines executed.
- **Uncovered Line #s**: Lines that were not executed during tests.

##### HTML Report

The HTML report is generated in the `coverage` directory by default. Open `coverage/index.html` in your browser to view it.

**Features:**

- **Interactive Navigation**: Browse coverage details for each file.
- **Highlighted Code**: See which lines are covered (green) and which are not (red).
- **Detailed Metrics**: Provides line-by-line coverage information.

#### Best Practices for Code Coverage

##### 1. Aim for Meaningful Coverage, Not Just High Percentages

- **Quality Over Quantity**: High coverage does not guarantee good tests. Focus on writing meaningful tests that cover critical code paths.
- **Avoid Coverage for Its Own Sake**: Don't write superficial tests just to increase coverage numbers.

##### 2. Identify and Prioritize Critical Code

- **Core Functionality**: Ensure that essential functions and business logic are thoroughly tested.
- **Edge Cases**: Write tests for boundary conditions and error handling paths.

##### 3. Exclude Generated or External Code

- **Configuration Files**: Exclude files like configuration or setup files that don't need testing.
- **Third-Party Libraries**: Exclude `node_modules` or other external code from coverage.

**Example: Exclude in Configuration**

```javascript
// vitest.config.js
export default defineConfig({
  test: {
    coverage: {
      exclude: ['node_modules', 'src/setupTests.js'],
    },
  },
});
```

##### 4. Regularly Review Coverage Reports

- **Identify Gaps**: Use reports to find untested code areas.
- **Refactor Tests**: Improve existing tests to cover missing branches or statements.

##### 5. Integrate Coverage into Continuous Integration (CI)

- **Automate Coverage Reporting**: Run coverage analysis as part of your CI pipeline.
- **Set Coverage Thresholds**: Fail the build if coverage drops below a certain percentage.

**Example: Setting Coverage Thresholds**

```javascript
// vitest.config.js
export default defineConfig({
  test: {
    coverage: {
      statements: 80,
      branches: 75,
      functions: 80,
      lines: 80,
    },
  },
});
```

##### 6. Use Coverage Badges in Documentation

- **Visibility**: Display coverage status in your project's README file.
- **Motivation**: Encourages contributors to maintain or improve coverage.

#### Common Pitfalls and How to Avoid Them

##### 1. Misinterpreting Coverage Metrics

**Issue**: Assuming that 100% coverage means no bugs.

**Solution**:

- Understand that coverage metrics indicate which code is executed, not whether the code behaves correctly.
- Focus on writing meaningful tests that assert correct behavior.

##### 2. Overlooking Uncovered Branches

**Issue**: Missing coverage for conditional branches.

**Solution**:

- Examine branch coverage to identify untested `if`/`else` conditions.
- Write tests that cover different logical paths.

##### 3. Ignoring Integration and End-to-End Tests

**Issue**: Relying solely on unit tests for coverage.

**Solution**:

- Incorporate integration and end-to-end tests to cover interactions between components.
- Use tools like **Playwright** for end-to-end testing.

##### 4. Including Unnecessary Files

**Issue**: Coverage reports include files that should be excluded, skewing metrics.

**Solution**:

- Use `include` and `exclude` patterns in your configuration to focus on relevant files.

##### 5. Not Cleaning Up Before Coverage Runs

**Issue**: Old coverage data persists, causing inaccurate reports.

**Solution**:

- Ensure that the coverage directory is cleaned before generating new reports.
- Use scripts to automate cleanup.

#### Examples in Practice

##### Example 1: Testing a Simple Module

**math.js**

```javascript
// src/math.js
export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}
```

**math.test.js**

```javascript
// tests/math.test.js
import { expect, test } from 'vitest';
import { add, subtract } from '../src/math';

test('adds numbers correctly', () => {
  expect(add(2, 3)).toBe(5);
});

test('subtracts numbers correctly', () => {
  expect(subtract(5, 3)).toBe(2);
});
```

**Run Tests with Coverage**

```bash
npx vitest --coverage
```

**Expected Coverage Report**

- Both `add` and `subtract` functions are covered.
- Coverage should be 100% for statements, branches, functions, and lines.

##### Example 2: Identifying Uncovered Code

**helper.js**

```javascript
// src/utils/helper.js
export function greet(name) {
  if (!name) {
    return 'Hello, Stranger!';
  }
  return `Hello, ${name}!`;
}
```

**helper.test.js**

```javascript
// tests/helper.test.js
import { expect, test } from 'vitest';
import { greet } from '../src/utils/helper';

test('greets a named person', () => {
  expect(greet('Alice')).toBe('Hello, Alice!');
});
```

**Issue**

- The case where `name` is not provided is not tested.
- Coverage report will show incomplete branch coverage.

**Solution**

Add a test for the missing branch:

```javascript
test('greets a stranger when no name is provided', () => {
  expect(greet()).toBe('Hello, Stranger!');
});
```

#### Integrating Coverage with Continuous Integration (CI)

Automate coverage analysis in your CI pipeline.

**Example: GitHub Actions Workflow**

```yaml
# .github/workflows/ci.yml
name: CI

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
      - name: Install dependencies
        run: npm install
      - name: Run tests with coverage
        run: npx vitest --coverage
      - name: Upload coverage report
        uses: actions/upload-artifact@v2
        with:
          name: coverage-report
          path: coverage
```

**Explanation:**

- The workflow runs tests with coverage on each push or pull request.
- Coverage reports are uploaded as artifacts for review.

#### Conclusion

Code coverage is a valuable tool for assessing the effectiveness of your tests. By integrating coverage analysis into your Vitest testing workflow, you can gain insights into untested code, improve test quality, and maintain a robust codebase. Remember that while coverage metrics are helpful, they should be used alongside other quality measures to ensure comprehensive testing.

#### Key Takeaways

- **Enable Coverage**: Use `--coverage` flag or configure in `vitest.config.js`.
- **Interpret Reports**: Understand statements, branches, functions, and lines coverage.
- **Aim for Meaningful Coverage**: Focus on writing effective tests rather than achieving arbitrary coverage percentages.
- **Use Coverage Tools**: Leverage coverage reports to identify gaps and improve tests.
- **Integrate with CI**: Automate coverage analysis in your continuous integration pipeline.
- **Exclude Irrelevant Files**: Configure inclusion and exclusion patterns to focus on relevant code.

By following these practices, you can effectively utilize code coverage in Vitest to enhance the quality and reliability of your applications.

#### Additional Resources

- **Vitest Documentation**: [Vitest Coverage Guide](https://vitest.dev/guide/coverage.html)
- **C8 Coverage Tool**: [c8 on npm](https://www.npmjs.com/package/c8)
- **Understanding Code Coverage Metrics**: [Istanbul.js Documentation](https://istanbul.js.org/docs/advanced/coverage-object/)
