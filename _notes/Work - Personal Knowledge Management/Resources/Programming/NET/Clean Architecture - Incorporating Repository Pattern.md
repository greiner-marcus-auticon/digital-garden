---
title: Clean Architecture - Incorporating Repository Pattern
feed: show
---

[https://link.medium.com/7XMiCanPezb](https://link.medium.com/7XMiCanPezb)

## Introduction

This document provides a detailed overview of how to incorporate a Repository Pattern into a Clean Architecture Pattern. It touches on as many common aspects you would have in your existing MVC pattern (Logging, Unit Testing, DI, EF Core, etc.).

## Purpose

The Product Architecture Document (PAD) provides a comprehensive architectural overview on how to integrate a database first approach into a Clean Architecture approach — what components are involved and how they are related to each other.

## Scope

The scope of this PAD is to convey the concepts needed to produce a Clean Architecture pattern with a Repository pattern.

## Prerequisites

- An understanding of the MVC or DDD architecture patterns
- An understanding of Database First approach (Repository Pattern)
- Knowledge of SQL Server — running SQL scripts
- Knowledge of .NET Core\6
- SQL Server (inc. SSMS) and Visual Studio installed (free+ editions)
- Basic knowledge of MSTest

## Solution\Environment Setup

## Codebase

Clone the code-repo using this link [https://github.com/Bert0Neill/CleanArchitectureDemo.git](https://github.com/Bert0Neill/CleanArchitectureDemo.git).

## SQL Database Script

## Visual Studio Extension

Download and install the **EF Core Power Tools** extension for Visual Studio from here -[https://marketplace.visualstudio.com/items?itemName=ErikEJ.EFCorePowerTools](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EFCorePowerTools)

## Components and Nuget Packages

To make the solution as realistic as possible, I have used the following packages and components that you would currently use with your existing architecture pattern — which have been incorporated into the solution projects, where they are used.

- Faker\Bogus
- MOQ
- MSTest
- IHttpFactory
- Http Polly (API retries)
- EF Core 6
- SQL Server database
- EF Power Core Tools
- Middleware (Exception Handling)
- Logging (file based)
- GuardRails
- Blazor WASM
- Benchmark.Net (performance)
- AutoMapper
- Swagger UI

## Quick Explanation of Clean Architecture

The concept of a Clean Architecture pattern has been around for over a decade and initially conceived by Robert Martin (better known as [Uncle Bob](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)). The keyword from Uncle Bob is **Interchangeable**. In the image below, everything on the blue circle is interchangeable, for e.g., the UI can be swapped out from Angular to React, or the database can be migrated from Oracle to MySQL, and nothing in the underlying layers need to change.

The concept of having all your interfaces (Infrastructure and Application) in one project, will make it easier to Unit Test and mock.

But the main rational behind Clean Architecture, is that MVC doesn’t scale or allow for the same loose coupling of the layers. In Clean Architecture, the dependency is inward facing only (this satisfies DI from SOLID principal). In MVC, the Model View acts as the UI and Controller layer in one, this can get very large and difficult to test (because of the tight coupling). MVC has served the software industry for over 20 years, but the industry wants a new leaner architecture pattern, for the next 20 years — one that is scalable\interchangeable\decoupled.

## Core (Domain and Application)

- The Application layer is a top layer or parent layer (of the core) which only depends on Domain layer. Other layers like Infrastructure or UI will depend on Application.
- Application contains ‘Interfaces’, ‘Business Logic’, etc.
- The Domain project mostly contains ‘Entities’, Custom Exceptions, Enumerations, etc.

## Infrastructure

Infrastructure deals with ‘Databases’, ‘External API Calls’, ‘Cache’, etc. Basically, infrastructure deals with all external resources. Infrastructure depends on the ‘Interface’ inside of Application. Because of the dependency inversion, our Application will be loosely coupled, which makes it easy to test.

## UI Application

UI Application consumes the ‘Application Core’ to produce the results. In a real-time scenario, UI Application never depends on the infrastructure layer, but we have to reference the infrastructure layer into the UI project in the case to register the services dependency injection. So UI project should not use any code of the infrastructure layer other than dependency injection.

## Visual Studio — Solution Structure

The solution is split up into **solution folders**, within each folder, there is a project (there can be multiple projects per layer depending on the size or complexity, thus splitting a layer into multiple projects may make more sense, maintenance wise):

- `CleanArchitecture.Domain` - 'Application Core' - A class library template project
- `CleanArchitecture.Application` - 'Application Core' - A class library template project
- `CleanArchitecture.Infrastructure` - 'Infrastructure' - A class library template project
- `CleanArchitecture.Api` - 'API' - A web API template project
- `CleanArchitecture.Blazor` - 'Web UI' - A web UI template project
- `CleanArchitecture.UnitTests` - 'Unit Test' - A class library template project

## Blazor Application Running

The image below display the final UI screen, which is a call from the client web application, to the API controller, which in turn calls the Application service (use cases\business logic), which calls the respective Infrastructure class to perform external tasks.

## Add Existing Database Entities and DBContext

## Entities

The database entities will be saved within the Domain project itself, right click the Domain project and select **EF Core Power Tools** from context menu, then select **Reverse Engineer**.

You will then be prompted to select the database you wish to reverse engineer the database models.

Next, you can select the appropriate database tables to generate the entities for:

Ensure to select **EntityTypes only** from the **What to generate** dropdown, as we only wish to create the entities and not the `DBContext` in the `Domain` project. Enter the path to create the entities, the namespace should be correct as you right clicked on the `Domain` project.

## DBContext

Right click on the Infrastructure project and again select **EF Core Power Tool**, to **Reverse Engineer** the `DBContext` (into the `Application` project).

Ensure to select **DbContext Only** from the dropdown and enter the path to where you would like the **DBContext class** generated:

This will then generate the structure below for the entities and the `DBContext`, in the appropriate projects:

## Connection String (API Project)

The connection string is placed in the **Programs class of the API project**, because we are consuming the `Application` project, which in turn will need the Infrastructure classes injected that will use the `DBContext`.

Add a reference to the `DBContext` (`MusicContext` from the Infrastructure project) into your _Program.cs_ class, as it is only allowing DI to be performed into the `Application` classes.

An entry in the _Appsettings_ file to the connection string:

## Clean Architecture — Our Solution’s Main Layers

The Clean Architecture approach is very diversifiable, in that it can cater for any design pattern you wish to use (Factory with Decorator, for e.g.), but in our example, I am using the common repository pattern approach.

## Domain (Class Library)

The domain project will host enterprise wide entities\models, custom Exceptions, Enumerations, etc., but it has no dependencies, no project or class reference, no business logic, etc.

**Tip** — The term ‘`Entity`’ comes from the SQL Server ‘Id**entity**’ property – meaning that the entity must have a primary key.

## Application — Use Case\Business Logic (Class Library)

Consider these services as your application’s business logic\use case layer, a pass through from the UI to the Domain and performing the necessary application logic needed for your solution. The Application layer will consume the Domain models, but use the Infrastructure layer to communicate with the outside world, thus using those results to perform its business logic (slicing and dicing results from multiple Infrastructure calls, which in turn gets passed back to the client as (for e.g.) a DTO).

**NB**: Only Domain is added as reference project.

## Infrastructure (Class Library)

These classes are responsible for external infrastructure communications like database storage, file system, external systems/APIs/Services and so on. We can add more class libraries under this project folder for external plug-ins or SDKs.

In essence, the Infrastructure layer is technically not needed, as you could design an application that doesn’t interact with the outside world, and does all it own business logic, this would certainly be the exception to the norm!

It is the outermost layer of the system and should have no knowledge of the inner layers.

**NB**: Application Class is added as reference.

**NB**: The UI Application never depends on the infrastructure layer, but we have to reference the infrastructure layer into the UI project in the case to register the services dependency injection. So UI project should not use any code of the infrastructure layer other than dependency injection.

The Application layer needs the Infrastructure classes injected, so this has to come from the Web API layer (the UI layer makes no use of Infrastructure, only to DI into Application layer).

The API’s project _Programs.cs_ — configuring Infrastructure DI:

Application service consuming the injected Infrastructure classes:

In the image below, you can see the inward reference structure between the projects. The API project must reference the Application layer so that it can makes calls to the various business logic services (it references the Infrastructure for DI purposes only).

## Application Layer — Interfaces

One of the main characteristics of a Clean Architecture approach is having ALL your interfaces in one place, namely the Application project. This indicates that only this layer needs to be mocked put when generating unit tests for other parts of the solution. All you’re mocking in one place makes for easier unit testing.

So, below I am generating an interface for the Infrastructure Repository class (in the Infrastructure project) and then moving it to the Application project.

**NB**: It’s going to create the interface by default in the `Infrastructure` class, we will just move it after it has been created.

## DTOs

Although DTOs are in themselves entities, and thus you may be thinking that they should go into the `Domain` project, the rule of thumb with DTOs in any project, is that they should go into the project where they are (most) used. For web related solutions, this would be the API layer – as data coming from the client will be a DTO and data going to the client will be mapped entities (into DTOs).

But another strong arguments for putting the DTOs close to where they are used, is that if they go into the `Domain` project, they are then set with the same structure even if a new API wishes to use the Application service – thus to make the API\Client more flexible, the `Domain` shouldn’t be dictating the format of the data the client will receive, that should be left up to each individual API project.

## Swagger Enabled (Launch Settings API)

After adding the Swagger packages to the API project, you can configure you project as follows:

Alter your _Programs.cs_ class to take account of your Swagger settings:

## Polly Retry Policy

Polly is configured within the Blazor UI application _Program.cs_ file, below I am using the _appsettings.json_ file to pickup various settings (retry count and base URL) and assigning them to each API call — below I am stating to retry the API call a max of 5 times, plus it jitters the retries, so as not to overwhelm the server with a call right away.

## Testing Polly

Only run the Blazor application (not in conjunction with the API), then call the Fetch Album data option again from the left hand menu — notice that it will retry 5 times (the 5 coming from our appsetting) before giving up.

## Logging (Server Side)

Configure NLog to how you wish to log your data; below I am just setting it up in the API project to log everything to a log file (using the _nlog.file_ on the project root).

Configuring NLog within _Programs.cs_:

Example output:

## Unit Tests — MSTest\Faker\Bogus

Below, I am configuring the unit tests with mock data (using the Bogus package), where I am faking the data returned from the Repository calls, thus I can unit test the business logic\use cases held within the Application project — you can view the unit test code in the solution project:
