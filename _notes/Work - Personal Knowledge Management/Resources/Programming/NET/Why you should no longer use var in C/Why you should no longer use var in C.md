---
title: Why you should no longer use var in C
feed: show
---

[https://schwabencode.com/blog/2022/09/09/csharp-why-you-should-not-use-var-anymore](https://schwabencode.com/blog/2022/09/09/csharp-why-you-should-not-use-var-anymore)

![[assets/schwabencode-dotnet.png]]

[Why you should no longer use var in C#](https://schwabencode.com/blog/2022/09/09/csharp-why-you-should-not-use-var-anymore)

> Beginning with C# 3, variables that are declared at method scope can have an implicit "type" var. An implicitly typed local variable is strongly typed just as if you had declared the type yourself, but the compiler determines the type.

With this section `var` is described in the [documentation of C#](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/var?WT.mc_id=DT-MVP-5001507).

So, as you can read, the `var` keyword came from C# 3 and is thus already a few years old, to be more precise, the release of C# 3 was almost 15 years ago. At that time, the language was still relatively young and the ecosystem as a whole - Visual Studio, Visual Studio Code, Azure DevOps, GitHub or overall Code Reviews and Open Source - was also different.

One of the features why var was introduced was implicit typing: you didn't have to write the type twice and the compiler simply recognized what type var must be based on the assignment.

```Plain
var list = new List<string>();
// The compiler knows, var must be of List<string>
```

## Why var is bad?

So var had its place in its time, but what are the disadvantages of `var`?

### Type

Many mainly advance the argument that the type no longer needs to be written twice.

```Plain
List<string> list = new List<string>();
```

That is correct. However thank God meanwhile a simplified syntax sugar exists, which makes this argument obsolete.

```Plain
List<string> list = new();
```

Regarding the type, however, there are other scenarios that play a role in everyday life here, and that is the use of interfaces during initialization.

```Plain
IList<string> list = new List<string>();
```

This is a notation you see very often — even in open source projects. On its own, this is more a matter of style, but there is often a programmatic use case behind such a style.

```Plain
IList<string> list;
if(condition)
{
   list = MyCollectionFactory.CreateCollection();
}
else
{
   list = new List<string>();
}
```

So this is a scenario in which `var` simply does not work — and for which var was not designed, but misused. It automatically becomes inconsistent — unavoidable.

Even worse are the workarounds you see when `var` is abused.

```Plain
var list = new List<string>() as IList<string>;
if (condition)
{
   list = MyCollectionFactory.CreateCollection();
}
```

Result: an unnecessary allocation of the list as soon as the condition occurs. This is bad code.

## Naming

Many supporters of var say that you don’t need the type because the name of methods and variables says everything necessary.

Nice wishful thinking that makes me wonder if people really write code. Yes, in theory this should be so — everyday life is far from it: often it can be recognized neither by the method name nor by the variable name, which concrete type comes back.

Of course, now you will hear the statement that you should just learn the naming. Haha! With naming, one thing is guaranteed: it will never work and will never be consistent.

If you see this code, without context: what type does var have?

```Plain
var proxy = ProxyFactory.For<IUserClient>( );
```

Is it rather

```Plain
Proxy<IUserClient> proxy = ProxyFactory.For<IUserClient>( );
```

or is it

```Plain
IUserClient proxy = ProxyFactory.For<IUserClient>( );
```

Naming is never perfect. Context is necessary, if you use var.

## Reviewing and IntelliSense

And that is the main reason against var. Most of us now live in a world where source code is not always read only in an IDE. There are more and more scenarios where we don’t have compiler and IntelliSense support.

Without IntelliSense or type help I have no chance to recognize which concrete type it is or which one comes back from a method. We now live in a world where source code is part of documentations or published as open source, for example on GitHub and in markdown files.

It is an insane hurdle when I have to click through umpteen calls in documentation or source code in the browser to find out which type comes back. What’s the point? You make it unnecessarily difficult for the users of the documentation or source code!

## Please: Stop it

It's 2022, soon 2023. Know your language.  
There are better options than var today that do not place unnecessary barriers.
