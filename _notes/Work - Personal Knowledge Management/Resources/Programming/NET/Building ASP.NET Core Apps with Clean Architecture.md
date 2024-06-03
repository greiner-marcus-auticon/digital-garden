---
title: Building ASP.NET Core Apps with Clean Architecture
feed: show
---

[https://waqasahmeddev.medium.com/building-asp-net-core-apps-with-clean-architecture-724cea7b0e49](https://waqasahmeddev.medium.com/building-asp-net-core-apps-with-clean-architecture-724cea7b0e49)

Clean Architecture is a software design pattern that focuses on separation of concerns, testability, maintainability, and scalability. The architecture has four layers:

1. **Presentation Layer (PL)** — This layer contains the user interface and the code to handle user requests. In our case, it will be the ASP.NET Core web application.
2. **Application Layer (AL)** — This layer contains the business logic that is independent of the user interface. It coordinates the application’s data flow between the presentation layer and the domain layer.
3. **Domain Layer (DL)** — This layer contains the core business logic and entities of the application. It should be agnostic of the implementation details of other layers.
4. **Infrastructure Layer (IL)** — This layer contains the implementation details such as data access, network communication, and logging. It should be agnostic of the business logic.

Here’s an example of how to implement a Bookstore application with Clean Architecture in ASP.NET Core:

## 1. Presentation Layer (PL)

Create an ASP.NET Core web application and add the following folders and files to the project:

- Controllers folder — contains the controllers that handle user requests
- Views folder — contains the Razor views that render the HTML pages
- ViewModels folder — contains the view models that represent the data displayed in the views

## 2. Application Layer (AL)

Create a separate project for the application layer and add the following folders and files to the project:

- Interfaces folder — contains the interfaces for the application services
- Services folder — contains the implementations of the application services

Add the following interface in the Interfaces folder:

```Plain
public interface IBookService
{
    List<BookViewModel> GetBooks();
    BookViewModel GetBook(int id);
    void AddBook(BookViewModel book);
    void UpdateBook(BookViewModel book);
    void DeleteBook(int id);
}
```

Add the following implementation in the Services folder:

```Plain
public class BookService : IBookService
{
    private readonly IBookRepository _bookRepository;
    private readonly IMapper _mapper;

    public BookService(IBookRepository bookRepository, IMapper mapper)
    {
        _bookRepository = bookRepository;
        _mapper = mapper;
    }

    public List<BookViewModel> GetBooks()
    {
        var books = _bookRepository.GetBooks();
        return _mapper.Map<List<BookViewModel>>(books);
    }

    public BookViewModel GetBook(int id)
    {
        var book = _bookRepository.GetBook(id);
        return _mapper.Map<BookViewModel>(book);
    }

    public void AddBook(BookViewModel book)
    {
        var bookEntity = _mapper.Map<Book>(book);
        _bookRepository.AddBook(bookEntity);
    }

    public void UpdateBook(BookViewModel book)
    {
        var bookEntity = _mapper.Map<Book>(book);
        _bookRepository.UpdateBook(bookEntity);
    }

    public void DeleteBook(int id)
    {
        _bookRepository.DeleteBook(id);
    }
}
```

## 3. Domain Layer (DL)

Create a separate project for the domain layer and add the following folders and files to the project:

- Entities folder — contains the entity classes that represent the domain objects
- Interfaces folder — contains the interfaces for the repositories

Add the following entity class in the Entities folder:

```Plain
public class Book
{
    public int Id { get; set; }
    public string Title { get; set; }
    public string Author { get; set; }
    public decimal Price { get; set; }
}
```

Add the following interface in the Interfaces folder:

```Plain
public interface IBookRepository
{
    List<Book> GetBooks();
    Book GetBook(int id);
    void Add(Book book);
    void UpdateBook(Book book);
    void DeleteBook(int id);
}
```

## 4. Infrastructure Layer (IL)

Create a separate project for the infrastructure layer and add the following folders and files to the project:

- Data folder — contains the code to access the data store
- Mappings folder — contains the AutoMapper mappings

Add the following implementation in the Data folder:

```Plain
public class BookRepository : IBookRepository
{
    private readonly List<Book> _books = new List<Book>
    {
        new Book { Id = 1, Title = "Clean Code", Author = "Robert C. Martin", Price = 30.00m },
        new Book { Id = 2, Title = "Code Complete", Author = "Steve McConnell", Price = 25.00m },
        new Book { Id = 3, Title = "Refactoring", Author = "Martin Fowler", Price = 27.00m }
    };

    public List<Book> GetBooks()
    {
        return _books;
    }

    public Book GetBook(int id)
    {
        return _books.FirstOrDefault(b => b.Id == id);
    }

    public void AddBook(Book book)
    {
        _books.Add(book);
    }

    public void UpdateBook(Book book)
    {
        var existingBook = _books.FirstOrDefault(b => b.Id == book.Id);
        if (existingBook != null)
        {
            existingBook.Title = book.Title;
            existingBook.Author = book.Author;
            existingBook.Price = book.Price;
        }
    }

    public void DeleteBook(int id)
    {
        var existingBook = _books.FirstOrDefault(b => b.Id == id);
        if (existingBook != null)
        {
            _books.Remove(existingBook);
        }
    }
}
```

Add the following mapping in the Mappings folder:

```Plain
public class MappingProfile : Profile
{
    public MappingProfile()
    {
        CreateMap<Book, BookViewModel>().ReverseMap();
    }
}
```

## 5. Dependency Injection

In the ASP.NET Core Startup.cs file, add the following code to configure the dependency injection:

```Plain
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();

    services.AddScoped<IBookRepository, BookRepository>();
    services.AddScoped<IBookService, BookService>();

    services.AddAutoMapper(typeof(MappingProfile));
}
```

## 6. Usage

In the ASP.NET Core Controllers, inject the `IBookService` and use it to handle user requests:

```Plain
public class BookController : Controller
{
    private readonly IBookService _bookService;

    public BookController(IBookService bookService)
    {
        _bookService = bookService;
    }

    public IActionResult Index()
    {
        var books = _bookService.GetBooks();
        return View(books);
    }

    public IActionResult Details(int id)
    {
        var book = _bookService.GetBook(id);
        return View(book);
    }

    [HttpGet]
    public IActionResult Create()
    {
        return View();
    }

    [HttpPost]
    public IActionResult Create(BookViewModel book)
    {
        if (ModelState.IsValid)
        {
            _bookService.AddBook(book);
            return RedirectToAction("Index");
        }

        return View(book);
    }

    [HttpGet]
    public IActionResult Edit(int id)
    {
        var book = _bookService.GetBook(id);
        return View(book);
    }

    [HttpPost]
    public IActionResult Edit(BookViewModel book)
    {
        if (ModelState.IsValid)
        {
            _bookService.UpdateBook(book);
            return RedirectToAction("Index");
        }

        return View(book);
    }

    [HttpGet]
    public IActionResult Delete(int id)
    {
        var book = _bookService.GetBook(id);
        return View(book);
    }

    [HttpPost]
    public IActionResult DeleteConfirmed(int id)
    {
        _bookService.DeleteBook(id);
        return RedirectToAction("Index");
    }
}
```

## 7. ASP.NET Core Views

Add the following Views for the `Book` entity:

- Index.cshtml — displays a list of all books
- Details.cshtml — displays details for a single book
- Create.cshtml — displays a form to create a new book
- Edit.cshtml — displays a form to edit an existing book
- Delete.cshtml — displays a confirmation message to delete a book

Here’s an example of the Index.cshtml view:

```Plain
@model List<BookViewModel>

<h2>Book List</h2>

<table class="table">
    <thead>
        <tr>
            <th>Title</th>
            <th>Author</th>
            <th>Price</th>
            <th></th>
        </tr>
    </thead>
    <tbody>
        @foreach (var book in Model)
        {
            <tr>
                <td>@book.Title</td>
                <td>@book.Author</td>
                <td>@book.Price</td>
                <td>
                    <a asp-action="Details" asp-route-id="@book.Id">Details</a> |
                    <a asp-action="Edit" asp-route-id="@book.Id">Edit</a> |
                    <a asp-action="Delete" asp-route-id="@book.Id">Delete</a>
                </td>
            </tr>
        }
    </tbody>
</table>

<a asp-action="Create" class="btn btn-primary">Create New</a>
```

## 8. **Run the Application**

Run the ASP.NET Core application and navigate to the `/book` route to see the list of books. You can also navigate to the `/book/create` route to create a new book, or click on the "Details", "Edit", or "Delete" links for each book to perform those actions.

Before running the application, make sure that the database is created and up-to-date. You can do this by running the following commands in the Package Manager Console:

```Plain
Update-Database
```

Once the database is ready, you can run the application by pressing F5 in Visual Studio or by running the following command in the terminal:

```Plain
dotnet run
```

This will start the application and launch it in the default web browser. Navigate to the `/book` route to see the list of books. You can also navigate to the `/book/create` route to create a new book, or click on the "Details", "Edit", or "Delete" links for each book to perform those actions.

Congratulations! You have successfully implemented a clean architecture using ASP.NET Core and created a simple bookstore application. You can continue to build upon this application by adding more features, such as user authentication, search functionality, or the ability to add reviews for each book.

Dependency injection (DI) is a fundamental aspect of clean architecture and is essential for building scalable and maintainable applications. In ASP.NET Core, DI is built-in and provides a simple way to register and resolve dependencies.

To implement DI in our bookstore application, we need to do the following:

1. Register Services We need to register the services that our application depends on in the `ConfigureServices` method of the `Startup` class. For example, we need to register the `IBookService` interface with its implementation `BookService`. We can do this using the `AddTransient`, `AddScoped`, or `AddSingleton` methods, depending on our requirements. Here's an example:

    ```Plain
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddDbContext<BookstoreContext>(options =>
            options.UseSqlServer(Configuration.GetConnectionString("BookstoreConnection")));

        services.AddTransient<IBookService, BookService>();

        services.AddControllersWithViews();
    }
    ```

2. Resolve Dependencies We can use constructor injection to resolve dependencies in our controllers. For example, in the `BookController`, we can inject the `IBookService` interface in the constructor:

    ```Plain
    public class BookController : Controller
    {
        private readonly IBookService _bookService;

        public BookController(IBookService bookService)
        {
            _bookService = bookService;
        }

        // actions...
    }
    ```

3. Use Dependencies Once we have resolved the dependencies, we can use them in our controller actions. For example, in the `Index` action of the `BookController`, we can use the `_bookService` to get a list of books:

```Plain
public IActionResult Index()
{
    var books = _bookService.GetBooks();
    var bookViewModels = _mapper.Map<List<BookViewModel>>(books);

    return View(bookViewModels);
}
```

That’s it! We have now implemented dependency injection in our application. By using DI, we have decoupled our application components and made them more testable, maintainable, and scalable.

Implementing security in an application is crucial to protect user data and prevent unauthorized access. In an ASP.NET Core application, you can implement security by using authentication and authorization.

Authentication is the process of verifying the identity of a user, while authorization is the process of determining whether a user has the necessary permissions to access a resource.

Here’s how you can implement authentication and authorization in your bookstore application:

1. Authentication To implement authentication, we need to configure the authentication middleware in the `Configure` method of the `Startup` class. We can use the `AddAuthentication` and `AddCookie` methods to enable cookie-based authentication. Here's an example:

    ```Plain
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        //...

        app.UseAuthentication();
        app.UseAuthorization();

        app.UseEndpoints(endpoints =>
        {
            endpoints.MapControllerRoute(
                name: "default",
                pattern: "{controller=Home}/{action=Index}/{id?}");
        });
    }
    ```

    In the `AccountController`, we can add an action method to handle the login process. We can use the `SignInManager` and `UserManager` classes to authenticate the user and create a cookie with the user's claims. Here's an example:

    ```Plain
    public class AccountController : Controller
    {
        private readonly UserManager<ApplicationUser> _userManager;
        private readonly SignInManager<ApplicationUser> _signInManager;

        public AccountController(UserManager<ApplicationUser> userManager, SignInManager<ApplicationUser> signInManager)
        {
            _userManager = userManager;
            _signInManager = signInManager;
        }

        [HttpGet]
        public IActionResult Login()
        {
            return View();
        }

        [HttpPost]
        public async Task<IActionResult> Login(LoginViewModel model)
        {
            if (!ModelState.IsValid)
            {
                return View(model);
            }

            var result = await _signInManager.PasswordSignInAsync(model.Email, model.Password, model.RememberMe, false);

            if (result.Succeeded)
            {
                return RedirectToAction("Index", "Book");
            }

            ModelState.AddModelError("", "Invalid login attempt.");
            return View(model);
        }
    }
    ```

2. Authorization To implement authorization, we can use the `[Authorize]` attribute to decorate the actions or controllers that require authorization. We can also use roles or policies to control access to resources based on the user's identity or claims.

Here’s an example of how to use the `[Authorize]` attribute to protect the `BookController`:

```Plain
[Authorize]
public class BookController : Controller
{
    // actions...
}
```

We can also use the `Authorize` middleware to configure authorization policies in the `ConfigureServices` method of the `Startup` class. Here's an example:

```Plain
public void ConfigureServices(IServiceCollection services)
{
    //...

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdminRole", policy => policy.RequireRole("Admin"));
    });

    //...
}
```

We can use the `Authorize` attribute with a policy name to protect the actions or controllers that require a specific policy:

```Plain
[Authorize(Policy = "RequireAdminRole")]
public IActionResult Create()
{
    //...
}
```

That’s it! By implementing authentication and authorization, you have added an extra layer of security to your bookstore application.
