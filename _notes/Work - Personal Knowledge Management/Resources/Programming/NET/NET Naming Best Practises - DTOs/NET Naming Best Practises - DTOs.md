---
title: NET Naming Best Practises - DTOs
feed: show
---

[https://schwabencode.com/blog/2024/01/02/dotnet-naming-best-practises-dto](https://schwabencode.com/blog/2024/01/02/dotnet-naming-best-practises-dto)

[.NET Naming Best Practises: DTOs](https://schwabencode.com/blog/2024/01/02/dotnet-naming-best-practises-dto)

Often seen, often used incorrectly: DTOs. Data Transfer Objects.

In principle, DTO is an umbrella term, a design pattern that is used to transfer data between. In reality, however, the umbrella term is used directly as an implementation, which has disadvantages in all cases.

## DTO in ASP.NET Core

Let's take an ASP.NET Core API as an example in which a user object is to be transferred.

```Plain
/// Don't do this!
public class UserDTO
{
    public int UserId { get; set; }
    public string UserName { get; set; }
}
```

Looks like a legitimate object in principle - but for what?

As a response object, this structure makes sense - because I only have necessary properties and values that are directly assigned to the user and must exist.

However, if I were to use this structure for a create flow, i.e. an endpoint that is to create a user, then it makes no sense: because the caller should certainly not set an ID; the application (logic) is responsible for this.

In APIs in particular, it is very often the case that request and response models are structured very differently.

```Plain
/// Do this!
public class UserCreateRequestModel
{
    public string UserName { get; set; }
}

public class UserModel
{
    public int UserId { get; set; }
    public string UserName { get; set; }
}

// or if you have a special model for the create response
public class UserCreateResponseModel
{
    public int UserId { get; set; }
    public string UserName { get; set; }
}
```

Summary: every API should have separate `Request`- and `Response`-Models.

### Validation

Another major advantage of this separation is that each model can be structured and set up separately, and the validation - e.g. using FluentValidation - can also be set up individually.

This makes implementation much easier, especially in medium-sized and large applications.

## DTOs when reading and writing files

As with APIs, models are often used for files to read and write files or to work with the runtime object. However, while APIs should have separate models for requests and responses, this almost never makes sense for files - you only have one structure representation.

But here too, there is no need for the designation "DTO" to be part of the name - after all, you only want to enable the data to be displayed.

```Plain
/// Don't do this!
public class MySettingsDTO
{
    public string Field1 { get; set; }
    public string Field2 { get; set; }
    public MySubSettingDTO[] SubSettings1 { get; set; }
}

/// Do this!
public class ApplicationSettings
{
    public string Field1 { get; set; }
    public string Field2 { get; set; }
    public MySubSetting[] SubSettings1 { get; set; }
}
```

## Conclusion

In principle, all models for data transfer can be assigned to the DTO pattern - but you will not see "DTO" as part of the software design name.

It is particularly important that models are given precise names that make it clear what they are used for and that incorrect naming habits do not create a design flaw that is difficult to correct.
