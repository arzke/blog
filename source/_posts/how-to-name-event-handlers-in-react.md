---
title: How to name your event handlers in React
date: "2019-03-13T09:26:15.104Z"
tags: 
- react
- clean-code
description: There might be a better naming than handleClick
---

Code examples can be confusing and misleading, especially when we assume they're following some kind of best practices 
and that we can blindly rely on them.
But we must keep in mind that code samples are not meant to be clean, but rather to demonstrate something with the
minimal amount of code.

Therefore, considering that examples show us the right way to do something
is not relevant. One good illustration of this is how people usually name event handlers
in React. Here is a piece of code [you might be familiar with](https://reactjs.org/docs/handling-events.html):

```jsx
function handleClick() {
  // This function does something
}

function Foo() {
  return <button onClick={handleClick}>Foo</button>;
}
```

The name of the component and the label of the button have been
deliberately set to Foo because they're not relevant to the point I'm making here.

## So what is wrong with this example?

The problem here is that **handleClick doesn't reveal anything about what the function does**.
Instead, it tells us how the function is used. Still, it happens that when you ask someone why they name
their handler this way, the answer you get is "*everybody does this*". Unfortunately, [this cannot be considered a valid
argument](https://en.wikipedia.org/wiki/Argumentum_ad_populum).

You might remember from Uncle Bob's *Clean Code* the following rule:
> Write explicit code – naming variables and methods can reveal the entire intent of the code

Here, our code is not explicit. We know that *something* is happening when clicking the button, but we have
no idea *what* it is.

Another problem here is that we're tightly coupling the function to its caller, meaning that
if we decide to perform the same action on a different event, we have to
rename the function. This would make no sense as the body of the function hasn't changed.

Finally, what should we do if our component renders two buttons with different click handlers? How should we name them?

## The solution

Name your function after what it does.

```jsx
function clearForm() {
  // This function does something
}

function MySuperComponent() {
  return <button onClick={clearForm}>Foo</button>;
}
```

Event handlers are nothing more than functions, so here the same rules apply for the naming.
Receiving an event in their parameters does not make them any special.
If in the future your function does something different then it makes perfect sense
to rename it. 

## Conclusion

Finding a good name that reveals the intent of your code is often tricky. But using a
name that is too generic or doesn't answer the *what* is by no means better.
Everything that is said in this post might seem obvious, but I'm still surprised to see how many people keep using 
bad names for their handlers even in production code. This is a quick win.