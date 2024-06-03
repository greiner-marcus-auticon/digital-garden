---
title: How to eliminate injecting ILogger everywhere in .NET
feed: show
---

[https://github.com/TobiStr/LoggerElimination](https://github.com/TobiStr/LoggerElimination)

## Logger Elimination

## How to eliminate injecting ILogger everywhere in .NET

Simply create a static class, that holds a Dictionary<Type, ILogger> and initialize a static LoggerFactory from your Startup Logic.

```Plain
public static class StaticLoggerFactory
{
 private static ILoggerFactory _loggerFactory;

 private static ConcurrentDictionary<Type, ILogger> loggerByType = new();

 public static void Initialize(ILoggerFactory loggerFactory)
 {
 if (_loggerFactory is not null)
 throw new InvalidOperationException("StaticLogger already initialized!");

 _loggerFactory = loggerFactory ?? throw new ArgumentNullException(nameof(loggerFactory));
 }

 public static ILogger GetStaticLogger<T>()
 {
 if (_loggerFactory is null)
 throw new InvalidOperationException("StaticLogger is not initialized yet.");

 return loggerByType
 .GetOrAdd(typeof(T), _loggerFactory.CreateLogger<T>());
 }
}
```

## Using it

Add this line to your GlobalUsings.cs

```Plain
global using static LoggerElimination.StaticLoggerFactory;
```

Use the StaticLoggerFactory like below:

```Plain
internal class Test
{
 private ILogger Logger { get => GetStaticLogger<Test>(); }

 public void LogTest() => Logger.LogInformation("It works!");
}
```
