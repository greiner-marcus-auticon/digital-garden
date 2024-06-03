---
title: 12 Bad Practices to Avoid in ASP.NET Core API Controllers
feed: show
---

[https://levelup.gitconnected.com/12-bad-practices-to-avoid-in-asp-net-core-api-controllers-3ba52a10954e](https://levelup.gitconnected.com/12-bad-practices-to-avoid-in-asp-net-core-api-controllers-3ba52a10954e)

![[assets/1eQjyva0NPFrFajInkZDfqQ.png]]

When developing controllers in ASP.NET Core, there are certain practices that should be avoided to ensure maintainability, performance, and adherence to best practices. Here are 12 things we should avoid in our controllers.

## 1. Tight coupling

Avoid tightly coupling in controllers with specific dependencies or frameworks. Instead, use dependency injection to inject dependencies into controllers. This promotes loose coupling and makes the code more testable and maintainable.

```Plain
// Avoid:
[ApiController]
[Route("[controller]")]
public class ProductController : ControllerBase
{
    ProductService productService = new ProductService();
    // ...
}


// Prefer:
[ApiController]
[Route("[controller]")]
public class ProductController : ControllerBase
{
    private readonly ILogger<ProductController> _logger;
    private readonly IProductService _productService;

    public ProductController(IProductService productService, ILogger<ProductController> logger)
    {
        _logger = logger;
        _productService = productService;
    }
    // ...
}
```

_Learn more about_ [_dependency injection_](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-7.0)

## 2. Mixing concerns

Controllers should focus on handling HTTP requests and generating responses. Avoid mixing concerns such as data access, authentication, or authorization directly in the controller. Delegate these responsibilities to separate classes or middleware.

```Plain
// Avoid:
[HttpPost]
public async Task<IActionResult> CreateProduct(ProductDto productDto)
{
    // Authentication and authorization logic here
    // ...
    // Data access logic here
    // ...
    return Ok(StatusCodes.Status201Created);
}


// Prefer:
[HttpPost]
[Authorize]
public async Task<IActionResult> CreateProduct(ProductDto productDto)
{
    await _productService.CreateAsync(productDto);

    return Ok(StatusCodes.Status201Created);
}
```

_Learn more about the_ [_Single-responsibility principle_](https://en.wikipedia.org/wiki/Single-responsibility_principle) _|_ [_Separation of concerns_](https://learn.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/architectural-principles)

## 3. Lack of exception handling

Controllers should handle exceptions gracefully and return appropriate error responses. Avoid letting exceptions propagate to the client without proper handling. Implement global…
