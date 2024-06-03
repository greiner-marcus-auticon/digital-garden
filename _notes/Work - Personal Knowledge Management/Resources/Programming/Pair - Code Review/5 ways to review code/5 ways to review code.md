---
title: 5 ways to review code
feed: show
---

[https://medium.com/volvo-cars-engineering/5-ways-to-review-code-without-wasting-everyones-time-aedeecc51094](https://medium.com/volvo-cars-engineering/5-ways-to-review-code-without-wasting-everyones-time-aedeecc51094)

![[assets/1OJAMs5rD0VtH9K8PlIZtdA.png]]

Icon credit: GitHub, Volvo Cars

![[assets/1z-MpdUQwngYuV2ebCfLTng.png]]

As an iOS developer, I have reviewed thousands of lines of code from my peers. Like programming, reviewing code makes you feel either smart or utterly stupid.

This is because of the main issues with code reviews:

1. People don’t really know why they review code.

2. People don’t review the right things.

3. The Pull Requests are not reviewable.

In this article, we focus on **what and how** we should review. It’s part two of an article series explaining [why](https://medium.com/volvo-cars-engineering/part-1-why-do-we-review-code-ba4c662d8b49), what and how to review code.

## What to review in a Pull Request

We will look at the 5 key areas when reviewing pull requests, both for **what** needs to be reviewed and **how** it should be reviewed.

![[assets/1OJAMs5rD0VtH9K8PlIZtdA 1.png]]

## 1. Agreements

## Breaking down the reviewable sections

If you have read “[why do we review code](https://medium.com/volvo-cars-engineering/part-1-why-do-we-review-code-ba4c662d8b49)”, you know why we do code reviews: getting feedback, clarity, and making sure what you understand, can be understood by others.

This gives the context of how to review each section of the code. An easy guess would be that simply going through the changes, and making sure they are clear and coherent as a whole, is good enough.

But is it?

Knowing how to review each area of the code doesn’t make the review **great**. What does is to know in the first place which areas need to be reviewed.

The key concept to break them down is to look at the **agreements** within your team, guild, or organization.

If you’ve worked for more than one team (or company), you probably know how every team has different requirements.

## Agreement types

Some examples of agreements within a team of developers are:

- A set amount of test coverage (unit tests, screenshot tests, etc.) for every change.
- Creating [ADRs](http://adr.github.io/) (_Architectural Decision Records_) when proposing architectural changes.
- Documenting your Software Design.
- Agreeing on certain architectures and patterns.
- Having a golden path of best practices that all of you should follow (which probably includes some of the previous items).

A quick check on the team agreements is one of the best pieces of feedback a developer can get in a code review. Did the developer who created the Pull Request miss any of them?

## The annoying reviewer

A fantastic example of this is that annoying colleague who just adds the same comment in any Pull Request: “You should add some (more) unit tests”.

When getting this comment, two things happen.

First, your neck veins thicken, your face turns red, and you smash your keyboard against your screen.

Then you add the tests.

At the end of the day, the only thing that lives in your Git history is how you added the tests, which makes the comment from the reviewer extremely valuable.

So instead of smashing the keyboard, buy them a coffee. They will appreciate it and make sure you never forget adding tests again.

Picture by [Pragyan Bezbaruah](https://www.pexels.com/@pragyanbezbo/)

![[assets/12g78p-c_px-xendG3jUH7A.png]]

## 2. Getting a second pair of eyes

Even when you review your own code, you might have missed something.

It can be that you renamed a variable right before pushing the code and now, in different areas of the code, a single concept has multiple names.

Or it can be that you always make the same typo in the same place. Like the word “acquiesce”, which many people don’t know because they haven’t been able to find it in the dictionary.

It can also be that the _smart_ solution you came up with, to solve a problem, was completely unreadable.

Having a second pair of eyes to look at your work can help you identify mistakes and confusing areas that aren’t obvious to you. This is one of the biggest benefits from reviewing code against pair programming (which has its own advantages, don’t get me wrong): the lack of context within a code review will make you look at the code without knowing as much as the person who wrote it.

Something else worth remembering is that, as a person who reviews someone else’s code, you are responsible for raising your voice when you don’t understand what you’re reading.

Don’t be shy to share what you think, that’s why you’re part of the review. That said, you not understanding the code doesn’t mean it’s bad. Ask for clarification from the person who wrote it, and they might give you the input you need to understand it. If that’s not the case, as a reviewer you’re the right person to ask for changes.

## 3. Ask questions and give feedback

No matter how much experience you have, eventually, there’s someone who knows less than you.

This puts you in a privileged position where you can help someone grow as a developer. You can question why something was done as it was, suggest alternatives, and point out design flaws that can affect the overall quality of the code change.

Even when others have more experience than you, you can take the chance to learn.

Asking about why an architectural decision was taken can help you improve on your future decisions. Or it can help the other person realise that maybe they over-engineered their solution, by building a very well defined architecture that adds more noise than value.

This makes your feedback consistently relevant, no matter how experienced the developers that created the Pull Request are.

## 4. Say no

From time to time, you will find a Pull Request that just can’t be reviewed.

![[assets/1kGhBlQd13VNQOOhHJGjfiQ.png]]

Here’s some of the common reasons:

- You don’t understand anything of what you’re reading when checking the code.
- There’s a high number of changes.
- There is no context of what you are supposed to review.

You can ask the developer who created the Pull Request for clarification and help, but ultimately you can always say no.

One of your responsibilities as a reviewer is to stop bad code from being delivered. If you cannot understand the code how will anyone be able to maintain it in the future?

Make sure the other developer understands your reasons for saying no and bring up suggestions for how to fix the problem.

In the examples listed above, these could be some of the suggested solutions:

- Asking for a session where both you and the developer who wrote the code go through it. Maybe a few name changes for variables or classes can help you understand what’s going on.
- Asking the developer to break down the content in smaller Pull Requests.
- Ask for a description in the Pull Request, including videos or pictures of how the change will look, software diagrams, etc.

## 5. Don’t be rude

Even when you’re looking at code, what you review has been built by **people**.

When people ask for review, they don’t want to receive comments trashing their work, but constructive feedback on how to improve. Nobody tries to create bad code, they might be under time pressure, be dealing with personal problems or be less experienced than you.

There’s many reasons for bad code to appear, and the more code you review the more code you will find that is, to say the least, not great.

Understanding that we’re dealing with people is key to know how to write valuable comments. Ask challenging questions, bring up suggestions when asking for changes, and maybe reach the developer outside the Pull Request and ask them to go through the code together.

Most of the times, the easiest way to understand if the comment you’re adding is destructive is to ask yourself if you’d like to receive it if you had created that Pull Request.

A common trigger for destructive comments are strong opinions. If something you read is annoying you, try to assume it’s the right call and then, after a pause, try to figure out why it has been implemented. You don’t need to block a change because it’s not how you would do it, but because it’s objectively bad code.

Software design has many right and wrong solutions, not just the ones you would build. Try to remove your bias towards your own solutions. This will also give you the chance to learn from different ways to solve similar problems, which will make you a better software developer.

## Summary

Here’s a list of the key points I would like you to remember next time you review code:

- Validate agreements (i.e. is there unit tests? is there documentation?).
- Offer another pair of eyes (i.e. check for typos, identify confusing areas).
- Ask questions when you don’t understand something and offer feedback if you’d like to change something.
- Say no to the Pull Request if it’s either unclear or unreviewable.
- Don’t be rude when writing comments.

## The last tip

These are just some of the highlights on what and how to review code, but there is one piece of advice that will make you great at reviewing code.

And that is to review code.

As I mentioned “[why do we review code](https://medium.com/volvo-cars-engineering/part-1-why-do-we-review-code-ba4c662d8b49)”, writers and programmers deal with similar challenges. They need to create drafts, ask for feedback and write. The more they do it, the better they become.

This applies both to writing code and reviewing it. And also to juggling, playing the guitar, or eating sausages very, very fast. As any skill, repeated practice is what will make you great at it.

## Support us

You can follow [Volvo Cars Engineering](https://medium.com/volvo-cars-engineering) if you want to receive updates on when there are more articles available. By doing it, you also support many engineers to share insights and learnings.
