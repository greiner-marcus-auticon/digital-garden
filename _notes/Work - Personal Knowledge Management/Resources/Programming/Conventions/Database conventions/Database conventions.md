---
title: Database conventions
feed: show
---

[https://levelup.gitconnected.com/how-to-design-a-clean-database-2c7158114e2f]( https://levelup.gitconnected.com/how-to-design-a-clean-database-2c7158114e2f)

![[assets/1jRRB_bNmfI5DZm5ZxHwnNw.jpeg]]

Photo by [Lukas](https://www.pexels.com/@goumbik?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/person-writing-on-notebook-669615/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

No matter what kind of developer you are, every once in a while, we come across an API which returns data in such a way that we don’t have to spend much time understanding it.

But generating this type of clean and consistent result takes time, effort, and experience. Today we will take the first step towards designing a clean database.

We are keeping it short and to the point. Let’s start

## Some Terminology

**Table:** this is a collection of data

**Primary Key:** This is the unique identifier of a table

**Attribute:** means property of your data. For example, `name` is an attribute of a `user` .

**Data Type:** Data types represent the various types of your data. For example -string, int, timestamp, etc.

## 1. Words should be underscore separated

When your attribute name has more than 1 word, then separate it with `snake_case`. Don’t use `camelCase` or any other case for consistency.

**Reason**

- Improves readability
- Names can become more platform-independent

## 2. Data Types Should not Be Names

Never have data-types as your column name. This happens mostly for timestamp parameters. Give a meaningful name to it.

**Reason**

- Using data types can create confusion on the other end of the application.
- Giving a proper name gives more context to the usage of the parameter.

## 3. Attribute names should be lowercase

Don’t use upper-case names for your attributes.

**Reason**

- This practice avoids confusion from upper-case SQL keywords
- It can improve typing speed

## 4. Write Full Words

Don’t try to shorten the names of columns for the sake of space or any other logic. Try to be as explicit as possible.

**Reason**

This rule promotes self-documenting design

## 5. But use common abbreviations

An exception of rule-4 is when you have a widespread abbreviation. In those situations, go for the short one.

**Good**

```Plain
i18n
```

But if you find yourself in confusion, go for the full name. It’s an investment you are making for the future.

## 6. Avoid having numbers in a column name

Believe it or not, I have seen it enough. Never have numbers in your column name.

**Bad**

```Plain
address1 , address2
```

**Reason**

This is a sign of very poor normalization on your end. So try to avoid it as much as possible.

## 7. Use short table names

Be very careful when naming tables because long table names can have a huge bad impact in the future.

**Reason**

Short table names will help you when you create relational columns and linking tables.

## 8. Lookout for reserved words

Each database has some reserved words. Learn them and avoid them.

**Bad**

```Plain
user lock table etc
```

## List of reserved words for some popular database

## 9. Singular names for tables

Always try to use singular names for tables. This is a controversial one, and different people have different opinions. But stick to one.

**Bad**

```Plain
users and orders
```

**Good**

```Plain
user and order
```

**Reason**

- This promotes consistency with primary keys and lookup tables
- Pluralization can be tricky sometimes. So having singular table names can make it easier to program.

## 10. Linking tables should have alphabetical order

When creating a junction table, concatenate the names of the two tables in alphabetical order.

## 11. Singular Column Names

Usually, it’s the best practice unless you are breaking data normalization rules.

## 12. Primary key name

If it’s a single column, then it should be named as `**id**`

```Plain
CREATE TABLE order (
  id            bigint PRIMARY KEY,
  order_date    date NOT NULL
);
```

## 13. Foreign key name

It should be the name of the other table and the referred field. For example, if you are referencing a `person` inside your `team_member` the table then you can do it like this.

## 14. Never suffix column names with types

There is no point in suffixing your column names with types of data. Avoid doing this.

## 15. Indexes should have both table and column name

If you are creating an index, then have the table name followed by the column names that you are referencing

```Plain
CREATE TABLE person (
  id          bigserial PRIMARY KEY,
  first_name  text NOT NULL,
  last_name   text NOT NULL,
);
```

## 16. Date type column names

Suffix your date-type column names with `_on` or `_date` .

For example, if you have a column for storing the updated date, then do this,

**Good**

```Plain
updated_on or updated_date
```

## 17. Date-Time type column names

If your column name has time with it, then suffix them with `_at` or `_time` .

For example, if you want to store the order time, then

**Good**

```Plain
ordered_at or order_time
```

## 18. Boolean type column Names

If you have boolean type column names, then prefix them with `is_` or `has_` .

**Good**

```Plain
is_admin or has_membership
```

## Final Words

If you are already working on a project, stick to the convention that the project is already following. Because

> Only thing worse than a bad convention is multiple conventions

But if you are learning or designing a database from scratch, having these rules in mind will take you a long way.

What are your thoughts? Is there any rule you disagree with? I am more than happy to have some productive conversation in the comment section!

Have a great day! :D

**Have something to say? Get in touch with me via** [**LinkedIn**](https://www.linkedin.com/in/56faisal/)

## References
