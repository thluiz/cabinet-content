---
created: 2023-12-29T11:00:48 (UTC -03:00)
tags: []
source: https://www.infoworld.com/article/3711640/sql-unleashed-9-ways-to-speed-up-your-sql-queries.html?ref=dailydev#tk.rss_all
author: Serdar Yegulalp
---

# SQL unleashed: 9 ways to speed up your SQL queries | InfoWorld

> ## Excerpt
> Want faster SQL queries? Here are nine best practices for writing SQL queries that work like a charm (or at least, like they should).

---
### Want faster SQL queries? Here are nine best practices for writing SQL queries that work like a charm (or at least, like they should).

Senior Writer, InfoWorld | Dec 20, 2023 2:00 am PST

![shutterstock 111592973 runners passing baton relay race](https://images.idgesg.net/images/article/2023/01/shutterstock_111592973-100936712-large.jpg?auto=webp&quality=85,70)

sirtravelalot

SQL is the [leading language for developing and querying databases](https://www.infoworld.com/article/3219795/what-is-sql-the-lingua-franca-of-data-analysis.html), but it has a few quirks. In my last article, I shared [7 SQL mistakes to avoid](https://www.infoworld.com/article/3209665/sql-unleashed-7-sql-mistakes-to-avoid.html). Now, let's take a look at 9 best practices for writing faster SQL queries. 

## 9 best practices for faster SQL queries

1.  Retrieve only the columns you need
2.  Use CASE instead of UPDATE for conditional column updates
3.  Keep large-table queries to a minimum
4.  Pre-stage your data
5.  Perform deletes and updates in batches
6.  Use temp tables to improve cursor performance
7.  Use table-valued functions over scalar functions
8.  Use partitioning to avoid large data moves
9.  Use stored procedures for performance, use ORMs for convenience

### Retrieve only the columns you need

A common SQL habit is to use `SELECT *` on a query, because it's tedious to list all the columns you need. Plus, sometimes those columns may change over time, so why not just do things the easy way?

But what happens if you query all the columns on a table that has a hundred or more columns? Such behemoths show up with depressing regularity in the wild, and it isn't always possible to rework them for a more sane schema. Sometimes the only way to tame this beast is to select a subset of columns, which keeps other queries from being resource-starved.

It's okay to use `SELECT *` when prototyping a query, but anything that heads into production should request only the columns actually used.

### Use CASE instead of UPDATE for conditional column updates

Something else developers do a lot is using `UPDATE ... WHERE` to set the value of one column based on the value of another column, e.g., `UPDATE Users SET Users.Status="Legacy" WHERE Users.ID<1000`. This approach is simple and intuitive, but sometimes it adds an unnecessary step.

For instance, if you insert data into a table and then use `UPDATE` to change it, like I've just shown, that's two separate transactions. When you have millions of rows, additional transactions can create a lot of unnecesary ops.

The better solution for such a massive operation is to use an inline `CASE` statement in the query to set the column value, _during the insert operation itself_. This way, you handle both the initial insert and the modified data in a single pass.

### Keep large-table queries to a minimum

Queries on tables of any size aren't free. Queries on tables with hundreds of millions or billions of rows are _absolutely_ not free.

Whenever possible, consolidate queries on big tables to the fewest possible discrete operations. For instance, if you have a table where you want to query first by one column and then by another, first merge that into a _single_ query, then ensure the columns you're querying against have a covering index.

If you find yourself taking the same subset of data from a big table and running smaller queries against it, you can speed things up for yourself and others by persisting the subset elsewhere, and querying against that. That leads us to the next tip.

### Pre-stage your data

Let's say you or others in your organization routinely run reports or stored procedures that require aggregating a lot of data by joining several large tables. Rather than re-run the join each time, you can save yourself (and everyone else) a lot of work by "pre-staging" it into a table specifically for that purpose. The reports or procedures can then run against that table, so the work they all have in common only has to be done once. If you have the resources for it, and your database supports it, you can use an in-memory table to speed this up even more.

### Perform deletes and updates in batches

Imagine a table that has billions of rows, from which millions need to be purged. The naive approach is to simply run a `DELETE` in a transaction. But then the entire table will be locked until the transaction completes.

The more sophisticated approach is to perform the delete (or update) operation in batches that can be interleaved with other things. Each transaction becomes smaller and easier to manage, and other work can take place around and during the operation.

On the application side, this is a good use case for a task queue, which can track the progress of operations across sessions and allow them to be performed as low-priority background operations.

### Use temp tables to improve cursor performance

For the most part, cursors should be avoided—they're slow, they block other operations, and whatever they accomplish can almost always be done some other way. Still, if you're stuck using a cursor for whatever reason, a temp table can reduce the performance issues that come with it.

For instance, if you need to loop through a table and change a column based on some computation, you can take the candidate data you want to update, put it in a temp table, loop through _that_ with the cursor, and then apply all the updates in a single operation. You can also break up the cursor processing into batches this way.

### Use table-valued functions over scalar functions

Scalar functions let you encapsulate a calculation into a stored procedure-like snippet of SQL. It's common practice to return the results of a scalar function as a column in a `SELECT` query.

If you find yourself doing this a lot in Microsoft SQL Server, you can get better performance by using a table-valued function instead and using `CROSS APPLY` in the query. For more on the little-discussed `APPLY` operator, see this [training module from the Microsoft Virtual Academy](https://learn.microsoft.com/en-us/training/modules/combine-query-results-set-operators/).

### Use partitioning to avoid large data moves

SQL Server Enterprise offers "partitioning," which allows you to split database tables into multiple partitions. If you have a table you're constantly archiving into another table, you can avoid using `INSERT/DELETE` to move the data, and use `SWITCH` instead.

For instance, if you have a table that is emptied out daily into an archive table, you can perform this emptying-and-copying operation by using `SWITCH` to simply assign the pages in the daily table to the archive table. The switching process takes orders of magnitude less time than a manual copy-and-delete. Cathrine Wilhelmsen has an [excellent tutorial on how to use partitioning in this way](https://www.cathrinewilhelmsen.net/table-partitioning-in-sql-server-partition-switching/).

### Use stored procedures for performance, use ORMs for convenience

ORMS—object-relational mappers—are software toolkits that produce programmatically generated SQL code. They allow you to use your application's programming language and its metaphors to develop and maintain your queries.

Many database developers dislike ORMs on principle. They are are notorious for producing inefficient and sometimes unoptimizable code, and they give developers less incentive to learn SQL and understand what their queries are doing. When a developer needs to hand-write a query to get the best possible performance, they don't know how.

On the other hand, ORMs make writing and maintaining database code far easier. The database part of the application isn't off in another domain somewhere, and it's written in a way that's more loosely coupled to the application logic.

It makes the most sense to use stored procedures for queries that are called constantly, require good performance, aren't likely to be changed often (if ever), and need to be surveyed for performance by the database's profiling tools. Most databases make it easier to obtain such statistics in aggregate for a stored procedure than for an ad hoc query. It's also easier for stored procedures to be optimized by the database's query planner.

The downside of moving more of your database logic into stored procedures is that your logic is coupled that much more tightly to the database. Stored procedures can mutate from a performance advantage into a massive technical debt. If you decide to migrate to another database technology later, it's easier to change the ORM's target than to rewrite all the stored procedures. Also, the code generated by an ORM can be inspected for optimization, and query caching often allows the most commonly generated queries to be reused.

If it's app-side maintainability that matters, use an ORM. If it's database-side performance, use stored procedures.

Serdar Yegulalp is a senior writer at InfoWorld, focused on machine learning, containerization, devops, the Python ecosystem, and periodic reviews.

Copyright © 2023 IDG Communications, Inc.
