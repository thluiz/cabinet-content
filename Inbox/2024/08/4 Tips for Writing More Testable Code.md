---
created: 2024-08-21T15:12:35 (UTC -03:00)
tags: []
source: https://spin.atomicobject.com/more-testable-code/?ref=dailydev
author: Anjali Munasinghe
---

# 4 Tips for Writing More Testable Code

> ## Excerpt
> I recently paired with a developer new to automated testing and shared tips I've picked up over the years on how to write more testable code.

---
I recently paired with a developer newer to [automated testing](https://spin.atomicobject.com/ux-design/h-exploratory-testing/) on setting up a project’s first unit tests. I enjoyed sharing some of the tips I’ve picked up over the years on how to write more testable code. And, it was fun to see how introducing tests caused our production code to change to make testing easier and increase our confidence in the tests.

Here, I’ll share a few things we did to make our code more testable and principles I try to follow to write more testable code.

## Choose specific input types.

When setting up the parameters for a function you’ll be testing, the types you choose can make a big difference.

For example, in the project I described above, we were writing tests for some custom filtering implemented on a data grid. Rather than using the class that contained all data about the column, we chose to create a simple class consisting of just the field name and search value to use for our custom filtering function.

Including only the exact data needed by the function under test as parameters makes it much more straightforward to set up the data in our tests. And, this also helps avoid coupling to the component libraries or frameworks you’re using.

## Avoid mutating objects.

In the same filtering example I described above, we ran into another issue. All of our tests were passing, but we saw unexpected behavior when manually testing. While stepping through the code to investigate what was happening, we saw that the items we were filtering to were being modified in place by the data grid component as it replaced null values with placeholders while rendering each row.

If other parts of the system are allowed to modify objects, then the tests written involving those objects can’t really be trusted. They could, in fact, create a false sense of confidence in the code written.

## Keep complicated logic out of the database.

Next, I’ll touch on one of the big factors that prevented us from adding additional appropriate test coverage for the changes we made on this project. On this project, the majority of the business logic was written as [SQL functions](https://learn.microsoft.com/en-us/sql/t-sql/functions/functions?view=sql-server-ver16). The C# code we were writing was really only responsible for displaying the queried results.

It’s possible to test your SQL functions, views, and triggers, but there is a lot of additional overhead to doing so. It requires a predictable schema and test databases that can be thrown away and recreated easily. [Seeding test data](https://spin.atomicobject.com/cypress-web-project-test-data-set/) may also be more tricky and tedious. It requires us to set up data for things that we don’t actually care about for the sake of the test.

## Create new modules and services as needed.

And finally, don’t be hesitant to add additional services and new layers to your code as needed. In the past, I have noticed a hesitancy to split more complicated functionality out into a focused class or module. I’ve especially seen doing so discouraged in a language like C#, where you must be a lot more explicit about dependency management. I’ve also seen a reluctance to create a new category of class/module that didn’t fit neatly into the established code org patterns.

Instead, when the logic gets more complex, the tendency may be to reach for private helper functions. While this may boost readability, it doesn’t necessarily boost testability. That’s because you still, ultimately, need to remember the private helpers’ behavior as you set up your test scenarios.

Many things I’ve touched on are standard development and code org practices that have become second nature. It was great to revisit these in the context of trying to introduce testing strategies to an existing project.
