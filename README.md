# Recursion Workshop

## Objectives
Student will be able to:
* Use terminology like "stack", "stack trace", "stack frame"
* Identify a recursive function
* Identify the base case in a recursive function and explain why a base case is needed
* Identify the recursive case
* Basically, not be totally intimidated or mystified by recursion and see that it works via the same principles we already know (functions call functions, wait for the calls to evaluate to a return value)

## Prerequisites
Get a big stack of post-it notes and/or index cards. At the start of the lecture ask that each student take about 10. Ask that students bring a pen/ pencil/ writing instrument of some kind.

## Slides
* [Slide Deck](https://docs.google.com/presentation/d/1fhdnYFvSZqRJXw0hMNlT-dVNDV0Iqdv9HyJIGWeGXNc/edit#slide=id.p)

## Notes

The slides start not by talking about recursion at all, but just trace through, _in excruciating detail_, normally executing ruby code

### Slide 2
Define stack trace. This is something students have seen all the time. A method in file A calls a method in file B which calls another method in file B which then errors on line X. Students have probably scanned through a big stack trace to find a file that contains code they wrote vs. internal library code files

### Slides 3 - 27
In the following slides we trace through a method being called. (Apologies for incorrectly camelCasing ruby method_names)

Example talking points:
* **Slide 4:** "I run `ruby some_file.rb`, ruby starts executing the code. It sees this method invocation so it has to go find where that method is defined"
* **Slide 5:** "The original method call _has not disappeared from the stack; Ruby is waiting to figure out what the return value is_. This is the point to make over and over again here. A method is called, and ruby has to go figure out the return value before it does anything else"
* **Slides 8-9; 16-17 etc:** "Remember that in Ruby _we are always sending a message to an object_. Even `1 + 1` is really calling the `+` method on the instance of the `Integer` class `1` ie `1.+(1)`" (The `class Integer` slides are a useful mental model, it's not exactly implemented like that)

### Slides 28 - 29
Still not talking about Recursion.
**Q**: If the `*` operator did not exist, how would you implement your own version of `multiplication`? What is 6 * 4?

### Slide 30
**A**: Here's an iterative version of `multiplication`. Almost without fail a group will propose exactly this implementation.

### Slide 31
Ok, finally talking about recursion. I specifically avoided `factorial` because it seems like every intro to recursion thing in the world starts with factorial.  This is a tough question to pose to students. The goal would be to get them to "describe multiplication in terms of itself", or "what's the absolute laziest way I could describe the process of multiplication?", "6 x 4 is really just 6 + ________ ?" where the goal is to fill in that blank as simply as possible. Generally you have to very much steer this convo.

### Slides 32-36
Break down a recursive model of multiplication. Stress that you are always trying to describe the problem in the simplest terms possible until something that is so easy you can just solve directly.

### Slides 37-38
Ok, we'll _directly translate_ that process we just described into code. There's a _leap of faith_ involved in recursion. It's weird. We're like trusting the method we are writing will work even though we have not finished writing it yet. Crazy.

### Slides 39 - 53
### Post-it note exercise
In the name of trusting the recursive process, it's useful to trace it through in detail. In this exercise we are basically pretending we are the ruby interpreter stepping through the code line by line

Line by Line talking points:
* On the first post-it write `a = 6  b = 4` so we can keep track of what values each frame was called with.
* Did we pass that conditional? nope, draw an X. let's go to the `else`
* Ok, let's write `6 + __________`. How do we fill in that blank? Ruby doesn't know the answer so it has to go call the method to evaluate it. That's a method call so take your next post-it and place it _directly on top_ of the current post-it. The current post-it hasn't gone away, it's still there waiting for that blank to be filled in.
* On the next card write `a = 6  b = 3`, etc.
* Eventually when you get to the base case, your method finally returns something! You know how to fill in the blank of the card below. Take that post-it, crumple it up and throw it on the ground! Please be sure to clean up afterwards.

### Slides 54-56
A breakdown of the recursive process in general. We must have a base case to stop the recursion otherwise it's an infinite loop. The recursive case must _get closer to the base case_. If the base case is that our number is small, we should be making the number smaller.

### Reverse a String Exercise
Let's get some more practice determining what a base case is. Often, base cases are so simple that it's kind of hard to see.

**Q:** What's the base case for reversing a string recursively? How can we break down the process reversing a string into problems that have the same structure of the original?

**A:** A 1 or 0 character string is reversed. If you handed me the letter "a" asking me to reverse it, I would give it straight back to you and say "here you go, this is reversed"

Draw out this process in a text editor.

```
reverse("hello")
"o" + reverse("hell")
|      "l" + reverse("hel")
|      |      "l" + reverse("he")
|      |      |      "e" + reverse("h")
|      |      |      |      "h"
|      |      |      "eh"
|      |      "leh"
|      "lleh"
"olleh"     
```

### Additional Exercises

Usually, I like if they trace out one more exercise using the post-it notes. I think it's helpful. This could probably be completed outside of lecture time.

We discuss what the recursive insight is if we wanted to find the maximum element in an array recursively.

**Base Case:** With a 2 or less element array I could easily compare element a to element b and return the biggest.
**Recursive Insight:** Ok, so what if I cut my array in half and gave the first half to ${student A} and the last half to ${student B}. I told each to find the max element in their half and then give me back the largest from each and I would take it from there. Each one of those students might decide to follow the same pattern, giving half of their half to each student behind them, etc etc.


We sketch out the code, add a ton of `console.log`s, they step through with post-it notes. This is a cool one becuase the stack doesnt just pile up and then wind down, but piles up -> winds down -> piles up -> winds down in a few cycles.


```js
const recursiveFindMax = arr => {
  if (arr.length <= 2) {
    const a = arr[0]
    // useful trick to make this work for 2 or 1 elem arrays
    const b = arr[arr.length -1]

    return a > b ? a : b
  } else {
    const midpoint = Math.floor(arr.length / 2)

    // not the most space-efficient to create copies of the sub-arrays
    // but generally students are a bit more familiar with slice
    const firstHalf = arr.slice(0, midpoint)
    const secondHalf = arr.slice(midpoint)

    const firstMax = recursiveFindMax(firstHalf)
    const secondMax = recursiveFindMax(secondHalf)

    return firstMax > secondMax ? firstMax : secondMax
  }
}
```


### Wrap Up
A lot of intro to recursion problems do have simple iterative solutions. So it can often seem kind of silly to spend all the trouble. There are certain problems that lend themselves super well to recursion. Think of the DOM aka a tree. It has a recursive structure! Nodes have children, those children are nodes which have children, etc. You could easily navigate the DOM using recursion. Also look up "recursive backtracking" where the problem domains are so large that to exhaustively iteratively try every possibility is impractical.

The [exercises here](https://github.com/alexgriff/wdf_recursion_exercises) ramp up in difficulty significantly
