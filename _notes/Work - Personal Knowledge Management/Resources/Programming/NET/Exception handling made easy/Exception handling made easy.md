---
title: Exception handling made easy
feed: show
---

[https://salihcantekin.medium.com/net5-exception-handling-made-easy-a2236fbc1186]( https://salihcantekin.medium.com/net5-exception-handling-made-easy-a2236fbc1186)

![[assets/1fIPW9TvUh3LvGXq-ljgQkg.png]]

One of the things I always admire is the open-source publications that people do by spending their valuable time contributing to the community. Despite this admiring, I never had a chance to do the same. I have a youtube channel in order to explain to people what I know and what I experienced in my professional life. However, I can’t count this an open-source publication :))

Today, I’ve made an open-source library to help developers whenever they want to handle and manage unhandled exceptions in their WebApi projects. As exception handling is an important feature for all Web API projects, this library is built to make this process easy. Let’s talk about this library.

First of all, this library can be found in [NuGet](https://www.nuget.org/packages/TechBuddy.Middlewares.ExceptionHandling/) as well as its [GitHub Repo](https://github.com/salihcantekin/TechBuddySolution) is also public and accessible. Although this project has documentation on its NuGet page or GitHub Page, I will share some pieces of code of using it.

The name of this library is _“TechBuddy.Middlewares.ExceptionHandling”_

As long as you installed its NuGet package into your project, this single line of code in Configure Method in your startup.cs file will handle and manage your exception in a simple way.

As soon as you added this code to your project, your response model will appear by the time an unhandled exception occurred in your project.

New Response Model

As this response looks simple and enough, you don’t have to stick with it. You are able to customize your response by using _“MiddlewareOptions”_ like below.

Customized Response Configuration

To explain the code lines above, the first thing we care about is the _IsDevelopment_ option. When it is true, the _ExceptionMessage_ will be using stack trace info of exception to make it detailed. When _ContentType_, _DefaultMessage_ and _DefaultHttpStatusCode_ is set, it overrides its original value on response. On the other hand, by adding specific exception types into _ExceptionTypeList,_ we can customize our exception handling mechanism to work only these exceptions occurred. So basically, this middleware handles only the exception is one of the ones you specify.

If you are not happy to see the default response model, now you can override this model too. It is amazing, right? To customize this model, you can use _UseResponseModelCreator_ method by passing any class that inherits _IResponseModelCreator_ interface as a generic parameter. See an example below.

Custom Response Model Creator

When you use _CustomResponseModelCreator_ class, the response will be like below. Please note, when you customize this feature and _IsDevelopment_ is true, the _ExMes_ property shows a detailed message. In this case, it is set to false.

If these features are still not good enough to make you happy, I have one more bullet in my pocket. _ExceptionHandlerAction_ will make you happy I guess. When you use this feature, the exception handling mechanism will handle and pass it through your action. This is super cool. 👌It also gives you access to the Exception and HttpContext in order to customize your response how you want it to look. While doing this, It also considers about _ExceptionTypeList_.

So at the end of the day, you would have this response on the client-side. Don’t you like it this way? Then, use this library and make it the way you like :)

Final Response after Action is override

> Github Repo: <https://github.com/salihcantekin/TechBuddySolution>
>
> NuGet: [https://www.nuget.org/packages/TechBuddy.Middlewares.ExceptionHandling/](https://www.nuget.org/packages/TechBuddy.Middlewares.ExceptionHandling/)

Enjoy handling your exceptions :)
