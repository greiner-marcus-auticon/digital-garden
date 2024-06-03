---
title: Configuring Serilog through appsettings.json file
feed: show
---

[https://mohsen.es/configuring-serilog-through-appsettings-json-file-33b26594bb46](https://mohsen.es/configuring-serilog-through-appsettings-json-file-33b26594bb46)

![[assets/1WdXcrIjj_nocCnsnTfI9CQ.jpeg]]

In this post, I’m going to show you how to configure Serilog via the configuration file. Changing the logging configuration without touching the codebase is really helpful, especially in the production environment. First why [Serilog](https://serilog.net/)? It is easy to set up, has a clean API, and is portable between recent .NET platforms. The big difference between Serilog and the other frameworks is that it is designed to do structured logging out of the box. Another thing I really like about Serilog is that it can be configured via the `appsetting.json` file alongside configuring through code.

Let’s start with creating a new project.

Create a new ASP.NET 7.0 Web API project.

Install [Serilog.AspNetCore](https://www.nuget.org/packages/Serilog.AspNetCore) nuget package.

Open `Program.cs` file and add the following code:

```Plain
builder.Host.UseSerilog((hostingContext, loggerConfiguration) =>
    loggerConfiguration.ReadFrom.Configuration(hostingContext.Configuration));
```

`UseSerilog` sets Serilog as the logging provider. We are going to config Serilog via the `appsettings.json` file.

Open `appsettings.json` and `appsettings.Development.json` files and get rid of the logging section:

```Plain
"Logging": {
  "LogLevel": {
    "Default": "Information",
    "Microsoft": "Warning",
    "Microsoft.Hosting.Lifetime": "Information"
  }
}
```

Add Serilog configuration section to `appsettings.Development.json` file:

```Plain
"Serilog": {
  "MinimumLevel": {
    "Default": "Debug",
    "Override": {
      "Microsoft": "Information",
      "System": "Information"
    },
    "Using": [ ],
  },
  "WriteTo": [
    { }
  ]
}
```

`Serilog.AspNetCore` nuget package has a dependency on [Serilog.Settings.Configuration](https://github.com/serilog/serilog-settings-configuration) nuget package and is a Serilog settings provider that reads from `Microsoft.Extensions.Configuration` sources. The above configuration is equivalent to this:

```Plain
Log.Logger = new LoggerConfiguration()
    .MinimumLevel.Debug()
    .MinimumLevel.Override("Microsoft", LogEventLevel.Information)
    .MinimumLevel.Override("System", LogEventLevel.Information)
    .Enrich.FromLogContext()
    .CreateLogger();
```

Serilog uses [sinks](https://github.com/serilog/serilog/wiki/Provided-Sinks) to write log events to storage for example database, file, etc. One of the most popular sinks for the debugging environment is the [Console](https://github.com/serilog/serilog-sinks-console) sink.

- Install `Serilog.Sinks.Console`nuget package
- Add the following configuration:

```Plain
"Serilog": {
  "MinimumLevel": {
    "Default": "Debug",
    "Override": {
      "Microsoft": "Information",
      "System": "Information"
    }
  },
  "Using": [ "Serilog.Sinks.Console" ],
  "WriteTo": [
    { "Name": "Console" }
  ]
}
```

In the `Using` section add the sink's nuget package name `"Using": [ "Serilog.Sinks.Console" ]`

- In the `WriteTo` section add sink name and arguments `"WriteTo":[ { "Name": "Console" } ]`

Run the project and in the console window you should see logs like below

Now we want to use SQL Server sink for other environments:

- Install [Serilog.Sinks.MSSqlServer](https://www.nuget.org/packages/serilog.sinks.mssqlserver) sink
- Copy Serilog setting form `appsettings.Development.json` to `appsettings.json` file

```Plain
"Serilog": {
  "MinimumLevel": {
    "Default": "Information",
    "Override": {
      "Microsoft": "Error",
      "System": "Error"
    },
    "Using": [ "Serilog.Sinks.MSSqlServer" ]
  },
  "WriteTo": [
    {
      "Name": "MSSqlServer",
      "Args": {
        "connectionString": "ConnectionString",
        "tableName": "Logs",
        "autoCreateSqlTable": true
      }
    }
  ]
}
```

Well, configuring a sink via `appsettings.json` could be harder than configuring through the code, and for each sink, you might not be able to find a JSON configuration sample. Normally each sink accepts several parameters to configure the sink. For instance, the Console sink accepts the below parameters:

Each one of these parameters can be configured through JSON:

```Plain
"WriteTo": [
  {
    "Name": "Console",
    "Args": {
      "restrictedToMinimumLevel": "Verbose",
      "outputTemplate": "[{Timestamp:HH:mm:ss} {Level:u3}] {Message:lj} <s:{SourceContext}>{NewLine}{Exception}",
      "theme": "Serilog.Sinks.SystemConsole.Themes.AnsiConsoleTheme::Code, Serilog.Sinks.Console"
    }
  }
]
```

After setting the console sink parameters, logs should be like the following image:

To see the complete SQL Server sink JSON configuration check out this [appsettings.json](https://github.com/serilog-mssql/serilog-sinks-mssqlserver/blob/dcdb0cb87ad8bdc2f94229579acb57c3abfdfa3c/sample/WorkerServiceDemo/appsettings.json) file.

Log events can be enriched with properties in various ways. You can add additional data by enrichers. For instance, in the production environment, we want to add the IP of the client to the log events.

- Install [Serilog.Enrichers.ClientInfo](https://www.nuget.org/packages/Serilog.Enrichers.ClientInfo) package
- Add enriched package name to `Using` section
- Add `Enrich` section with `WithClientIp` value (enriched name normally starts with `With` word)

```Plain
"Using": [ "Serilog.Sinks.MSSqlServer", "Serilog.Enrichers.ClientInfo" ],
"Enrich": [ "WithClientIp" ]
```

All events written through the logger will carry a property `ClientIp` with the IP of the client. Check out the list of available enrichers [here](https://github.com/serilog/serilog/wiki/Enrichment#available-enricher-packages).

By using filters you can exclude or include some log events.

- Install `Serilog.Expressions` nuget package
- Add the “Filter” section to Serilog settings

```Plain
"Filter": [
  {
    "Name": "ByExcluding",
    "Args": {
      "expression": "RequestPath like '%swagger%'"
    }
  }
]
```

All log events that contain `swagger` will be excluded.

Moreover, you can log the HTTP requests. `UseSerilogRequestLoging` extension adds middleware for streamlined request logging. Instead of writing HTTP request information like method, path, timing, status code and exception details in several events, this middleware collects information during the request (including from `Serilog.IDiagnosticContext`), and writes a single event at request completion.

- In `Program.cs` file, add the following code:

```Plain
var app = builder.Build();
app.UseSerilogRequestLogging();
```

- In `MinimumLevel.Override` section add `"Microsoft.AspNetCore": "Warning"`:

```Plain
"MinimumLevel": {
  "Default": "Information",
  "Override": {
    "Microsoft": "Error",
    "Microsoft.AspNetCore": "Warning",
    "System": "Error"
  },
```

Last but not least here I want to mention that you can override the Serilog setting by the Docker environment variable. Consider the following configuration:

```Plain
"Serilog": {
  "MinimumLevel": {
    "Default": "Information",
    "Override": {
      "Microsoft": "Error",
      "System": "Error"
    },
    "Using": [ "Serilog.Sinks.MSSqlServer" ]
  },
  "WriteTo": [
    {
      "Name": "MSSqlServer",
      "Args": {
        "connectionString": "",
        "tableName": "Logs",
        "autoCreateSqlTable": true
      }
    }
  ]
}
```

Now in the docker-compose file, we want to pass the actual connection string:

```Plain
my-api:
    environment:
      - ASPNETCORE_ENVIRONMENT=Production
      - Serilog__MinimumLevel__Default=Warning
      - Serilog__WriteTo__0__Args__connectionString="Your connection string"
```

The value of each section can be accessed by `__`, for instance, `Serilog__MinimumLevel__Default` is equivalent to:

```Plain
"Serilog": {
  "MinimumLevel": {
    "Default": "",
```

In a section to access an item inside the array, use the item index number. `"WriteTo"` section accepts an array of sinks configuration. If you are using two sinks use `Serilog__WriteTo__0__` to access the first sink and `Serilog__WriteTo__1__` to access the second sink.

Let’s do a simple test. Open a `CMD` or `Powershell` at the project directory:

- Type `dotnet add package Serilog.Sinks.File` to install File sink
- Open `appsettings.josn` file and change the logging configuration like this:

```Plain
"Serilog": {
  "MinimumLevel": {
    "Default": "Debug",
    "Override": {
      "Microsoft": "Information",
      "System": "Information"
    }
  },
  "Using": [ "Serilog.Sinks.Console", "Serilog.Sinks.File" ],
  "WriteTo": [
    { "Name": "Console" },
    {
      "Name": "File",
      "Args": {
        "path": "log.txt",
        "rollingInterval": "Day"
      }
    }
  ]
}
```

- Type `dotnet build` then `dotnet run`
- After running the application you should see a log file inside the project directory

The source code for this walkthrough could be found on [GitHub](https://github.com/mo-esmp/cool-webapi).
