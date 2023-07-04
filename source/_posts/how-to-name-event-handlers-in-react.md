---
title: How to name your event handlers in React
date: "2019-03-13T09:26:15.104Z"
tags: 
- react
- clean-code
---
When I started programming, my workflow consisted in the following:
1. Searching for the problem I was trying to solve on Google
2. Find some code sample on StackOverflow that seemed to achieve what I wanted to do
3. Copy/paste it

My codebase would eventually look like an inconsistent patchwork of differents samples put together, but it did the job. Even my personal projects would look as if different teams of developers worked on it.

But code snippets are just examples. People writing are not aware of or even care about your best practices. The sole purpose of these examples is to demonstrate how to solve a specific problem in a generic way. Using good variable names, a good code structure or something too specific is therefore not relevant in this context.

But it sometimes happens that a code snippet becomes widely popular, to the point you find it replicated as is in many different code bases.

One good illustration of this is how people usually name their event handlers in React. Here is a piece of code [you're certainly familiar with](https://react.dev/learn/responding-to-events) if you've ever used the library:

```tsx
function handleClick() {
  // This function does something
}

function UserSettingsForm() {
  return <button onClick={handleClick}>Clear form</button>;
}
```

## So what is wrong with this example?

The problem here is that **handleClick doesn't reveal anything about what the function does**.
Instead, it tells us how the function is being used. When you ask people why they name their handler this way, the answer you often get is "*everyone does this*". Unfortunately, [it's not because something is popular that it's right](https://en.wikipedia.org/wiki/Argumentum_ad_populum).

If you've read Uncle Bob's *Clean Code*, you might remember the following piece of advice:
> Write explicit code – naming variables and methods can reveal the entire intent of the code.

Here, our code is not explicit. We know that *something* is happening *when clicking the button*, but we have no idea *what* it is.

A second issue with this approach is that we're tightly coupling the function to the event triggering it. If we decide to perform the same action on a different event or use the same handler for two different types of events, we would have to rename the function despite not changing a single line of it.

Finally, what should we do if our component renders two buttons with different click handlers? How should we name them? `handleClick1` and `handleClick2` does not feel like the right solution.

## The solution

Name your function after what it does.

```tsx
function clearForm() {
  // We know that the form is cleared just by reading the function's name
}

function UserSettingsForm() {
  return <button onClick={clearForm}>Clear form</button>;
}
```

Event handlers are nothing more than functions, so there is no reason to follow different conventions for naming them.
Receiving an event in their parameters does not make them any special.

The only reasons you should be renaming your functions are:
- you found a better, clearer name for it
- you've changed its behavior

## The exception to the rule

We've seen when not to give a generic name to your handlers, but there are valid use cases where you'd want to do just that. I assume this is where the confusion comes from.

An example of this is when you create a component that would call a function it received in its props. In this case, we don't know what the function would do. We don't even care.

```tsx
function DeleteItemConfirmationModal({ onConfirm }: { onConfirm: () => void }) {
  return <div>
    <p>Are you sure you want to delete this item?</p>
    <button onClick={onConfirm}>Yes, delete this item</button>
  <div>;
}
```

## Conclusion

Finding a good name that reveals the intent of your code is sometimes difficult. But when you know upfront what a function does, make sure its name reflects its intent.