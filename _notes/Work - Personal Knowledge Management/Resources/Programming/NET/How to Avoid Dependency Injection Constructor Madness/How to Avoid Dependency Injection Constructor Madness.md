---
title: How to Avoid Dependency Injection Constructor Madness
feed: show
---

[https://medium.com/@callmeyaz/how-to-avoid-dependency-injection-constructor-madness-net-65b48046bfd1](https://medium.com/@callmeyaz/how-to-avoid-dependency-injection-constructor-madness-net-65b48046bfd1)

![[assets/1yuVzMhCJyDENbyhwAsrkwA.png]]

## How to Avoid Dependency Injection Constructor Madness —ASP.NET Core 8.0

![[assets/1pB2EJO-7n8i15lu9zgSKtQ.png]]

[Yaz](https://medium.com/@callmeyaz?source=post_page-----65b48046bfd1--------------------------------)

·

[Follow](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F821989934a74&operation=register&redirect=https%3A%2F%2Fmedium.com%2F%40callmeyaz%2Fhow-to-avoid-dependency-injection-constructor-madness-net-65b48046bfd1&user=Yaz&userId=821989934a74&source=post_page-821989934a74----65b48046bfd1---------------------post_header-----------)

3 min read

·

Dec 2, 2023

If you are in the same boat as me, you might have come across a time when the number of injected dependencies grew exponentially, and the constructor parameters looked like a never-ending story.

```Plain
public MyClass(SomeClass1 obj1, SomeClass2 obj2, SomeClass3 obj3, ....) {
  /* some code here */
}
```

A good example would be, injecting a large number of Repositories into a _UnitOfWork_ class.

Of course, we could use the [Service Locator](https://blog.ploeh.dk/2010/02/03/ServiceLocatorisanAnti-Pattern/) design pattern but since that is now considered as an anti-pattern it should be avoided. [Read more here](https://medium.com/@callmeyaz/factory-pattern-vs-service-locator-3e6511e03cc0) to see how the implementation below is not a Service Locator design pattern.

So how to deal with this situation?

Well, if we use Factory Method of a Factory Method, and if we have designed the interface inheritance the right way this can be achieved.

Let us assume the above scenario where we need to inject a huge list of repositories into a _UnitOfWork_ constructor. Since all those repositories are, well, repositories, they should follow the inheritance principle as follows:

```Plain
/* Base interface */
public interface IRepository {}

/* Child 1 */
public interface IUserRepository: IRepository {
  /* Some interface specific declarations here */
}

/* Child 2*/
public interface IAddressRepository: IRepository {
  /* Some interface specific declarations here */
}

/* Child 3*/
public interface IOrderRepository: IRepository {
  /* Some interface specific declarations here */
}
```

In the above code, we have created a base Interface _IRepository_ which is inherited by other child interfaces with their specific declarations.

We would assume that these child interfaces are implemented by their concrete implementations as follows:

```Plain
/* Concrete Child 1 */
public class UserRepository: IUserRepository {
  /* Some interface specific implementation here */
}

/* Concrete Child 2*/
public interface AddressRepository: IAddressRepository {
  /* Some interface specific implementation here */
}

/* Concrete Child 3*/
public interface OrderRepository: IOrderRepository{
  /* Some interface specific implementation here */
}
```

Now the moment of truth; here is how we will inject the above dependencies into the _UnitOfWork_ class:

```Plain
public class UnitOfWork: IUnitofWork {

  // factory injected to resolve…
```
