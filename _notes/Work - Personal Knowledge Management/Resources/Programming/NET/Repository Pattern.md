---
title: Repository Pattern
feed: show
---

[https://link.medium.com/cc9yzn2Oezb](https://link.medium.com/cc9yzn2Oezb)

In today’s software development industry, building APIs has become a fundamental part of many applications. APIs provide a way to expose data and services to external systems, and they help to create a loosely coupled architecture that enables different systems to communicate seamlessly. Building a robust and scalable API is essential for delivering a successful application, and using the right patterns and technologies can help developers achieve this goal. In this article, we will explore how to build a .Net Core Web API with the Repository Pattern.

The Repository Pattern is a design pattern that separates the logic that retrieves the data and maps it to the entity model from the business logic that acts on the model. The Repository Pattern is a widely used pattern in the .NET ecosystem, and it has been around for many years. It helps to create a clean separation of concerns and enables better testability, maintainability, and scalability.

To begin, we need to create a new .Net Core Web API project in Visual Studio. Once the project is created, we need to add the necessary NuGet packages for our project. We will be using Entity Framework Core as our ORM, so we need to add the following packages:

- Microsoft.EntityFrameworkCore
- Microsoft.EntityFrameworkCore.Design
- Microsoft.EntityFrameworkCore.SqlServer
- Microsoft.EntityFrameworkCore.Tools

We also need to add the Newtonsoft.Json package for serializing and deserializing JSON data.

```Plain
using Microsoft.EntityFrameworkCore;

namespace MyWebApi.Models
{
    public class AppDbContext : DbContext
    {
        public AppDbContext(DbContextOptions<AppDbContext> options)
            : base(options)
        {
        }

        public DbSet<Customer> Customers { get; set; }
        public DbSet<Order> Orders { get; set; }
    }
}
```

Next, we need to create our repositories. We can create a new folder called Repositories and create a class for each entity we want to access. Each repository class should inherit from a generic Repository base class, which will provide the basic CRUD operations for our entities. The generic Repository class should have a constructor that takes in an instance of the AppDbContext class.

Next, we can create our generic Repository base class:

```Plain
using Microsoft.EntityFrameworkCore;
using System;
using System.Collections.Generic;
using System.Linq;

namespace MyWebApi.Repositories
{
    public abstract class Repository<T> : IRepository<T> where T : class
    {
        protected readonly DbContext _context;
        protected readonly DbSet<T> _entities;

        public Repository(DbContext context)
        {
            _context = context;
            _entities = context.Set<T>();
        }

        public virtual IQueryable<T> GetAll()
        {
            return _entities.AsQueryable();
        }

        public virtual IEnumerable<T> Find(Func<T, bool> predicate)
        {
            return _entities.Where(predicate);
        }

        public virtual T GetById(int id)
        {
            return _entities.Find(id);
        }

        public virtual void Add(T entity)
        {
            _entities.Add(entity);
        }

        public virtual void Update(T entity)
        {
            _context.Entry(entity).State = EntityState.Modified;
        }

        public virtual void Delete(T entity)
        {
            _entities.Remove(entity);
        }
    }
}
```

Then, we can create our CustomerRepository class that inherits from the Repository base class:

```Plain
using Microsoft.EntityFrameworkCore;
using MyWebApi.Models;
using System.Collections.Generic;
using System.Linq;

namespace MyWebApi.Repositories
{
    public class CustomerRepository : Repository<Customer>, ICustomerRepository
    {
        public CustomerRepository(AppDbContext context)
            : base(context)
        {
        }

        public IEnumerable<Customer> GetCustomersWithOrders()
        {
            return _entities.Include(c => c.Orders).ToList();
        }
    }
}
```

Now that we have our repositories set up, we can create our Web API controllers. In the controllers, we can use dependency injection to inject instances of our repositories into our controller constructors. This enables us to access our data from the repositories and perform any necessary business logic.

Finally, we can create our Web API controller that injects an instance of the CustomerRepository:

```Plain
using Microsoft.AspNetCore.Mvc;
using MyWebApi.Models;
using MyWebApi.Repositories;

namespace MyWebApi.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class CustomersController : ControllerBase
    {
        private readonly ICustomerRepository _customerRepository;

        public CustomersController(ICustomerRepository customerRepository)
        {
            _customerRepository = customerRepository;
        }

        [HttpGet]
        public IActionResult GetAll()
        {
            var customers = _customerRepository.GetAll();
            return Ok(customers);
        }

        [HttpGet("{id}")]
        public IActionResult GetById(int id)
        {
            var customer = _customerRepository.GetById(id);
            if (customer == null)
            {
                return NotFound();
            }
            return Ok(customer);
        }

        [HttpPost]
        public IActionResult Create(Customer customer)
        {
            _customerRepository.Add(customer);
            _customerRepository.Save();
            return Ok(customer);
        }

        [HttpPut("{id}")]
        public IActionResult Update(int id, Customer customer)
        {
            var existingCustomer = _customerRepository.GetById(id);
            if (existingCustomer == null)
            {
                return NotFound();
            }
            existingCustomer.Name = customer.Name;
            existingCustomer.Email = customer.Email;
            _customerRepository.Update(existingCustomer);
            _customerRepository.Save();
            return Ok(existingCustomer);
        }

        [HttpDelete("{id}")]
        public IActionResult Delete(int id)
        {
            var existingCustomer = _customerRepository.GetById(id);
            if (existingCustomer == null)
            {
                return NotFound();
            }
            _customerRepository.Delete(existingCustomer);
            _customerRepository.Save();
            return Ok();
        }

        [HttpGet("orders")]
        public IActionResult GetCustomersWithOrders()
        {
            var customers = _customerRepository.GetCustomersWithOrders();
            return Ok(customers);
        }
    }
}
```

In this code, we have created an abstract base class `Repository<T>` that implements the basic CRUD operations for any entity of type `T`. Then, we have created a `CustomerRepository` class that inherits from `Repository<Customer>` and adds a custom method to retrieve customers with their orders. Finally, we have created a `CustomersController` that injects an instance of the `ICustomerRepository` interface and implements the standard CRUD operations for customers as well as the custom operation to retrieve customers with their orders.

This is just a basic example, and you can modify and extend this pattern to suit the needs of your application. The Repository Pattern can help you separate the concerns of data access from the rest of your application and make it easier to maintain and test your code.

In the Web API controller methods, we can use the repository methods to retrieve, create, update, or delete data. We can also map our entities to DTOs (Data Transfer Objects) to expose only the necessary data to the client.

Using the Repository Pattern with .Net Core Web API provides a clean and structured way of accessing data in a database. The Repository Pattern allows for better testability, maintainability, and scalability of our application. It separates the concerns of data access and business logic and makes it easier to modify and extend our application in the future.

In conclusion, building a .Net Core Web API with the Repository Pattern is a great way to create a robust and scalable API. The Repository Pattern separates the concerns of data access and business logic, and it enables better testability, maintainability, and scalability. By using .Net Core and Entity Framework Core, we can create a modern and efficient API that can handle a large number of requests and provide a seamless experience for our users.
