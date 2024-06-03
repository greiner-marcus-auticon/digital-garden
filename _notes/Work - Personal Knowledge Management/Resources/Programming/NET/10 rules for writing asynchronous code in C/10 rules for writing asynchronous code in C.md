---
title: 10 rules for writing asynchronous code in C
feed: show
---

[https://anthony-trad.medium.com/the-ultimate-10-rules-for-writing-asynchronous-code-in-c-3eb3ccaec1bc](https://anthony-trad.medium.com/the-ultimate-10-rules-for-writing-asynchronous-code-in-c-3eb3ccaec1bc)

![[assets/1P7c5-yVf4CEwaArJ0RfKCw.jpeg]]

Async await meme

![[assets/1P7c5-yVf4CEwaArJ0RfKCw 1.jpeg]]

## Introduction

Imagine you’re a chef in a kitchen full of ingredients, some fresh, some a bit past their prime, all thanks to Microsoft’s “_We never throw anything away_” policy. This is what programming asynchronously in C# is like — an overwhelming mix of new and old syntax all in one big pot.

This pot can turn to be a big ball of code mud, you could end up with the coding equivalent of food poisoning: thread starvation, deadlocks, or even the dreaded application crashes. Most of these problems boil down to a lack of understanding of the underlying concepts such as state machines, the thread pool, context switching etc...

I drafted a document for my team explaining those concepts and decided to narrow it down to a simple 10 rule cook book here as those are pretty common in the industry. There’s a lot more than 10 rules, feel free to let me know if you want the rest !

## 1) Do not use Async void

From all the rules, this one is the most critical potentially crashing your whole application. `async void` can’t be tracked and if any exception is triggered, it will crash the whole application.

In a previous project, this caused our whole API to crash randomly on production because of an `async void` present in a background service.

```Plain
public void BadAddUserAsync(User user); //You should not do this
```

```Plain
public Task GoodAddUserAsync(User user); //Do this instead!
```

## 2) Use ValueTask or Task.FromResult for easy computations

The below example is considered bad, wasting a thread pool thread to return something that does not need an async call.

```Plain
public Task<int> BadComputeDiscountAsync(User user) => Task.Run(()
 => 0.3 * user.BaskedAmount);
```

You should definitely use `ValueTask<T>` here to avoid using an extra thread AND save a minor Task allocation.

```Plain
public Task<int> BestComputeDiscountAsync(User user)
 => new ValueTask<int>(0.3 * user.BaskedAmount);
```
