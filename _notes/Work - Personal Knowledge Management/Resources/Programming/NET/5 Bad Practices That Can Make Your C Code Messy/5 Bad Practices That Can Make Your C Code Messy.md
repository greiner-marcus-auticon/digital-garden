---
title: 5 Bad Practices That Can Make Your C Code Messy
feed: show
---

[https://blog.dotnetsafer.com/5-bad-practices-that-can-make-your-c-code-messy-and-how-to-avoid-them/](https://blog.dotnetsafer.com/5-bad-practices-that-can-make-your-c-code-messy-and-how-to-avoid-them/)

![[assets/5-Bad-Practices-That-Can-Make-Your-Csharp-Code-Messy.png]]

How do you know if you’re following good practices when programming in C#? And how can you avoid the bad practices that can make your code messy and harder to manage? In this article, we’ll look at five common bad practices, and explain how to avoid them by using some better alternatives.

## Stop check null with if statement

The `if` statement can have an enormous number of utilities and uses. One of them is to use it to check `null`. Although this is a correct way, it can be stated a little better:

**Bad way:**

**Good way:**

At no point am I saying that you can’t use the `if` statement for null checking, but by having the **null conditional** (which is what that feature was implemented for), we will get **cleaner and easier to read code.**

📚Check out the Microsoft article to learn more: [**Null coalescing operator**](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/null-coalescing-operator)

## Use Tuples instead classes

If you want to return more than one result, the first thing that will probably come to mind is to create a class just for that purpose. This is a correct way to do it but not the best way to do it.

**Bad way:**

**Good way:**

For this there are `tuples`, according to [Mahesh Chand](https://www.c-sharpcorner.com/members/mahesh-chand) in his article [Tuples in C#](https://www.c-sharpcorner.com/article/tuples-in-c-sharp/):

> “Often, we want to return more than one value from a class method. Prior to the introduction of tuples in .NET, there were three common ways to do so.
> – Out parameters
> – Class or struct types
> – Anonymous types returned through a dynamic return type
> Tuples solve this problem”

As Mahesh Chand says, **tuples solve this problem**. So using them is a much better option than creating a new class.

📚Check out the Microsoft article to learn more: [**Tuple types in C#**](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/value-tuples)

## Avoid modifications by using private members

It is **not good practice** to have the ability to modify members. You have to **be careful** which of them can be modified or not.

**Bad way:**

**Good way:**

If the member will not be modified, it is better **not to use** `set;` to avoid any accidental (or intentional) modification in the future.

📚Check out the Microsoft article to learn more: [**Get & Set accessors**](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/using-properties)

## Use conditional operator instead if-else

Often out of habit, we get used to using the classic `if-else` and that’s it. I would not really like to consider this as a bad practice but there is a better way to do it:

**Bad way:**

**Good way:**

As we can clearly see, although it is a similar alternative to `if-else`, by using the ternary **conditional operator** `?:`, it is much easier to read, understand and we will get a cleaner code.

📚Check out the Microsoft article to learn more: [**Conditional operator**](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/conditional-operator)

## Avoid initials as identifier abbreviations

When thinking about clean code, one of the first occurrences may be to use initials to shorten the code in identifiers. This is a good practice but you have to **be very careful** with it.

**Bad way:**

**Good way:**

If you are going to use abbreviations, **please, always think twice to avoid possible confusion in the future.** Many times easy does not equal optimal.

📚Check out the Microsoft article series to learn more: [**Naming Guidelines**](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/naming-guidelines)
