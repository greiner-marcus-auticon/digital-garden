---
title: Art of code review
feed: show
---

[https://bharatmane.medium.com/the-art-of-code-review-b37ca8fa7ba6]( https://bharatmane.medium.com/the-art-of-code-review-b37ca8fa7ba6)

> A way to improve the product & self…

This article will share my experience with some of the best practices that I came across, which helped me improve my code reviews.

Code reviews are an integral part of the software development process. They help maintain code quality, identify/prevent bugs, styling, conventions, patterns, and are critical to learning. Unfortunately, code review is often overlooked as an ongoing practice during the development phase, but countless studies show it’s the most effective quality assurance strategy.

> At the end of the review process, now we have two people who know what the code does. This means they can cover for each other.

I would always prefer [pair programming](https://en.wikipedia.org/wiki/Pair_programming) as the first choice; it has immense benefits. In pair programming, the focus is on developing “**the good**” code the **first time**. We do it by having more than one pair of eyes looking at code as we write. In addition, it has **the active involvement** of the guys at work.

A code review, in contrast, involves inspecting the code later, usually when the author thinks it is ready for deployment. In this process, the involvement of the reviewer is passive. Nevertheless, many developers are still using the code review process to ensure the best code goes into production. Git and many other code repositories support this feature using [Pull Request](https://docs.github.com/en/pull-requests) (PR).

Remember, there is a human behind the code under review. So the first problem is understanding that we are not reviewing the **coder but the code**. Secondly, code reviews are done via text — the way we write, the words. They may not carry the same emotion from a reader’s point of view. If not done correctly, it can easily create friction with teammates. To avoid this, we have to be mindful of how we approach code reviews.

**Here are my top 7 picks**

> Don’t forget, a person’s greatest emotional need is to feel appreciated. — H. Jackson Brown Jr.

**Give** kudos and compliment the author for their work. Do lighten the mood with a carefully selected smiley such as +1, 👍, 💯, 😻, 🏆, 💚, 🌈, 🦄 etc. Just be creative about it at the same time; don’t overdo it.

```Plain
😻 Yay! You used the new feature
💚 Thanks for adding test coverage!
🏆 omg finally someone refactored this code
💯 The strategy pattern fits perfectly here
```

The funny thing about code reviews is that if we have to review 200–400 lines of code(LOC) in 2–3 files, on average, it might take 60–90 minutes. But, on the other hand, if the LOC is beyond 400 or the number of files is more, it would take less than 30 minutes. Interesting, isn’t it? I am sure you might have done/felt the same some time in your career.

A [SmartBear study of a Cisco Systems programming team](http://smartbear.com/resources/case-studies/cisco-systems-collaborator/) revealed that the brain could only effectively process so much information at a time; beyond 400 LOC, the ability to find defects diminishes.

> Code review is not a voluntary activity, but a righteous duty that must be accounted, respected, and committed.

Code reviews may not be the first choice for every developer, with few exceptions. The reasons lie in the obligation that if something goes wrong in the code, there will always be two questions

1. **Who wrote it?**
2. **Who reviewed it?**

As software development professionals, we have to come out of that mindset and own the success and failure as a team. We must create an environment for our team to enjoy the review process with a free flow of communication. While doing so, we must commit to the task wholeheartedly and do justice to the job. Otherwise, say no beforehand.

- Be sure to _read_ the code, don’t just skim it.
- Code reviews and rework can be effective if done in the next two hours since it was requested. Commit to it.
- Commitment can go all the way to following it up with the developer until closure.

Leave your ego at the door. If you find yourself going back and forth in a conversation with your teammate, it’s getting too long or leading nowhere — take it offline.

> Review logic, not semicolons. Clean Code starts in your IDE.

There are so many tools and plugins available that do static code analysis. [SonarLint](https://www.sonarlint.org/) is a _**Free**_ and _**Open Source**_ IDE extension that identifies and helps you fix quality and security issues as you code.[SonarQube](http://www.sonarqube.org/)® is an automatic code review tool to detect bugs, vulnerabilities, and code smells in your code. Make them as prerequisite for the review.

![[assets/074IgV0oDG6Rwa8TM.png]]

ref: <https://docs.sonarqube.org/latest/>

- Use plugins/hooks that can run before a developer commits so that if there are any issues, the developer won’t be able to commit the code.[**Git Code Format Maven Plugin**](https://github.com/Cosium/git-code-format-maven-plugin) : A maven plugin that automatically deploys [google-java-format](https://github.com/google/google-java-format) code formatter as a `pre-commit` git hook. On commit, the hook will automatically format staged java files.
- At times the developer might commit the code without checking if any test cases fail. Enforce it during the commit using [**githook-maven-plugin**](https://github.com/phillipuniverse/githook-maven-plugin)

```Plain
<hooks>
    <pre-commit>
        echo "Validating..."
        exec mvn test
        echo "Formatting code..."
        exec mvn git-code-format:format-code
        echo "Validating format..."
        exec mvn git-code-format:validate-code-format
    </pre-commit>
</hooks>
```

- You can add SonarQube analysis and a Quality Gate to your Pull Requests (PR). **This is a must.** If there are anomalies, inform politely to the developer to fix them as a **prerequisite**.

Ask for the unit (TDD), integration (BDD), or end-to-end tests appropriate for the change. Most importantly, **review the test cases too**. In addition, it’s my recommendation that you can go beyond TDD and add [mutation testing](https://en.wikipedia.org/wiki/Mutation_testing).

> Results of code coverage, branch coverage, alive mutants can aid the reviews

_Code review is a conversation, not a queue of commands._

Always stay in the habit of co-creation; it doesn’t matter if you consider yourself a junior or a senior developer. As said earlier, words don’t carry the same emotions.Remember, it’s feedback about code and not a coder. Instead of telling them what the problem is, ask them questions, **make them think** with a friendly tone. Here are a few examples:³

```Plain
❓ Do you think maybe we could assign this to a variable and re-use it on line 9?
❓ What do you think about trying this option?
🔧 Could we possibly use this helpful utility that already does that for us, which our dear teammate Sarah built?
🔧 Can we move this code in its function to write more tests?
🎨 In the past we have used the classnames package to handle conditional styles. Do you have a preference?
😕 I'm not sure if I understand the whole picture. Could you explain what this function is doing?
⚠️ ️Heads up, this may end up conflicting with a change in \#4321. Please coordinate with them to ease the merge.
🚨 This change would introduce a bug. See \#4455 for more context.
🛑 Renaming a column without change and rollback script would fail deployment
🪓 These updates don’t seem to be related to the current PR and would best be introduced in a separate PR.
```

> At the same time, be confident; anything that implies doubt, avoid it. If you’re not sure, self-reflect “why aren’t you sure?” Do some experiments or research and then come back with confidence.

_One of my most effective tricks in code reviews is to replace all occurrences of «I»/«me» and «you» with «we»/«us» or at least be passive_

```Plain
Not good
❌ "you forgot a variable here"Good
✅ "We are missing a variable here"
✅ "There is a variable missing here: [code block]"
```

Rather than just telling them, “this code could be improved”. Propose possible refactoring/solution using principle/pattern with example and at the same time be open-minded and accept different approaches, such as:

```Plain
This code works perfectly but, after I started reading, I thought of another idea that I wanted to run it by you. I'm not sure, but what about:
sample code \#1,
sample code \#2
What are your thoughts?
```

**Refer principles and best practices such as**

- Design Patterns
- SOLID Principles
- YAGNI: “**Y**ou **A**ren’t **G**onna **N**eed **I**t”
- DRY: “**D**on’t **R**epeat **Y**ourself”
- KISS: “**K**eep **I**t **S**imple, **S**tupid”
- Separation of Concerns
- Boy-Scout Rule
- Principle of Least Astonishment

> Attitudes are contagious. Are yours worth catching?

As discussed earlier, Code Reviews are a great way to broadcast detailed knowledge about what changes are happening on a project. So we improve the functional and technical expertise in the area that is under review

Code Reviews provide a direct and explicit way for the team members to teach/coach one another. When you see a carefully crafted code during the review, it makes us do the same. But, on the other hand, when you see an opportunity to improve the code, you want to be sure of your suggestion. In the process of proposing examples, we learn.  
Encouraging this culture of asking for, providing, and accepting feedback helps the team move forward in a unified way.

> Last but not the least, Code Reviews should result in a “making the day” feeling, irrespective of who.

**Positive**, a **congratulatory comment**

- 👍 `:thumbsup:` to compliment
- 💯 `:100:` to compliment
- 😻 `:heart_eyes_cat:` to compliment
- 🌈`:rainbow:` to compliment
- 🦄`:unicorn:` to compliment
- 🏆 `:trophy:` omg finally
- 💚`:green_heart:` Thanks for adding test coverage!

**for Non-blocking** **questions**

- ❓ `:question:` question about feature/impl/design...
- 🤔`:thinking:` Out of curiosity, when would I choose...
- 😕`:confused:` I don't get it
- ⚠ `:warning:` Heads up, this may...
- 📘 `:blue_book:` convention not respected
- 🔧`:wrench:` improvement
- 🛠 `:tools:` improvement to do later
- 🎱`:8ball:` fault/mismatch of the requested feature

**For Blocking** comments that **must** be addressed

- 🚨`:rotating_light:` This change would introduce a bug...
- 🛑`:octagonal_sign:` Something that will break
- 🪓`:axe:` break into a separate Pull request
