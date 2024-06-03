---
title: C interview questions for 0–3 years of Experience
feed: show
---

![[assets/1M_CexUzKplwt0nAohFVVvQ.png]]

![[assets/1M_CexUzKplwt0nAohFVVvQ 1.png]]

## Basics of the Programming

1. **What are OOPS concepts. Explain briefly.** You can know more [here](https://medium.com/from-the-scratch/oop-everything-you-need-to-know-about-object-oriented-programming-aee3c18e281b) and [here.](https://stackify.com/oop-concepts-c-sharp/)

2. **What are SOLID principles?** It is explained very briefly and easily [here.](https://www.freecodecamp.org/news/solid-principles-explained-in-plain-english)

3. **Abstract vs. Inheritance.** _**Note that here the interviewer is asking about what is the difference between these two concepts.**_

**Abstraction** hides the implementation details and only show the functionality. It reduces the code complexity.

**Inheritance** creates a class using a property of another class. It improves the code reusability.

## **Language specific questions**

1. **Abstract class vs. Interfaces**

    _short answer_: An abstract class allows you to create functionality that subclasses can implement or override.

    An interface only allows you to define functionality, not implement it.

    And whereas a class can extend only one abstract class, it can take advantage of multiple interfaces.

2. **Types of access modifiers in C#**

    Here you can see different access modifiers with their accessibility reach.

    ![[assets/1PIZRexxgunRkTOodneQyPg.png]]

3. **What are different types of classes?**

    - Static Class
    - Sealed Class
    - Partial Class
    - Abstract Class

    on this [link](https://www.c-sharpcorner.com/UploadFile/0c1bb2/types-of-classes-in-C-Sharp1/), you can learn more about the types of classes_**.**_

4. **Method overloading vs. method overriding vs. Method shadowing**

    - _**Method overriding**_ is a mechanism to achieve polymorphism where the super class and subclass have same methods, including the parameters and signature and, when you call it using the subclass object, the implementation in the subclass is invoked.
    - Having two or more methods with same name but different in parameters, is known as **method overloading**
    - The method of hiding the base class’s methods from the derived class is known as Method Hiding. This method is also known as [**Method Shadowing**](https://www.codingninjas.com/codestudio/library/c-method-hiding)**.**

5. **Memory stack vs. heap.** [Here](https://dotnettutorials.net/lesson/stack-and-heap-dotnet/) you can get further information.

6. **Delegates and it’s types — why we use delegates.**

7. **Different types of constructor**. [Here](https://www.programiz.com/csharp-programming/constructors) you can get further information.

8. **Async And Await. How to use it. And when to use it.**

9. **What is Yield keyword?** [Here](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/statements/yield) you can get further information.

10. **Readonly vs. const**

    - Readonly fields can be initialized at declaration or in the constructor. Therefore, readonly variables are used for the run-time constants. Must have set value, by the time constructor exits.
    - Constants are _**static**_ by default. The constant fields must be initialized at the time of declaration. Therefore, const variables are used for compile-time constants.

11. **Properties vs. variables**

    - A _variable_ corresponds directly to a memory location. You define a variable with a single declaration statement.
    - A _property_ is a data element defined on a module, class, or structure. You define a property with a code block between the `Property` and `End Property` statements. The code block contains a `Get` procedure, a `Set` procedure, or both. These procedures are called _property procedures_ or _property accessors_. In addition to retrieving or storing the property's value, they can also perform custom actions, such as updating an access counter.
    - A _property_ is a data element defined on a module, class, or structure. You define a property with a code block between the `Property` and `End Property` statements. The code block contains a `Get` procedure, a `Set` procedure, or both. These procedures are called _property procedures_ or _property accessors_. In addition to retrieving or storing the property's value, they can also perform custom actions, such as updating an access counter.

12. **IEnumerable vs. IQueryable**

    - While query data from a database, **IEnumerable** execute a select query on the server side, load data in-memory on a client-side and then filter data.
    - While query data from a database, **IQueryable** execute the select query on the server side with all filters.

    Suppose you want to get users with city name Atlanta, IEnumerable will get all the user data from database and then will search in your system. But IQueryable will apply that as Filter on database and will give you user records with Atlanta only.

    _**It is preferable to use IEnumerable with system data and IQueryable with (remote) database**_

13. **Type casting vs. type conversion**

    Casting is a way of telling the compiler, “Object X is really Type Y, go ahead and treat it as such.”

    Conversion is saying “I know Object X isn’t Type Y, but there exists a way of creating a new Object from X of Type Y, go ahead and do it.”

14. **What is thread? What is Task? (Task vs. Threading).** The concepts are explained briefly [**here**](https://www.c-sharpcorner.com/article/task-and-thread-in-c-sharp/)**.**

15. **Var vs. Dynamic variable**. [Here](https://www.codingninjas.com/codestudio/library/var-vs-dynamic-in-c) you can get further information.

## LINQ related

1. What is LINQ? [Here](https://zetcode.com/csharp/linq/) you can get further information.
2. Select and SelectMany

    `Select` takes each source element, transforms it, and returns a sequence of the transformed values

    `SelectMany` flattens queries that return lists of lists.

    ```Plain
    public class PhoneNumber
    {
    public string Number { get; set; }
    }

    public class Person
    {
    public IEnumerable<PhoneNumber> PhoneNumbers { get; set; }
    public string Name { get; set; }
    }

    IEnumerable<Person> people = new List<Person>();

    // Select gets a list of lists of phone numbers
    var phoneLists = people.Select(p => p.PhoneNumbers);

    // SelectMany flattens it to just a list of phone numbers.
    var phoneNumbers = people.SelectMany(p => p.PhoneNumbers);

    // And to include data from the parent in the result:
    // pass an expression to the second parameter (resultSelector) in the overload:
    var directory = people
    .SelectMany(p => p.PhoneNumbers,
    (parent, child) => new { parent.Name, child.Number });
    ```

3. First vs. FirstOrDefault

`First` will throw an exception if there's no row to be returned,

`FirstOrDefault` will return the default value (`NULL` for all reference types) instead.
