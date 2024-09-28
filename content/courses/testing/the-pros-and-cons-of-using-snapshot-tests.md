---
title: The Pros And Cons Of Using Snapshot Tests
description: Insights into when to use and avoid snapshot testing in development.
modified: 2024-09-28T11:31:14-06:00
---

## The Pros and Cons of Using Snapshot Tests

Alright, let's talk snapshot testing! It's kind of like taking a picture of your code—or more accurately, your UI—at a certain point in time, and then any time you change something, you compare the new "picture" to the old one. **Sound easy? It is.** Sort of. But like most tools in our developer toolbox, snapshot tests have their pros and cons. So, let’s break down when they’re worth adding to the arsenal and when they can be… well, **troublesome**.

### The Pros of Snapshot Tests

#### Fast Setup, Quick Wins

The biggest allure of snapshots is how fast you can get up and running. No complex assertions to write, no digging through code to make sure all conditions are met. You just run your test, take a snapshot of the component, or some function output, and boom—you’ve got a test. Next time it runs, if there's a change, it’ll compare the new output to the snapshot and let you know.

It’s the “low-hanging fruit” of testing. The effort is minimal while the initial benefit can be impressive.

#### Great for **UI** Components

When you're testing *how* a UI component looks, snapshots can be a lifesaver. Instead of writing tons of specific tests that inspect each individual property of the component (👀 looking at you, `toEqual` madness), snapshots let you capture the full output and future-proof it. If someone changes the component's rendering, the snapshot will quickly let you know.

For a developer who just rage-quit tweaking some `div` styling for the hundredth time 🤯, a snapshot test can feel like a warm blanket of peace.

#### Catching Unintended Changes

Sometimes you, or someone on your team, makes a "small" change that somehow impacts a completely unrelated part of the app. Classic. However, if you’ve sprinkled snapshot tests throughout your components or functions, you might catch the fallout before the code sees daylight. If a button subtly changes shape or a CSS class disappears, snapshots will catch it for you.

### The Cons of Snapshot Tests

#### Too Easy to Ignore

Okay, so the test fails. What do you do? Look at the diff! No, don’t get lazy on me.

Over time, you may start to just glance at changes and, in a moment of haste, hit `u` (in Vitest/Jest, that's the key to *"update the snapshot please and thank you"*) without checking whether the change is valid. After all, updating the snapshot is easy, right? And who has time to analyze a 200-line snapshot of some absurd UI tree? Well… **sometimes you should**.

If you're snapshotting massive components, you may find yourself ignoring the exact kind of unexpected changes this test should be catching. This leads nicely into the next point…

#### Large Snapshots Are Overwhelming

If you’ve ever seen a snapshot with hundreds of lines of JSX or DOM structure, you know what I’m talking about. You’re stuck scrolling through an endless diff of props and deeply nested elements trying to figure out why the snapshot even *exists* at this point. **Don’t do that to yourself**.

Snapshots really start to lose their value when they become unwieldy. They morph into noise, obscuring the signal. And if you start snapshotting **everything**—just because you can—you’ll spend more time managing the snapshots than writing the code.

#### Brittle When Overused

Let’s be honest here: snapshots **aren’t** smart. They do an excellent job of telling you "something changed," but they can’t tell you "what changed matters."

This creates a situation where any kind of refactoring or even minor layout tweak can trigger a pile of failed snapshots. And because the assertion isn’t super specific, these tests can start breaking all over the place. Overusing snapshots can lead to a lot of frustration and maintenance, especially when updates happen rapidly across large teams.

### Use Them Wisely, Use Them Sparingly

So, snapshot tests: they’re a useful tool in the toolbox, but they’re not a hammer. Not every problem—or component—needs a snapshot solution.

Sure, write a snapshot test for simple components or sections of the UI where you’re genuinely interested in flagging unwanted changes. But… **balance it out** with specific tests for logic and behavior. And for the love of all things JavaScript, avoid snapshotting enormous components unless you’re a glutton for punishment.

In the end, snapshot testing is like that one office chair that’s super comfy—until Thanksgiving happens… and then you're just kind of stuck.

Just be thoughtful in how you use snapshots, and they'll serve you well.

```ts
```
