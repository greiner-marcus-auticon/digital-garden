---
title: Better APIs
feed: show
---

[https://levelup.gitconnected.com/good-api-design-bad-api-design-2405dcdde24c]( https://levelup.gitconnected.com/good-api-design-bad-api-design-2405dcdde24c)

Principals and rules of modern REST API design

Usually, updating or developing an API of some service, there are a long discussions on the structure of he API, naming and functions. Although, through the years there are created certain rules, that can be applied to the process and help getting to the common ground while developing.

First — some basics.

API - is the acronym **for Application Programming Interface**, which is a software that allows two applications to talk to each other. In this article, we’ll focus on web API.

![[assets/1oWaI10Ug4wRrIM4O5yzCRw.png]]

Web application structure

Basically, each time you use a web app, send a message, or go to some URL, you’re using an API of some application wherever it is a client application or server one.

As all the communication between API and everything else is done by HTTP, is important to remember that HTTP has a set of different methods that has to be used for the specific purposes. Most important of them are:

- `GET` — requests a representation of the specified resource. Requests using GET should only retrieve data.
- `POST` — submits data to the specified resource.
- `PUT` — replaces all current representations of the target resource with the request data.
- `DELETE` — deletes the specified resource.
- `PATCH` — applies partial modifications to a resource.

About the rest of them can be read [**here**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) in more details.

Through years, there were several different types of the API architectures and protocols. They are different on how they specify data types and commands as well as they are different in their capabilities. For some time most popular was XML based protocol, but it is mostly replaced now by JSON.

Most common API architecture in todays world is REST (_representational state transfer_). When using REST, it is obligatory to follow JSON rules and form your requests in valid JSON. Except of that, good API should follow the following rules:

- API have to be separated from the backend, data storage, client etc. It has to be separate layer due to security and flexibility.
- State less — different request should know nothing about each other and be processed independently. That also means that each request needs to include all the information necessary for processing it.
- API should work same way independently of a client sending request (e.g. is it a web-server or load-balancer or any other client).
- REST APIs usually send static resources, but in rear cases, responses can also contain executable code (such as Java applets). In these cases, the code should only run on-demand.
- Cacheability — when possible, resources should be cacheable on the client or server side. The goal is to improve performance on the client side, while increasing scalability on the server side. Although, there are special headers like [**Cache-Control**](https://restfulapi.net/caching/) to control the cache behaviour.
- Handle errors and return corresponding error codes. All errors have their specific codes, and instead throwing internal error to user, handle it and send corresponding code and message (e.g. 404 — not found. List of codes can be found [**here**](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status))
- Don’t forget that API should be idempotent (that means that can be invoked many times with the same result). Users sometimes can duplicate requests coming to the API. These duplicate requests may be unintentional (or intentional due to timeout or network issues). So API have to be fault-tolerant in such a way that the duplicate requests should have the same results (only POST request is not idempotent).
- Use swagger or other tool to write documentation on your API. Documentation is super important part (if someone going to use that API some day)

There are also some good manners in naming endpoints as well:

- Use only nouns: endpoint should be named with nouns that specify the contents of the resource, rather than adding a verb for the function that is being performed (e.g. name endpoint _/users_ and use different HTTP methods for working with users entity instead of creating several endpoints like _/get-user_, _/add-user_, etc.).
- Use clear names: name of endpoint should be clear and intuitive (don’t use any shortcuts or abbreviations, unless it is obvious — _/ids_ is understandable and preferable to _/identification-numbers_).
- Build hierarchy by forward slashes: group endpoints into logical groups (_/departments/ids_ and _/departments/managers_ is better then _/departments-ids_ and _/departments-managers)._
- Use only lowercase: URIs are case-sensitive (according to [**specification**](https://datatracker.ietf.org/doc/html/rfc3986)) so it is better to try to avoid upper case, unless it is necessary.
- Use “-” to separate words: different words in the endpoint name usually divided by “-” rather then underscores or camel case.
- Avoid special characters: URLs can only be sent and received using the ASCII character set, so it is possible to use only characters from that set (also there are some expectable but unsafe characters like “%”,”[]”,”{}”,”|”,” “,”<>” and it is better try to avoid using them as well).

Most of the REST API is made together with micro service architecture. In that case such API structure will provide flexibility to change underlaying logic, add or remove components etc. without changing other communication protocol with other services.

With all rules in place, let’s take a look at the example of the API:

If you want to dive deeper into API design here are some useful resources:

Thank you, and take care!
