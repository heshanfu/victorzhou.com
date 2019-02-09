---
title: Can You Find The Bug in This Code?
date: "2019-02-09T12:00:00.000Z"
template: "post"
draft: false
slug: "/blog/a-javascript-bug-i-had-once"
img: "/media/laptop-code.jpg"
category: "Javascript"
tags:
  - "Javascript"
description: "Fun Fact: this is an actual bug I had once."
---

Here's a bit of Javascript that prints "Hello World!" on two lines:

```javascript
(function() {
  (function() {
    console.log('Hello')
  })()

  (function() {
    console.log('World!')
  })()
})()
```

…except it fails with a runtime error. Can you spot the bug without running the code?

Scroll down for a hint.

<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />

## Hint

Here's the text of the error:

```
TypeError: (intermediate value)(...) is not a function
```

What's going on?

Scroll down for the solution.

<br />
<br />
<br />
<br />
<br />
<br />
<br />
<br />

## Solution

One character fixes this code:

```javascript
(function() {
  (function() {
    console.log('Hello')
  })(); // highlight-line

  (function() {
    console.log('World!')
  })()
})()
```

Without that semicolon, the last function is interpreted as an argument to a function call. Here's a rewrite that demonstrates how the code is run without the semicolon:

```javascript
(function() {
  const f1 = function() { console.log('Hello'); };
  const f2 = function() { console.log('World!'); };

  f1()(f2)();
})()
```

There are 3 function invocations here:

- `f1` is called with no arguments
- The return value of `f1()` is called with `f2` as its only argument
- The return value of `f1()(f2)` is called with no arguments

Since calling `f1()` doesn't return a function, the runtime throws a `TypeError` during the second invocation.

## Wait, you had this bug once?

Yup.

## Why would you ever write code with so many Immediately Invoked Function Expressions ([IIFE](https://developer.mozilla.org/en-US/docs/Glossary/IIFE))?

It's a long story, but I'll make a post soon explaining how I wrote bad enough code to have this bug. [Subscribe](http://eepurl.com/gf8JCX) (no spam, for real) if you want to get notified when it's published!

## The Lesson

**Always use semicolons**. This specific case was a bit contrived, but something similar could happen to you. Here's another Hello World program that fails for a related reason:

```javascript
const a = 'Hello'
const b = 'World' + '!'
[a, b].forEach(s => console.log(s))
```

I'll leave figuring this one out as an exercise for you.

Most Javascript style guides require semicolons, including [Google's](https://google.github.io/styleguide/jsguide.html#formatting-semicolons-are-required), [Airbnb's](https://github.com/airbnb/javascript#semicolons), and [jQuery's](https://contribute.jquery.org/style-guide/js/#semicolons). To summarize: **always use semicolons**. 