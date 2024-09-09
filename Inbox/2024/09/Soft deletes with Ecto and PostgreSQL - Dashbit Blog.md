---
created: 2024-08-28T14:48:49 (UTC -03:00)
tags: []
source: https://dashbit.co/blog/soft-deletes-with-ecto?utm_medium=email&utm_source=elixir-radar
author: 
---

# Soft deletes with Ecto and PostgreSQL - Dashbit Blog

> ## Excerpt
> This article details how to implement soft-delete in Ecto using PostgreSQL rules and views

---
-   José Valim
-   August 13th, 2024
-   [ecto](https://dashbit.co/blog/tags/ecto), [postgres](https://dashbit.co/blog/tags/postgres), [soft deletes](https://dashbit.co/blog/tags/soft%20deletes)

One of our clients at Dashbit has recently asked us the different ways to implement soft-deletes in their Phoenix + Ecto application.

The idea of a soft-delete is that, when you choose to “delete” a given resource, let’s call it “orders”, instead of effectively deleting it from the database, you will mark the order as deleted, and then you simply do not show such orders to the user.

There are several ways to implement such a feature. If you need it only for one resource or another, sometimes the best option is to handle it at the application level. The downsides of doing it at the application level is that you are responsible to make sure they are never deleted, which can be error prone, especially once you consider foreign keys cascade and deletion rules. Another common problem is that, on every query you perform, you must remember to filter it only to non-“soft-deleted” resources.

Alternatively, if soft deletion plays an important role in your application and applies to several resources, then enforcing it at the database level can be a robust option, as you guarantee no one can accidentally remove this data unintentionally.

In this post, we will discuss one approach to implement soft-deletion in the database, with rules and views. This article is an adaption of [“Soft deletion with PostgreSQL: but with logic on the database!”](https://evilmartians.com/chronicles/soft-deletion-with-postgresql-but-with-logic-on-the-database), but focused on [Ecto](https://github.com/elixir-ecto/ecto).

## Custom deletions with PostgreSQL rules

In order to add soft-deletion to a resource, the first step is to add a `deleted_at` column to its table. In this case, we are choosing a timestamp column, but a boolean `deleted` column would suffice too:

```
<span>add</span><span> </span><span>:deleted_at</span><span>,</span><span> </span><span>:utc_datetime</span>
```

Still in the migration, let’s add [PostgreSQL Rule](https://www.postgresql.org/docs/current/rules.html), that converts deletes into updates:

```
<span>execute</span><span> </span><span>"""
        CREATE OR REPLACE RULE soft_deletion AS ON DELETE TO orders
        DO INSTEAD UPDATE orders SET deleted_at = NOW() WHERE id = OLD.id AND deleted_at IS NULL RETURNING OLD.*;
        """</span><span>,</span><span>
        </span><span>"""
        DROP RULE IF EXISTS soft_deletion ON orders;
        """</span>
```

When migrating, we create a `soft_deletion` rule on the orders table that effectively replaces the deletion by an update statement that sets the `deleted_at` column. Notice the statement also uses `RETURNING OLD.*`, this is necessary if you have any `:read_after_writes` field in your schema.

## Custom selects with PostgreSQL views

By using a rule, we guarantee that no one can accidentally delete this resource, even if they are connected to the database terminal (we will see some explicit ways to bypass it though).

However, this still leaves one problem: whenever you ask `Ecto` to read all orders, such as `Repo.all(Order)`, it will include deleted orders by default. Therefore you, the application developer, must remember to filter them.

Luckily, we can solve this problem by adding two lines to our migration:

```
<span>execute</span><span> </span><span>"CREATE OR REPLACE VIEW visible_orders AS SELECT * FROM orders WHERE deleted_at IS NULL"</span><span>,</span><span>
        </span><span>"DROP VIEW IF EXISTS visible_orders"</span>
```

The example above creates a new view, called “visible\_orders”, which only contains the visible orders. Now you only need to change your `Ecto` schema to use this new view:

```
<span>defmodule</span><span> </span><span>MyApp.Order</span><span> </span><span data-group-id="9702450411-1">do</span><span>
  </span><span>use</span><span> </span><span>Ecto.Schema</span><span>

  </span><span>schema</span><span> </span><span>"visible_orders"</span><span> </span><span data-group-id="9702450411-2">do</span><span>
    </span><span>...</span><span>
    </span><span>field</span><span> </span><span>:deleted_at</span><span>,</span><span> </span><span>:utc_datetime</span><span>
    </span><span>timestamps</span><span data-group-id="9702450411-3">(</span><span data-group-id="9702450411-3">)</span><span>
  </span><span data-group-id="9702450411-2">end</span><span>
</span><span data-group-id="9702450411-1">end</span>
```

This brings two benefits. First, `Repo.all(MyApp.Order)` now only returns visible orders. Furthermore, because Ecto treats tables (sources) and schemas as distinct entities, if you want to query all orders, including deleted ones, you can simply write: `Repo.all({"orders", MyApp.Order})`. Or in a query:

```
<span>from</span><span> </span><span>o</span><span> </span><span>in</span><span> </span><span data-group-id="3970854072-1">{</span><span>"orders"</span><span>,</span><span> </span><span>MyApp.Order</span><span data-group-id="3970854072-1">}</span><span>,</span><span>
  </span><span>join</span><span>:</span><span> </span><span>u</span><span> </span><span>in</span><span> </span><span>assoc</span><span data-group-id="3970854072-2">(</span><span>o</span><span>,</span><span> </span><span>:users</span><span data-group-id="3970854072-2">)</span><span>,</span><span>
  </span><span>where</span><span>:</span><span> </span><span>o</span><span>.</span><span>total</span><span> </span><span>&gt;=</span><span> </span><span>^</span><span>min_value</span><span>,</span><span>
  </span><span>...</span>
```

Any query you execute in this style still has all of the type casting and SQL injection protections you are familiar with, but now applied to different views/tables. Furthermore, because our view is “updatable” (it only has simple WHERE clauses), we can insert, update, and delete entries directly from the view, without a need to specify the source table for those operations.

## The caveats

There are two caveats in this approach. The first one is that, if you attempt to delete an order, `Ecto` won’t be happy with it:

```
<span>Repo</span><span>.</span><span>delete</span><span data-group-id="2176269285-1">(</span><span>order</span><span data-group-id="2176269285-1">)</span>
```

This raises `Ecto.StaleEntryError` by default. This happens because, since the deletion does not happen, PostgreSQL returns the information that zero rows were affected. In turn, Ecto parses this result set to mean that the record no longer exists (and therefore it is stale). This is clearly spelled out in the error message and, from Ecto v3.12, you can opt-in and say that stale entries are expected:

```
<span>Repo</span><span>.</span><span>delete</span><span data-group-id="8797644218-1">(</span><span>order</span><span>,</span><span> </span><span>allow_stale</span><span>:</span><span> </span><span>true</span><span data-group-id="8797644218-1">)</span>
```

Similarly, if you attempt to use `Repo.delete_all(from o in Order)`, Ecto will always report that zero rows were affected. In case you do need to know the number of rows affected, you can use `Repo.update_all` and manually set its `deleted_at` column instead.

The other caveats are related to indexes, constraints, and foreign keys. If you are setting an index for performance, you may want to consider if the `deleted_at` should be included or not. Similarly, if you are adding constraints, such as unique indexes or check constraints, you may not want them to apply to deleted at. Luckily, PostgreSQL (as well as Ecto) supports partial indexes, and you can apply these constraints only when `deleted_at IS NULL`.

Cascading foreign keys also require attention when deleting resources and they may require you to adapt the database rule accordingly. This is [covered in the original article](https://evilmartians.com/chronicles/soft-deletion-with-postgresql-but-with-logic-on-the-database#cascade-deletes), so check it out.

## Making deletions possible

Before we wrap this up, there is one last question to answer: what happens when you want/need to effectively delete the data?

Luckily, PostgreSQL has a mechanism to enable and disable rules, which you can also use from within a transaction:

```
<span>Repo</span><span>.</span><span>transaction</span><span data-group-id="5201967762-1">(</span><span data-group-id="5201967762-2">fn</span><span> </span><span>-&gt;</span><span>
  </span><span>Repo</span><span>.</span><span>query!</span><span data-group-id="5201967762-3">(</span><span>"ALTER TABLE orders DISABLE RULE soft_deletion"</span><span data-group-id="5201967762-3">)</span><span>
  </span><span>Repo</span><span>.</span><span>delete!</span><span data-group-id="5201967762-4">(</span><span>order</span><span data-group-id="5201967762-4">)</span><span>
  </span><span>Repo</span><span>.</span><span>query!</span><span data-group-id="5201967762-5">(</span><span>"ALTER TABLE orders ENABLE RULE soft_deletion"</span><span data-group-id="5201967762-5">)</span><span>
</span><span data-group-id="5201967762-2">end</span><span data-group-id="5201967762-1">)</span>
```

In fact, you could even encapsulate this by adding a function to your `Ecto.Repo` module:

```
<span>defmodule</span><span> </span><span>MyApp.Repo</span><span> </span><span data-group-id="9327166314-1">do</span><span>
  </span><span>use</span><span> </span><span>Ecto.Repo</span><span>,</span><span> </span><span>otp_app</span><span>:</span><span> </span><span>:myapp</span><span>

  </span><span>def</span><span> </span><span>disable_soft_deletion</span><span data-group-id="9327166314-2">(</span><span>tables</span><span>,</span><span> </span><span>fun</span><span data-group-id="9327166314-2">)</span><span> </span><span data-group-id="9327166314-3">do</span><span>
    </span><span>transaction</span><span data-group-id="9327166314-4">(</span><span data-group-id="9327166314-5">fn</span><span> </span><span>-&gt;</span><span>
      </span><span>for</span><span> </span><span>table</span><span> </span><span>&lt;-</span><span> </span><span>tables</span><span> </span><span data-group-id="9327166314-6">do</span><span>
        </span><span>query!</span><span data-group-id="9327166314-7">(</span><span>"ALTER TABLE </span><span data-group-id="9327166314-8">#{</span><span>quote_table</span><span data-group-id="9327166314-9">(</span><span>table</span><span data-group-id="9327166314-9">)</span><span data-group-id="9327166314-8">}</span><span> DISABLE RULE soft_deletion"</span><span data-group-id="9327166314-7">)</span><span>
      </span><span data-group-id="9327166314-6">end</span><span>

      </span><span>try</span><span> </span><span data-group-id="9327166314-10">do</span><span>
        </span><span>fun</span><span>.</span><span data-group-id="9327166314-11">(</span><span data-group-id="9327166314-11">)</span><span>
      </span><span data-group-id="9327166314-10">after</span><span>
        </span><span>for</span><span> </span><span>table</span><span> </span><span>&lt;-</span><span> </span><span>tables</span><span> </span><span data-group-id="9327166314-12">do</span><span>
          </span><span>query!</span><span data-group-id="9327166314-13">(</span><span>"ALTER TABLE </span><span data-group-id="9327166314-14">#{</span><span>quote_table</span><span data-group-id="9327166314-15">(</span><span>table</span><span data-group-id="9327166314-15">)</span><span data-group-id="9327166314-14">}</span><span> ENABLE RULE soft_deletion"</span><span data-group-id="9327166314-13">)</span><span>
        </span><span data-group-id="9327166314-12">end</span><span>
      </span><span data-group-id="9327166314-10">end</span><span>
    </span><span data-group-id="9327166314-5">end</span><span data-group-id="9327166314-4">)</span><span>
  </span><span data-group-id="9327166314-3">end</span><span>

  </span><span>defp</span><span> </span><span>quote_table</span><span data-group-id="9327166314-16">(</span><span>name</span><span data-group-id="9327166314-16">)</span><span> </span><span>when</span><span> </span><span>is_binary</span><span data-group-id="9327166314-17">(</span><span>name</span><span data-group-id="9327166314-17">)</span><span> </span><span data-group-id="9327166314-18">do</span><span>
    </span><span>if</span><span> </span><span>String</span><span>.</span><span>contains?</span><span data-group-id="9327166314-19">(</span><span>name</span><span>,</span><span> </span><span>"</span><span>\"</span><span>"</span><span data-group-id="9327166314-19">)</span><span> </span><span data-group-id="9327166314-20">do</span><span>
      </span><span>raise</span><span> </span><span>"invalid table name"</span><span>
    </span><span data-group-id="9327166314-20">end</span><span>

    </span><span data-group-id="9327166314-21">[</span><span>?"</span><span>,</span><span> </span><span>name</span><span>,</span><span> </span><span>?"</span><span data-group-id="9327166314-21">]</span><span>
  </span><span data-group-id="9327166314-18">end</span><span>
</span><span data-group-id="9327166314-1">end</span>
```

And now:

```
<span>Repo</span><span>.</span><span>disable_soft_deletion</span><span data-group-id="1921855573-1">(</span><span data-group-id="1921855573-2">[</span><span>"orders"</span><span data-group-id="1921855573-2">]</span><span>,</span><span> </span><span data-group-id="1921855573-3">fn</span><span> </span><span>-&gt;</span><span>
  </span><span>Repo</span><span>.</span><span>delete!</span><span data-group-id="1921855573-4">(</span><span>order</span><span data-group-id="1921855573-4">)</span><span>
</span><span data-group-id="1921855573-3">end</span><span data-group-id="1921855573-1">)</span>
```

## Wrapping up

This article showed how to achieve soft deletions in Ecto and PostgreSQL by using rules and views. Not only that, we learned how Ecto allows us to apply the same schema to different tables/views, and how it deals with stale records on `delete` (which also applies to `update`). Those ideas can be useful outside of soft deletion.

The soft-deletion technique discussed here is useful when you want to keep deleted in the same table (and therefore with the same constraints and foreign keys) as regular data. But your application may have different needs. For example, if you want to implement some sort of trash bin, that stores all types of deleted resources in your application or you simply want to keep deleted data for auditing purposes, a simpler solution is to [use triggers to copy a JSON representation of the deleted data to another table](https://brandur.org/fragments/deleted-record-insert). We hope this article provides some options for you to explore whenever this question arises.

Happy coding!
