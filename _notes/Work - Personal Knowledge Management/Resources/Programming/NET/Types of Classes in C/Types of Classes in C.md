---
title: Types of Classes in C
feed: show
---

![[assets/twitter_card_logo.png]]

## Introduction

As we know, C# is an Object Oriented Programming language that provides the ability to reuse existing code. To reuse existing code, [C#](https://www.c-sharpcorner.com/topics/c#) provides various object-oriented concepts such as classes, objects, properties, methods, structs, and records.

Now in this article, we learn about the overview of classes that are part of C# Object Oriented Programming concepts, and in the next article, we learn in detail about each class in depth. Let us start with defining the class.

## What is a class in C#?

Classes are user-defined data types that represent the **state** and **behavior** of an object. The state represents the properties, and **behavior** is the action that objects can perform.

Classes can be declared using the following access specifiers that limit the accessibility of classes to other classes. However, some classes do not require any access modifiers.

- Public
- Private
- Protected
- Internal
- Protected internal

To learn the details of access specifiers, check out [Access Modifiers in C#](https://www.c-sharpcorner.com/blogs/access-modifiers-in-c-sharp1).

**For example**:

```Plain
public class Accounts
{

}
```

**Some Key points about classes:**

- Classes are reference types that hold the object created dynamically in a heap.
- All classes have a base type of **System.Object**.
- The default access modifier of a class is **Internal**.
- The default access modifier of methods and variables is **Private**.
- Directly inside the namespaces, declarations of private classes are not allowed.

### Types of classes in CSharp

## What is an Abstract Class in C#?

An abstract class is a class that provides a common definition to the subclasses, and this is the type of class whose object is not created.

**Some key points of Abstract classes are:**

- Abstract classes are declared using the abstract keyword.
- We cannot create an object of an abstract class.
- It must be inherited in a subclass if you want to use it.
- An Abstract class contains both abstract and non-abstract methods.
- The methods inside the abstract class can either have an or no implementation.
- We can inherit two abstract classes; in this case, implementation of the base class method is optional.
- An Abstract class has only one subclass.
- Methods inside the abstract class cannot be private.
- If there is at least one method abstract in a class, then the class must be abstract.

**For example**:

```Plain
abstract class Accounts
{

}
```

Continue here: [Abstract Classes In C# (c-sharpcorner.com)](https://www.c-sharpcorner.com/UploadFile/f0b2ed/abstract-class-in-c-sharp/)

## What are Partial Classes in CSharp

It is a type of class that allows dividing their properties, methods, and events into multiple source files, and at compile time, these files are combined into a single class.

**The following are some key points:**

- All the parts of the partial class must be prefixed with the partial keyword.
- If you seal a specific part of a partial class, the entire class is sealed, the same as for an abstract class.
- Inheritance cannot be applied to partial classes.
- The classes written in two class files are combined at run time.

**For example**:

```Plain
partial class Accounts

{

}
```

Continue here: [Partial Classes in C#(c-sharpcorner.com)](https://www.c-sharpcorner.com/UploadFile/3d39b4/partial-classes-in-C-Sharp-with-real-example/)

## What is Sealed Class in C#?

A Sealed class is a class that cannot be inherited and used to restrict the properties.

**The following are some key points:**

- A Sealed class is created using the sealed keyword.
- Access modifiers are not applied to a sealed class.
- To access the sealed members, we must create an object of the class.

**For example**:

```Plain
sealed class Accounts

{

}
```

Continue reading: [Sealed Class in C# (c-sharpcorner.com)](https://www.c-sharpcorner.com/article/sealed-class-in-C-Sharp/).

## What is Static Class in C#?

It is the type of class that cannot be instantiated. In other words, we cannot create an object of that class using the new keyword, such that class members can be called directly using their name.

**The following are some key points:**

- It was created using the static keyword.
- Only static members are allowed; in other words, everything inside the class must be static.
- We cannot create an object of the static class.
- A Static class cannot be inherited.
- It allows only a static constructor to be declared.
- The static class methods can be called using the class name without creating the instance.

**For example**:

```Plain
static class Accounts

{

}
```

Continue reading [Static Class In C# (c-sharpcorner.com)](https://www.c-sharpcorner.com/UploadFile/74ce7b/static-class-in-C-Sharp/).

## Summary

I hope you have learned an overview of various types of classes in [C#](https://www.c-sharpcorner.com/topics/c#). In the next article, we will learn each class in C#.
