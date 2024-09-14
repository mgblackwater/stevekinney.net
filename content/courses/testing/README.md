---
modified: 2024-09-14T10:17:11-06:00
base: /courses/storybook
layout: contents
exclude: true
---

## Practical Applications

- [Writing tests: The very basics](the-basics.md)
- [Running your tests](running-tests.md)
- Exercise: [Basic Mathematics](basic-math.md)
	- Exercise: [Testing For Errors](testing-for-errors-exercise.md)
- [toBe or not toBe](to-be-or-not-to-be.md)
- [Understanding Assertions](vitest-assertions.md)
- [Asymmetric matchers](asymmetric-matchers.md)
- [Hooks: Setting Up and Tearing Down](setting-up-and-tearing-down-with-hooks.md)
	- Exercise: [Testing a reducer](testing-a-reducer.md)
- Great Expectations: Common matchers and assertions
	- Exercise: [Check the prefix](exercise-string-matching.md)
	- Exercise: [Ignore the ID](exercise-ignore-the-id.md)
	- Exercise: [Add the current date](exercise-add-timestamp.md)
- [Running a subset of your tests](filtering-tests.md)
- [Testing Asynchronous Code](testing-asynchronous-code.md)
	- [Testing promises](testing-promises.md)
- [Organizing and annotating your tests](organizing-and-annotating-tests.md)
- [Testing the DOM](testing-the-dom.md)
	- [Testing Library](testing-library.md)
	- [User Event](user-event.md)
	- Example: [Testing a Calculator with Testing Library](testing-a-calculator-dom.md)
- [Testing the Unhappy Path™](unhappy-path.md)
- [Throwing and catching](testing-errors.md)
- [Test Doubles](test-doubles.md)
	- Introduction to [Stubs](stubs.md)
	- Using [Spies](spies.md) in Vitest
		- [Spies Exercise](spies-exercise.md)
	- Creating [Mocks](mocks.md) with Vitest
		- [Clearing, Restoring, and Resetting Mocks](clearing-mocks.md)
		- [Mocking Modules and Imports](mocking-modules.md)
		- [Mocking Globals](mocking-globals.md)
		- [Mocking environment variables](mocking-environment-variables.md)
		- [Mocking APIs with Mock Service Worker](mocking-apis.md)
		- [Mocks Exercise](exercise-mocks.md)
		- [Best Practices and Common Pitfalls with Mocking](mocking-best-practices.md)
		- [Mocking Examples](mocking-examples.md)
	- Advanced: [Testing Time](mocking-time.md)
		- Exercise: [Time Since](exercise-time-since.md)
	- [Mock Service Worker](testing-with-mock-service-worker.md)
	- [Handling Asynchronous Code with Mocks, Stubs, and Spies](async-mocking.md)
	- [Best Practices and Patterns](mocking-best-practices.md)
	- [Mini-Project](mocking-project.md)
	- [Overriding Object Properties](overriding-object-properties.md)
	- [Checking Arguments](checking-arguments.md)
	- [Mocking Dependencies](mocking-dependencies)
	- Example: [AST Walker](exercise-ast-walker.md)
	- Exercise: [`filterMap`](exercise-filter-map.md)
- [Expecting the expected](expect-assertions.md)

## Next Steps

- [Snapshot testing](snapshot-testing.md)
- Reporters
- Strategies for testing conditional logic
- [Setting up continuous integration](continuous-integration.md)
- [Code coverage](code-coverage.md)
- [Testing types](testing-types.md)
- Browser mode
- [Test context](test-context.md)
- [Testing workspaces in mono-repositories](workspaces.md)

## Ivory Tower

- [The structure of a unit test](structure-of-a-unit-test.md)
- The importance of testing
- [The types of tests: Unit, integration, end-to-end](types-of-tests.md)
- [Components of a test: Test runners and assertion libraries](test-runners-and-assertion-libraries.md)
- [How tests work](how-tests-work.md)
- Assert versus expect
