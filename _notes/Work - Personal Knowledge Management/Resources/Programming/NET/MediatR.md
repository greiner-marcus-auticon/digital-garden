---
title: MediatR
feed: show
---

[https://link.medium.com/6Pco8WnOezb](https://link.medium.com/6Pco8WnOezb)

Command-Query Responsibility Segregation (CQRS) is an architectural pattern that separates the responsibilities of reading and writing data into two separate components. By using CQRS, commands and queries can be separated which allows them to be developed independently with different performance requirements. This creates more flexibility when scaling applications as the developers are able to scale out the commands and queries individually according to the load of each one. In addition, this separation of concerns allows for any changes on either side to not affect each other.

The two components of CQRS are:

- **Command**: A command is used to modify data and is typically executed by an action that a user performs.
- **Query:** A query is used to retrieve data and is typically executed by an action that displays data to the user.

**What Is MediatR**

[MediatR](https://github.com/jbogard/MediatR) is an open-source library that provides a simple mediator pattern implementation to help developers write cleaner, more maintainable code. It helps developers create applications using the Command Query Responsibility Segregation (CQRS) pattern, which allows them to separate the responsibilities of sending and handling commands, as well as querying and retrieving data. MediatR simplifies the process of writing code for CQRS applications by providing an easy-to-use API for sending commands and queries. It also provides a way to register handlers for specific commands or queries, allowing developers to easily create custom logic for their applications.

In this article, we will walk through the steps to implement CQRS in a .NET application using the MediatR library.

**Step 1: Install MediatR NuGet Package**

The first step in implementing CQRS in a .NET application using the MediatR library is to install the MediatR NuGet package. To do this, open the Package Manager Console in Visual Studio and enter the following command:

```Plain
Install-Package MediatR
```

**Step 2: Create a Command Class**

The next step is to create a command class. A command class is used to modify data in the system. A command class should implement the `IRequest` interface provided by the MediatR library. Here is a sample command class:

```Plain
public class CreateUserCommand : IRequest
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string Email { get; set; }
}
```

**Step 3: Create a Command Handler Class**

The next step is to create a command handler class. The command handler class is responsible for executing the command. A command handler class should implement the `IRequestHandler` interface provided by the MediatR library and should have a constructor that takes in the dependencies it requires. Here is a sample command handler class:

```Plain
public class CreateUserCommandHandler : IRequestHandler<CreateUserCommand>
{
    private readonly IUserRepository _userRepository;
    public CreateUserCommandHandler(IUserRepository userRepository)
    {
        _userRepository = userRepository;
    }

    public async Task<Unit> Handle(CreateUserCommand request, CancellationToken cancellationToken)
    {
        var user = new User
        {
            FirstName = request.FirstName,
            LastName = request.LastName,
            Email = request.Email
        };

        await _userRepository.CreateUser(user);

        return Unit.Value;
    }
}
```

**Step 4: Register the Command Handler in the Dependency Injection Container**

Next, we need to register the command handler in the dependency injection container. This can be done using the `AddMediatR` method provided by the MediatR library. Here is an example using the _Microsoft.Extensions.DependencyInjection_ library:

```Plain
public void ConfigureServices(IServiceCollection services)
{
    services.AddMediatR(typeof(CreateUserCommandHandler).Assembly);
    services.AddTransient<IUserRepository, UserRepository>();
}
```

**Step 5: Execute the Command**

Finally, we can execute the command by sending it to the mediator using the `Send` method provided by the MediatR library. Here is an example of executing the `CreateUserCommand`:

```Plain
var command = new CreateUserCommand
{
    FirstName = "John",
    LastName = "Doe",
    Email = "john.doe@example.com"
};

await _mediator.Send(command);
```

In conclusion, CQRS is an architectural pattern that separates the responsibilities of reading and writing data into two separate components. The MediatR library provides a simple and efficient way to implement CQRS in a .NET application. By following the steps outlined in this article, you can easily implement CQRS in your .NET application using the MediatR library.
