---
title: Best Practices for Achieving Immutability in C
feed: show
---

[https://medium.com/@shoebsd31/best-practices-for-achieving-immutability-in-c-e0375b11f77d](https://medium.com/@shoebsd31/best-practices-for-achieving-immutability-in-c-e0375b11f77d)

**Introduction**

Immutability is a key concept in software engineering and it is essential to ensure the integrity of your data and prevent unintended changes. In software development, immutability refers to the practice of creating objects that cannot be modified once they are created. This ensures that the state of an object remains consistent throughout its lifecycle, making it easier to reason about the behavior of your code and to avoid unintended consequences.

source : stock.adobe.com

![[assets/1x2eddJ3dyI3jWBrftJ-agQ.jpeg]]

In C#, immutability can be achieved by using value types and by using objects that are specifically designed to be immutable. In this blog, we will explore some of the best practices for achieving immutability in C#.

## Use value types

Value types in C# are always immutable. Examples of value types include integers, floating-point numbers, and dates. When you use a value type, you can be sure that its value will not change during its lifetime, making it ideal for use in scenarios where immutability is important.

## Use read-only properties

Another way to achieve immutability in C# is to use read-only properties. In C#, you can create read-only properties by using the `readonly` keyword. This ensures that the value of the property cannot be changed once it is set.

For example:

```Plain
class MyClass
{
 public readonly int MyInt;
 public MyClass(int myInt)
 {
 MyInt = myInt;
 }
}
```

**Use the** `**ImmutableArray**` **type**

The `ImmutableArray` type is a specialized type that is designed to be immutable. It is available in the System.Collections.Immutable namespace and provides an array that cannot be modified after it is created. This is useful in scenarios where you want to ensure that the data in the array remains unchanged.

For example:

```Plain
var array = ImmutableArray.Create(1, 2, 3, 4);
```

**Use the** `**ImmutableList**` **type**

The `ImmutableList` type is similar to the `ImmutableArray` type, but provides a list instead of an array. Like the `ImmutableArray`, the `ImmutableList` is designed to be immutable and provides a way to ensure that the data in the list remains unchanged.

For example:

```Plain
var list = ImmutableList.Create(1, 2, 3, 4);
```

**Use the** `**ImmutableDictionary**` **type**

The `ImmutableDictionary` type is a specialized type that provides an immutable dictionary. It is available in the System.Collections.Immutable namespace and provides a dictionary that cannot be modified after it is created. This is useful in scenarios where you want to ensure that the data in the dictionary remains unchanged.

For example:

```Plain
var dictionary = ImmutableDictionary.Create<string, int>();
dictionary = dictionary.Add("Key1", 1);
dictionary = dictionary.Add("Key2", 2);
```

## Conclusion

In conclusion, immutability is an important concept in software engineering and there are several best practices for achieving immutability in C#. By using value types, read-only properties, and the specialized `Immutable` types, you can ensure that your data remains unchanged and avoid unintended consequences.

“**Immutability is the cornerstone of robust and maintainable code.**” — Martin Fowler
