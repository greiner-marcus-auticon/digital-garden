---
title: String interpolation
feed: show
---

[https://medium.com/codex/c-string-interpolation-4c1b4462b1a4]( https://medium.com/codex/c-string-interpolation-4c1b4462b1a4)

![[assets/1ieIdzQ24XCI_E4kXE4zu-Q.png]]

You’re a C# programmer in 2021. You need to build strings that are more complex than a simple ToString() call.

This article will give you a look at many of the ways you’d do that, ending with what I believe is the current best solution.

Note: This article applies to C# in general. It also applies to Visual Basic in the .NET era, but converting between the two is up to you. The Debug.Log you see sprinkled throughout this article is [Unity Engine’s Debug.Log](https://docs.unity3d.com/ScriptReference/Debug.Log.html). You might be using plain dotnet instead — in that case, your logging might happen with [Console.WriteLine](https://docs.microsoft.com/en-us/dotnet/api/system.console.writeline?view=net-5.0), [System.Diagnostics.Debug.WriteLine](https://docs.microsoft.com/en-us/dotnet/api/system.diagnostics.debug.writeline?view=net-5.0) or something more exotic in your undoubtedly slick ASP.NET API.

Let’s say you have these three values and you need to print them on one line.

When you need to get those values into a string like “values: 3, 1, 2”…

## String Concatenation

Ah, the classic.

Do you [suffer](https://getyarn.io/yarn-clip/85530a0c-5646-45cc-b7a4-7d7e135715c2) from code like this?

String concatenation, short form

Or like this?

String concatenation, long form

It seems so quick and easy, doesn’t it?

There’s a significant catch, besides just the readability and cluttered syntax.

Concatenating two strings requires [a memory allocation and two copy operations](https://referencesource.microsoft.com/#mscorlib/system/string.cs,55b2f6374a7cf7c1). That code snippet in the picture? Think about how many allocations, copy operations, and subsequent garbage collections will be required. That’s new memory allocations and copy operations for each concatenation, plus eventual garbage collection, and that adds up fast! To the heap with that.

This style of string building harkens [back to BASIC](http://www.petesqbsite.com/sections/tutorials/tuts/dandd/basic-15.html) in the 1980s, back when we’d type programs in line-by-line on our [amber screens](https://en.wikipedia.org/wiki/Monochrome_monitor) and hope we didn’t make a typo. If you’re also from that era, let’s take a moment together to realize that was 40 years ago, before we go back to yelling at the kids on our lawn.

Yes, I know [Fortran 77 had string concatenation](https://docs.oracle.com/cd/E19957-01/805-4939/6j4m0vn73/index.html) before that, and I’m sure someone could find an even older one on [Rosetta Code](https://rosettacode.org/wiki/String_append). The point being, it’s probably time for an update. There have been a few innovations since then.

## StringBuilder

Let’s check out some more efficient tech from the ‘90s and early ‘00s.

You’ve heard [StringBuilder](https://docs.microsoft.com/en-us/dotnet/api/system.text.stringbuilder?view=net-5.0) is good (_it IS_) but maybe it seems like overkill for just putting a couple values together.

StringBuilder being used nicely — and VS Code reminding me that there can be improvements

C# got ahold of this with .NET Framework 1.1, in 2002.

Some of you recovering Java coders will recognize [StringBuilder from Java 1.5](https://docs.oracle.com/javase/1.5.0/docs/api/java/lang/StringBuilder.html) in 2004 or — if you have a few _more_ gray hairs — [StringBuffer from the original Java](http://titanium.cs.berkeley.edu/doc/java-langspec-1.0/javalang.doc12.html#14461) in 1998.

C++ has had a class in the Standard Library called [std::stringstream since C++98](https://www.cplusplus.com/reference/sstream/stringstream/stringstream/). It remains notable enough to have [a minimal Wikipedia article](https://en.wikipedia.org/wiki/Sstream).

StringBuilder does most of its memory allocation on initialization, and when you call ToString(). Otherwise, it primarily copies a value _once_ to already-claimed memory when you call Append, expanding with additional ‘chunk’ allocations as needed. [Check out the source](https://referencesource.microsoft.com/#mscorlib/system/text/stringbuilder.cs), if you’re into that sort of thing.

## Composite Formatting

Maybe you’ve tried [composite formatting](https://docs.microsoft.com/en-us/dotnet/standard/base-types/composite-formatting) via [String.Format](https://docs.microsoft.com/en-us/dotnet/api/system.string.format?view=net-5.0).

Composite formatting — now we’re really peeking our head out of the 1990s

More compact version

Composite formatting was really nice when it came out. It gave us template-style string building as an official language feature. It felt clunky when you used it for anything non-trivial, where it resulted in some extremely long lines of code, but it was _there_ and you didn’t have to write or find a string templating library. And you could use formatting strings, the same as you’d use in String.Format!

Is the SHL/SHR circuit in a CPU a… shift worker?

It’s nice until you need to change what you’re doing. For example, if you need to reorder the values, or insert new values into the middle of the string. Now you’re heading towards inadvertently creating bugs in your APIs. Juggling those lists of arguments gets to be a noticeable mental load.

By the way, [String.Format uses StringBuilder under the hood.](https://referencesource.microsoft.com/#mscorlib/system/string.cs,bdf91919a8d3537e,references)

## String.Join

You’ve probably heard of using [String.Join](https://docs.microsoft.com/en-us/dotnet/api/system.string.join?view=net-5.0) to convert lists of objects into strings, too. It’s an elegant tool for specific use cases. It’s been around just as long as StringBuilder.

Originally, you could join string arrays, and that was it.

People all over the world, join strings

People wrote some [clever custom workarounds](https://stackoverflow.com/a/3610795/691749) to support joining other types.

String.Join received support for joining generic objects together in 2010, with .NET Framework 4.0. (Unity wouldn’t get support for this until 2017.)

People all over the world, join things

[String.Join does its own unique thing under the hood](https://referencesource.microsoft.com/#mscorlib/system/string.cs,06d13c9cb8b83f5d).

## String Interpolation

Now that we’ve seen all the old ways to do it, let’s look at something less than 10 years old as of this writing in 2021.

String interpolation has been around for a while now. How long? It was [introduced in 2014 by C# 6](https://docs.microsoft.com/en-us/dotnet/csharp/whats-new/csharp-version-history#c-version-60) and this was the version aligned with .NET Framework 4.6 / CLR 4, with [limited support and caveats for earlier versions](https://stackoverflow.com/a/28921749/691749) of .NET Framework. It’s been supported natively from Visual Studio 2015, Visual Studio 2013 via an addon, and Unity 2017. Back then, [Unity called their C# 6 support in 2017 “experimental”](https://blog.unity.com/technology/introducing-unity-2017) and you had to manually select .NET 4.6 for your project in order to use it.

Yeah, yeah. Boring history lessons, am I right? The point is, it’s been around long enough that it should be in more common use.

How would you like your string building to look like this instead?

String interpolation two ways: the new hotness

This is called string interpolation. The astute reader may say “Hey, that looks like templates from other languages.” And you would basically be right!

It’s not a simple wrapper around a specific string function, though. There’s [a nice article about what the compiler does behind the scenes](https://www.meziantou.net/interpolated-strings-advanced-usages.htm) but the beauty of it is that you don’t need to care how it works.

I’ll summarize and give a couple more real-world examples. There’s a nice [tutorial in the documentation](https://docs.microsoft.com/en-us/dotnet/csharp/tutorials/string-interpolation). There’s also a [reference with ALL the gory details](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/tokens/interpolated).

To tell the compiler that you want interpolation on a string, simply prefix your opening double-quote character with a $ character. It’s important that there is nothing between the two characters. Well, except for maybe an @ in C# 8 and later, because then you can do interpolated [verbatim strings](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/tokens/verbatim) as well.

In an interpolated string, you can name variables and properties in between curly braces like {myVal} and your code becomes much more readable. If you need a curly brace literal in your string, you will have to double it as a sort of escaping mechanism, because single curly braces now have special meaning.

You’re not limited to only variables or properties. You can put any value expression inside the curly braces. Function calls, mathematical expressions, you name it — as long as it has a value, it can go there. You can even do formatting like you would with String.Format.

Sometimes you only have the appetite for a small piece of pi

It will convert objects using their default ToString() methods, so make sure those are in place. If you need to get a value via code, for example with a ternary operator, you can do that too… it’s not super readable, but go right ahead. Please drop a comment in your code for the next person/you who has to maintain it later.

Can you write good, concise code? Of course you can. You’re an expert.

## When to Avoid

This is a huge boon to debugging and formatting, and it helps us out with the first and arguably [most important virtue of a programmer: laziness.](http://threevirtues.com/)

It may be tempting, but _**never, EVER**_, use string interpolation to handle **un-sanitized user input** for the purpose of parameterizing SQL queries, assembling command lines or API calls, or anything else that truly requires sanitization. A user with intent could easily cause buffer overflows with this.

This advice is true of any string manipulation but it bears repeating: **Always sanitize user input.**

## Performance

I hear you asking, “What about performance?”

It’s fast enough that if I wanted to make anything faster, I’d be on the wrong side of diminishing returns for my game dev and data processing purposes. It’s as good as StringBuilder.

You want benchmarks? Objective evidence? Excellent. Please check out:

## Loggers

There are some alternatives if you’re using 3rd party loggers. [Serilog](https://serilog.net/) comes to mind right away. If you’re building an ASP.Net application, you should probably be using it with its enrichers, if not something similar.

## Thanks for reading
