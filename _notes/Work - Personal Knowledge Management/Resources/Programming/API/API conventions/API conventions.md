---
title: API conventions
feed: show
---

[https://javascript.plainenglish.io/8-best-practices-for-rest-api-design-3fd1b837b283]( https://javascript.plainenglish.io/8-best-practices-for-rest-api-design-3fd1b837b283)

![[assets/1Z_ZpWzWYEEM58ng27w9PUA.png]]

## 1. Automate Caching

Repeated request and responding to those request consumes resources and this becomes a sign of flawed design. To solve this problem you must store data fetched from the **API** on the server and serve later from there.

However, there are times when the data becomes outdated and needs to be updated, in this case, we have some industry caching solutions **(Redis** and **Amazon ElasticCache)** that can cache data even after making changes.

## 2. API Documentation

Always provide clear and concise documentation is necessary for any API, it becomes harder to use that API later without good documentation. So make sure that your API has good documentation using simple languages with updates and new releases.

_**Your API Documentation should include the following:**_

- Simple and easy to read language.
- Implementation of API in different programming languages.

## 3. Don't Return Plain Text

It is not necessary to go above **JSON** in **REST** architecture, as most _REST APIs_ use JSON as a data format. But there are times when returning a body that contains a JSON-formated string isn't enough.

Such as you can specify the **Content-Type** header set to the value **application/json.**

## 4. Use Plural Resource Nouns

There is nothing wrong with _singular resource nouns_, but if you want to just keep things simple, then it is recommended to use plural resource nouns.

_**For example**__,_ **GET/post/3/** looks fine but what about **GET/post/?.** In this case, we are not confirmed if we are getting only **one post** or all of them.

To avoid this approach and be more consistent you can use plural resource nouns.

## 5. Handle Trailing Slashes

Using trailing slashes is a matter of choice, but make sure that you are sticking to that one choice with trailing slashes. As there are times when inconsistency will be there due to some minor mistakes.

_**For example**_, in a REST API project, you can receive a **500 Internal Error** just because of a missing trailing slash.

## 6. 401 VS 403

Both the status codes have different meanings and it is very easy to get confused whether it related to _authentication_ or _authorization_. In order to resolve issues faster, you need to know the difference.

If the user hasn't provided authentication details or they were invalid, then it is **401 Unauthorized**.

And if the user successfully authenticated but didn't provide the permissions to access the resource, then it is **403 Forbidden**.

## 7. Filtering and Pagination

Every database behind a **REST API** becomes larger with time and there are times when we have to control the flow of the data at once, otherwise, that can bring our systems down easily. This is why always allow filtering in your APIs.

We also need to paginate data to return a few results at a time, otherwise, all of our resources will blow because of trying to get all the requested data at once.

## 8. Use CamelCase for Attribute Names

Avoid using underscores or capital letters.

Often times **RESTful** web services are consumed by a client written in JavaScript and that client will convert the JSON response to a JavaScript object and call its attributes. So it is recommended to stick with the JavaScript convention which makes the code more readable.
