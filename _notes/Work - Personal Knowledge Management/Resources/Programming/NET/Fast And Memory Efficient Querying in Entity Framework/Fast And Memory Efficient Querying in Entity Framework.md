---
title: Fast And Memory Efficient Querying in Entity Framework
feed: show
---

[https://medium.com/codex/fast-and-memory-efficient-querying-in-entity-framework-ebf906d9e6cb]( https://medium.com/codex/fast-and-memory-efficient-querying-in-entity-framework-ebf906d9e6cb)

![[assets/1dsMhvjq7FAhUwLW35icrkw.png]]

There are simple and useful ways to speed up our Entity Framework queries. We just need to know when to use what.

I was giving a presentation to a corporate client, laying out plan for a new software development. Their IT guy asked about the tech stack that we will be using for DB operations. My answer was SQL Server + Entity Framework. And he freaked out! He was concerned about the performance issues as the database was going to be huge. Since he was not a software guy, his perception was based on what he had heard from others. He wasn’t 100% wrong, but some beliefs needed to be challenged.

I assured him that Entity Framework performance isn’t as bad as he may have heard about, and a lot depends on the team/person writing the code. I explained to him some of the tips that I am going to cover in this article. After an in-depth discussion, I was able to convince him that EF (Entity Framework) has a lot more to offer than what most people know and use. The application was deployed after a few months and is running smoothly since then, with tons of data pouring in and out every day.

Here are some easy to use tips to speed up Entity Framework performance:

## Use AsNoTracking()

Every time we summon an entity or list using EF, the objects are tracked by EF by storing them in memory. Whenever we call db.SaveChanges(), EF determines those changes to generate queries to be fired. That’s the normal behavior, nothing special here.

However, in a large number of cases, we query data for read-only purpose. In such cases, we don’t want Entity Framework to track changes. We do this by adding **AsNoTracking()** after the table name. This reduces memory consumption and speeds up execution. Here’s how you do it:

Using AsNoTracking() in Entity Framework Queries

## Select Single Column

I have seen a lot of code where people query entire object, when they just need one column value. For e.g., we may need to query a student’s name based on StudentId. Let’s see how this can be done:

Select single column from Entity

This is extremely useful, especially if the underlying object has a lot of columns. Using .Select() makes EF to construct query with only a single column in SELECT statement.

## Select Multiple Columns

There may be cases where you need more than one column but still not the entire object. In those cases also, it makes sense to query only required columns instead of querying the entire object.

Select multiple columns resulting in complex type entity

## Query on Demand without code repetition

Some cases require us to query same table multiple times for different purposes. For e.g., you may need to fetch list of employees with Grade “Manager” but you also need total number of employees.

Usually what one would do is to fire a query with Grade == “Manager” and another query to count total number of employees without any filter.

What you can alternatively do is prepare your query statement without using the .ToList() or .Count() or .FirstOrDefault() at end of the query. When you do this, a query is not fired against the database. You can use the object so generated multiple times with filters, ordering etc.

While this may not save you another query round-trip but simplifies code and promotes reuse.

Query re-use without adding .ToList()

## Cache Objects Beforehand

This is a slightly complex case but worth noting as the benefits are huge.

Suppose our user uploaded an excel file to import list of employees. One of the column in excel is Location and we need to verify if the value entered by user is valid by checking it in the Locations table.

Locations Table

```Plain
+------------------------------------+
| Name             | State           |
+------------------------------------|
| Head Office      | Delaware        |
| Branch Office    | Wisconsin       |
+------------------------------------+
```

We’ve read the excel file and have the data in a DataTable. We iterate over DataTable rows and validate the data in each row before we add the employee record.

Instead of checking location validity by querying inside the loop, we can fetch list of locations outside of the loop and check against that. This fires the location query only once instead of each loop iteration firing it’s own query.

Cache objects before-hand for repetitive use

If the loop is long, this can save a lot of database round-trips and yield great results.

That’s all I have for now. I hope some of these tips will help you in writing a better and more efficient code in your next project. If you know more such tips, do share in comments, so we can learn from each other.

To read all my stories, along-with thousands of other writers on medium, consider opting for medium membership. If you [join using my link](https://prashantio.medium.com/membership), I will get a commission from your joining fee. Good Luck!
