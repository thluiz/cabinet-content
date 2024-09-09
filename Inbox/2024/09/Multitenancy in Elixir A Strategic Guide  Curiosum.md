---
created: 2024-09-04T10:20:35 (UTC -03:00)
tags: []
source: https://curiosum.com/blog/multitenancy-in-elixir?utm_medium=email&utm_source=elixir-radar
author: Mateusz Osiński
---

# Multitenancy in Elixir: A Strategic Guide | Curiosum

> ## Excerpt
> Explore the benefits and implementations of multitenancy in Elixir to transform your software architecture.

---
Have you ever wondered how to efficiently manage multiple clients within a single application? Or how to keep different users' data isolated while still leveraging shared resources? Enter multi-tenancy!

Table of contents

1.  [But, what is multitenancy basically?](https://curiosum.com/blog/multitenancy-in-elixir?utm_medium=email&utm_source=elixir-radar#but-what-is-multitenancy-basically)
2.  [What are the types of multitenancy?](https://curiosum.com/blog/multitenancy-in-elixir?utm_medium=email&utm_source=elixir-radar#what-are-the-types-of-multitenancy)
3.  [Who should use multitenancy?](https://curiosum.com/blog/multitenancy-in-elixir?utm_medium=email&utm_source=elixir-radar#who-should-use-multitenancy)
4.  [Enough of theory for now](https://curiosum.com/blog/multitenancy-in-elixir?utm_medium=email&utm_source=elixir-radar#enough-of-theory-for-now)
5.  [Multi-tenancy with schema prefixes in Ecto](https://curiosum.com/blog/multitenancy-in-elixir?utm_medium=email&utm_source=elixir-radar#multi-tenancy-with-schema-prefixes-in-ecto)
6.  [Multi-tenancy with foreign keys](https://curiosum.com/blog/multitenancy-in-elixir?utm_medium=email&utm_source=elixir-radar#multi-tenancy-with-foreign-keys)
7.  [Triplex - external Elixir library for managing multi-tenancy applications!](https://curiosum.com/blog/multitenancy-in-elixir?utm_medium=email&utm_source=elixir-radar#triplex---external-elixir-library-for-managing-multi-tenancy-applications)
8.  [Final words](https://curiosum.com/blog/multitenancy-in-elixir?utm_medium=email&utm_source=elixir-radar#final-words)
9.  [Questions to Consider When Evaluating Multitenancy for Your Project](https://curiosum.com/blog/multitenancy-in-elixir?utm_medium=email&utm_source=elixir-radar#questions-to-consider-when-evaluating-multitenancy-for-your-project)

This powerful concept allows you to create scalable, secure, and personalized experiences for each client without the hassle of maintaining separate systems. Imagine being able to provide each user with a tailored environment while you effortlessly control everything from one central hub. Intrigued? Let's delve into the world of multi-tenancy and discover how it can transform your application management strategy!

## But, what is multitenancy basically?

Multi-tenancy is a software architecture where a single instance of a software application serves multiple customers often called "tenants." Each tenant's data and configurations are isolated from one another, even though they share the same infrastructure. This approach allows for efficient resource utilization, cost savings and simplified maintenance.

In essence, multi-tenancy means that multiple users or groups of users (tenants) can operate within the same application environment, with each tenant's data kept separate and secure. This architecture is particularly useful for cloud-based services and SaaS (Software as a Service) applications, where scalability and resource optimization are critical.

## What are the types of multitenancy?

Multitenancy is a software architecture where a single instance of an application serves multiple customers (tenants), with each tenant's data being isolated from others. The main types of multitenancy differ in the degree of isolation and resource sharing among tenants.

Here are the main types of multitenancy configuration with pros and cons:

1.  **Shared Database, Shared Schema**
    
    -   **Description**: All tenants use the same database and the same schema. Data is separated by a tenant identifier (e.g., `tenant_id` column, or given table primary key field).
    -   **Advantages**: Ease of management, lower operational costs, resource savings.
    -   **Disadvantages**: Higher risk of unauthorized data access due to errors, difficulties in scaling for large tenants, and less flexibility.
2.  **Shared Database, Separate Schema**
    
    -   **Description**: All tenants use the same database but have separate database schemas.
    -   **Advantages**: Better data isolation between tenants, and greater flexibility in customizing schemas for individual tenant needs.
    -   **Disadvantages**: Increased complexity in database management, challenges in scaling, and higher operational costs than shared schema.
3.  **Separate Database**
    
    -   **Description**: Each tenant uses a separate database.
    -   **Advantages**: The highest level of data isolation, easier customization of the database to meet specific tenant needs, and better scalability for large tenants.
    -   **Disadvantages**: Higher operational and management costs, greater resource consumption, and more complex migrations and upgrades.
4.  **Hybrid Multitenancy**
    
    -   **Description**: Combines elements of different types of multitenancy, e.g., shared database and schema for smaller tenants, and separate databases for larger or more demanding tenants.
    -   **Advantages**: Flexibility in adjusting the architecture to the specific needs of different tenant groups, potential optimization of costs and resources.
    -   **Disadvantages**: Greater architectural and management complexity, potentially higher deployment and maintenance costs.

## Who should use multitenancy?

Multitenancy is suitable for a variety of scenarios, especially for organizations and applications that aim to serve multiple customers or clients with a single software instance. Here are some key groups for whom multitenancy is beneficial:

1.  **Software as a service (SaaS) Providers**
2.  **Government and Educational Institutions**
3.  **Customer Relationship Management (CRM) Systems**
4.  **Financial and Bank Institutions**

## Enough of theory for now

After this 'short' introduction, let's jump in into multitenancy in Elixir! We are going to discuss three examples:

-   usage of schema prefixes in [Ecto queries](https://curiosum.com/blog/composable-elixir-ecto-queries-modules),
-   usage of the primary key to gain more secure access to resources,
-   Triplex - external library to manage multi-tenant applications

## Multi-tenancy with schema prefixes in Ecto

Let's start with the example of a multi-tenant application, where we will use schema prefixes.

### What are schema prefixes?

In relational databases such as PostgreSQL, schema prefixes allow you to logically group database objects (such as tables) under different namespaces. It might be useful in multi-tenant applications, where each tenant needs isolated data - and thanks to the schema prefixes, we can achieve isolation while still using a single database!

### Implementation in Elixir

Let's start by creating a bare [Phoenix](https://curiosum.com/blog/phoenix-framework-guide) app

```bash
curiosum@> mix phx.new schema_prefixes curiosum@> cd schema_prefixes schema_prefixes@> mix ecto.setup
```

and then create a migration to create our tenants' schemas (prefixes). Later on in the article I will show, how to create prefixes with an external library usage - [Triplex](https://github.com/ateliware/triplex).

```bash
schema_prefixes@> mix ecto.gen.migration create_sample_prefixes
```

And let's define the schemas

```elixir
defmodule SchemaPrefixes.Repo.Migrations.CreateSamplePrefixes do use Ecto.Migration def change do execute "CREATE SCHEMA team_a" execute "CREATE SCHEMA team_b" end end
```

and let's migrate it!

```bash
schema_prefixes@> mix ecto.migrate 22:14:16.793 [info] == Running 20240714195112 SchemaPrefixes.Repo.Migrations.CreateSamplePrefixes.change/0 forward 22:14:16.797 [info] execute "CREATE SCHEMA team_a" 22:14:16.802 [info] execute "CREATE SCHEMA team_b" 22:14:16.804 [info] == Migrated 20240714195112 in 0.0s
```

We need one more ecto migration, to create users table:

```bash
schema_prefixes@> mix ecto.gen.migration create_users
```

The migration looks like this:

```elixir
defmodule SchemaPrefixes.Repo.Migrations.CreateUsers do use Ecto.Migration def change do Enum.each(["team_a", "team_b"], fn prefix -> create table(:users, prefix: prefix) do add :name, :string add :email, :string timestamps() end end) end end
```

Now, we can migrate it:

```bash
schema_prefixes@> mix ecto.migrate 22:18:33.969 [info] == Running 20240714201651 SchemaPrefixes.Repo.Migrations.CreateUsers.change/0 forward 22:18:33.973 [info] create table team_a.users 22:18:33.987 [info] create table team_b.users 22:18:33.994 [info] == Migrated 20240714201651 in 0.0s
```

As we can see from the output, two database tables were created successfully!

Now, we can inspect if those tables have indeed been created in our database. Let's connect via the terminal to the PostgreSQL session, and inspect the tables:

```sql
schema_prefixes@> psql psql (14.3) Type "help" for help. curiosum=# \c schema_prefixes_dev You are now connected to database "schema_prefixes_dev" as user "curiosum". schema_prefixes_dev=# \x Expanded display is on. schema_prefixes_dev=# \dt team_a.* List of relations -[ RECORD 1 ]---- Schema | team_a Name | users Type | table Owner | postgres schema_prefixes_dev=# \dt team_b.* List of relations -[ RECORD 1 ]---- Schema | team_b Name | users Type | table Owner | postgres
```

They are indeed created! (just a note - `\dt` would return all of the tables in the default schema, which is public).

Let's code back to the Elixir and define user schema:

```elixir
defmodule SchemaPrefixes.User do use Ecto.Schema import Ecto.Changeset schema "users" do field :name, :string field :email, :string timestamps() end def changeset(user, attrs) do user |> cast(attrs, [:name, :email]) |> validate_required([:name, :email]) end end
```

and user context file, with some simple actions (such as [ecto queries](https://curiosum.com/blog/composable-elixir-ecto-queries-modules), or repo calls):

```elixir
defmodule SchemaPrefixes.UserContext do import Ecto.Query alias SchemaPrefixes.Repo alias SchemaPrefixes.User def list_users(tenant) do Repo.all(User, prefix: tenant) end def get_user!(tenant, id) do Repo.get!(User, id, prefix: tenant) end def create_user(tenant, attrs \\ %{}) do %User{} |> User.changeset(attrs) |> Repo.insert(prefix: tenant) end end
```

(you can read more about contexts in [Elixir](https://curiosum.com/blog/elixir-programming-language-guide) in one of our articles [here](https://curiosum.com/blog/elixir-phoenix-context-maintainability-guildelines))

Let's explain quickly what happened there - the user schema has been created as usual, and there is nothing new. The same thing applies to the user context file, but to each function, we pass as a first argument the tenant prefix. When interacting with the database we specify the schema prefix dynamically based on that passed tenant parameter.

Let's use `iex` console, to check how it works. Firstly, we are going to create users in the corresponding schemas:

```elixir
iex(2)> SchemaPrefixes.UserContext.create_user("team_a", %{name: "first user", email: "curiosum@team_a.com"}) [debug] QUERY OK source="users" db=9.9ms decode=1.5ms queue=6.0ms idle=468.1ms INSERT INTO "team_a"."users" ("email","name","inserted_at","updated_at") VALUES ($1,$2,$3,$4) RETURNING "id" ["curiosum@team_a.com", "first user", ~N[2024-07-14 21:05:40], ~N[2024-07-14 21:05:40]] ↳ anonymous fn/4 in :elixir.eval_external_handler/1, at: src/elixir.erl:298 {:ok, %SchemaPrefixes.User{ __meta__: #Ecto.Schema.Metadata<:loaded, "team_a", "users">, id: 1, name: "first user", email: "curiosum@team_a.com", inserted_at: ~N[2024-07-14 21:05:40], updated_at: ~N[2024-07-14 21:05:40] }} iex(3)> SchemaPrefixes.UserContext.create_user("team_b", %{name: "first user", email: "curiosum@team_b.com"}) [debug] QUERY OK source="users" db=3.6ms queue=2.3ms idle=1023.3ms INSERT INTO "team_b"."users" ("email","name","inserted_at","updated_at") VALUES ($1,$2,$3,$4) RETURNING "id" ["curiosum@team_b.com", "first user", ~N[2024-07-14 21:07:02], ~N[2024-07-14 21:07:02]] ↳ anonymous fn/4 in :elixir.eval_external_handler/1, at: src/elixir.erl:298 {:ok, %SchemaPrefixes.User{ __meta__: #Ecto.Schema.Metadata<:loaded, "team_b", "users">, id: 1, name: "first user", email: "curiosum@team_b.com", inserted_at: ~N[2024-07-14 21:07:02], updated_at: ~N[2024-07-14 21:07:02] }}
```

Let's check in the `psql` console, whether the values were inserted correctly!

```sql
schema_prefixes_dev=# SELECT * FROM team_a.users; -[ RECORD 1 ]-------------------- id | 1 name | first user email | curiosum@team_a.com inserted_at | 2024-07-14 21:05:40 updated_at | 2024-07-14 21:05:40 schema_prefixes_dev=# SELECT * FROM team_b.users; -[ RECORD 1 ]-------------------- id | 1 name | first user email | curiosum@team_b.com inserted_at | 2024-07-14 21:07:02 updated_at | 2024-07-14 21:07:02
```

Indeed, we have two different tenants - hence, the existing entries are isolated!

Let's check the other two functions - `get` and `list`:

```elixir
iex(7)> SchemaPrefixes.UserContext.get_user!("team_a", 1) [debug] QUERY OK source="users" db=2.7ms queue=0.1ms idle=1996.9ms SELECT u0."id", u0."name", u0."email", u0."inserted_at", u0."updated_at" FROM "team_a"."users" AS u0 WHERE (u0."id" = $1) [1] ↳ anonymous fn/4 in :elixir.eval_external_handler/1, at: src/elixir.erl:298 %SchemaPrefixes.User{ __meta__: #Ecto.Schema.Metadata<:loaded, "team_a", "users">, id: 1, name: "first user", email: "curiosum@team_a.com", inserted_at: ~N[2024-07-14 21:05:40], updated_at: ~N[2024-07-14 21:05:40] } iex(8)> SchemaPrefixes.UserContext.get_user!("team_b", 1) [debug] QUERY OK source="users" db=1.1ms queue=2.0ms idle=1953.2ms SELECT u0."id", u0."name", u0."email", u0."inserted_at", u0."updated_at" FROM "team_b"."users" AS u0 WHERE (u0."id" = $1) [1] ↳ anonymous fn/4 in :elixir.eval_external_handler/1, at: src/elixir.erl:298 %SchemaPrefixes.User{ __meta__: #Ecto.Schema.Metadata<:loaded, "team_b", "users">, id: 1, name: "first user", email: "curiosum@team_b.com", inserted_at: ~N[2024-07-14 21:07:02], updated_at: ~N[2024-07-14 21:07:02] } iex(9)> SchemaPrefixes.UserContext.list_users("team_a") [debug] QUERY OK source="users" db=0.7ms queue=1.1ms idle=1365.9ms SELECT u0."id", u0."name", u0."email", u0."inserted_at", u0."updated_at" FROM "team_a"."users" AS u0 [] ↳ anonymous fn/4 in :elixir.eval_external_handler/1, at: src/elixir.erl:298 [ %SchemaPrefixes.User{ __meta__: #Ecto.Schema.Metadata<:loaded, "team_a", "users">, id: 1, name: "first user", email: "curiosum@team_a.com", inserted_at: ~N[2024-07-14 21:05:40], updated_at: ~N[2024-07-14 21:05:40] } ] iex(10)> SchemaPrefixes.UserContext.list_users("team_b") [debug] QUERY OK source="users" db=0.7ms queue=0.9ms idle=1880.7ms SELECT u0."id", u0."name", u0."email", u0."inserted_at", u0."updated_at" FROM "team_b"."users" AS u0 [] ↳ anonymous fn/4 in :elixir.eval_external_handler/1, at: src/elixir.erl:298 [ %SchemaPrefixes.User{ __meta__: #Ecto.Schema.Metadata<:loaded, "team_b", "users">, id: 1, name: "first user", email: "curiosum@team_b.com", inserted_at: ~N[2024-07-14 21:07:02], updated_at: ~N[2024-07-14 21:07:02] } ]
```

Nice, the users have been fetched correctly from each schema!

### That looks nice - but what if I would like to add columns to existing tables?

Let's kick this off by creating a new migration:

```bash
schema_prefixes@> mix ecto.gen.migration add_new_column_to_users
```

In that case, we can not just alter the column into `users table`. What we have to do here instead, is to migrate data on all different tenants - in that case, we can imagine something like, that our table name will be `team_a.users`. So the migration will look like this:

```elixir
defmodule SchemaPrefixes.Repo.Migrations.AddNewColumnToUsers do use Ecto.Migration def change do Enum.each(["team_a", "team_b"], fn prefix -> alter table(:users, prefix: prefix) do add :password, :string end end) end end
```

Let's run the migration and see the result:

```bash
schema_prefixes@> mix ecto.migrate 23:22:09.460 [info] == Running 20240714211747 SchemaPrefixes.Repo.Migrations.AddNewColumnToUsers.change/0 forward 23:22:09.462 [info] alter table team_a.users 23:22:09.470 [info] alter table team_b.users 23:22:09.471 [info] == Migrated 20240714211747 in 0.0s
```

Neat, we have a new column in our isolated tables!

### But, do I really have to pass the tenant's name as a function argument each time...?

The answer to this question is **no**! Firstly, we have to delete each tenant argument in our `user context` file:

```elixir
def list_users() do Repo.all(User) end def get_user!(id) do Repo.get!(User, id) end def create_user(attrs \\ %{}) do %User{} |> User.changeset(attrs) |> Repo.insert() end
```

The second thing is to keep the value of the tenant inside the Elixir processes, and set a customizable callback inside the repository module, to fetch the current prefix:

```elixir
defmodule SchemaPrefixes.Repo do use Ecto.Repo, otp_app: :schema_prefixes, adapter: Ecto.Adapters.Postgres @impl true @doc """ This callback is invoked as the entry point for all repository operations. This can be used to provide default values per operation. """ def default_options(_options) do tenant = get_tenant() if tenant, do: [prefix: tenant] end @tenant_key :tenant def set_tenant(tenant) do Process.put(@tenant_key, tenant) end def get_tenant() do Process.get(@tenant_key) end end
```

as we have our code defined, let's jump into `iex` console again:

```elixir
iex(1)> SchemaPrefixes.Repo.set_tenant("team_a") nil iex(2)> SchemaPrefixes.Repo.get_tenant() "team_a" iex(3)> SchemaPrefixes.UserContext.list_users() [debug] QUERY OK source="users" db=4.6ms decode=1.2ms queue=1.1ms idle=235.5ms SELECT u0."id", u0."name", u0."email", u0."inserted_at", u0."updated_at" FROM "team_a"."users" AS u0 [] ↳ anonymous fn/4 in :elixir.eval_external_handler/1, at: src/elixir.erl:298 [ %SchemaPrefixes.User{ __meta__: #Ecto.Schema.Metadata<:loaded, "team_a", "users">, id: 1, name: "first user", email: "curiosum@team_a.com", inserted_at: ~N[2024-07-14 21:05:40], updated_at: ~N[2024-07-14 21:05:40] } ]
```

The `prefix: tenant` option has been applied on the `default_options` function level, and thanks to that we, as developers, do not have to remember about passing the tenant as an argument to any function anymore!

Isn't that awesome?

## Multi-tenancy with foreign keys

Let's explore how to implement multi-tenancy using foreign keys in Ecto.

### What are foreign keys in multi-tenancy?

Foreign keys are used to establish a link between two tables. In a multi-tenant architecture, foreign keys can be utilized to associate records with specific tenants. In this approach, the idea is that most, if not all, resources in the system belong to a given user. In other words, we add to each table a unique `tenant_id` (it can be e.g. `user_id`) foreign key. Thanks to this, we can ensure data isolation and integrity.

### Implementation in Elixir

Let's start by creating a bare Phoenix app, in the same way as in the previous chapter:

```bash
curiosum@> mix phx.new foreign_keys curiosum@> cd foreign_keys foreign_keys@> mix ecto.setup
```

Next, create a migration for users table:

```bash
foreign_keys@> mix ecto.gen.migration create_users
```

and define the users table:

```elixir
defmodule ForeignKeys.Repo.Migrations.CreateUsers do use Ecto.Migration def change do create table(:users) do add :name, :string add :email, :string timestamps() end end end
```

We have to also create a blog table:

```bash
foreign_keys@> mix ecto.gen.migration create_blogs
```

```elixir
defmodule ForeignKeys.Repo.Migrations.CreateBlogs do use Ecto.Migration def change do create table(:blogs) do add :title, :string add :user_id, references(:users, on_delete: :delete_all) timestamps() end end end
```

and links table:

```bash
foreign_keys@> mix ecto.gen.migration create_links
```

```elixir
defmodule ForeignKeys.Repo.Migrations.CreateLinks do use Ecto.Migration def change do create table(:links) do add :url, :string add :blog_id, references(:blogs, on_delete: :delete_all) add :user_id, references(:users, on_delete: :delete_all) timestamps() end end end
```

now we can use `mix ecto` and migrate created migrations:

```bash
foreign_keys@> mix ecto.migrate 00:22:20.959 [info] == Running 20240714221608 ForeignKeys.Repo.Migrations.CreateUsers.change/0 forward 00:22:20.962 [info] create table users 00:22:20.976 [info] == Migrated 20240714221608 in 0.0s 00:22:21.013 [info] == Running 20240714221902 ForeignKeys.Repo.Migrations.CreateBlogs.change/0 forward 00:22:21.013 [info] create table blogs 00:22:21.022 [info] == Migrated 20240714221902 in 0.0s 00:22:21.024 [info] == Running 20240714221928 ForeignKeys.Repo.Migrations.CreateLinks.change/0 forward 00:22:21.024 [info] create table links 00:22:21.027 [info] == Migrated 20240714221928 in 0.0s
```

our business logic is defined in the database - let's now create Elixir schemas:

`user.ex`:

```elixir
defmodule ForeignKeys.User do use Ecto.Schema import Ecto.Changeset schema "users" do field :name, :string field :email, :string has_many :blogs, ForeignKeys.Blog timestamps() end def changeset(user, attrs) do user |> cast(attrs, [:name, :email]) |> validate_required([:name, :email]) end end
```

`blog.ex`:

```elixir
defmodule ForeignKeys.Blog do use Ecto.Schema import Ecto.Changeset schema "blogs" do field :title, :string belongs_to :user, ForeignKeys.User has_many :links, ForeignKeys.Link timestamps() end def changeset(blog, attrs) do blog |> cast(attrs, [:title, :user_id]) |> validate_required([:title, :user_id]) end end
```

`link.ex`:

```elixir
defmodule ForeignKeys.Link do use Ecto.Schema import Ecto.Changeset schema "links" do field :url, :string belongs_to :blog, ForeignKeys.Blog belongs_to :user, ForeignKeys.User timestamps() end def changeset(link, attrs) do link |> cast(attrs, [:url, :blog_id, :user_id]) |> validate_required([:url, :blog_id, :user_id]) end end
```

and let's define contexts:

`user_context.ex`:

```elixir
defmodule ForeignKeys.UserContext do import Ecto.Query, warn: false alias ForeignKeys.Repo alias ForeignKeys.User def list_users do Repo.all(User) end def get_user!(id), do: Repo.get!(User, id) def create_user(attrs \\ %{}) do %User{} |> User.changeset(attrs) |> Repo.insert() end def update_user(%User{} = user, attrs) do user |> User.changeset(attrs) |> Repo.update() end def delete_user(%User{} = user) do Repo.delete(user) end end
```

`blog_context.ex`:

```elixir
defmodule ForeignKeys.BlogContext do import Ecto.Query, warn: false alias ForeignKeys.Repo alias ForeignKeys.Blog def list_user_blogs(user_id) do Repo.all(from b in Blog, where: b.user_id == ^user_id) end def create_blog(user_id, attrs \\ %{}) do %Blog{} |> Blog.changeset(Map.put(attrs, :user_id, user_id)) |> Repo.insert() end end
```

`link_context.ex`:

```elixir
defmodule ForeignKeys.LinkContext do import Ecto.Query, warn: false alias ForeignKeys.Repo alias ForeignKeys.Link def list_blog_links_by_blog_id(blog_id) do Repo.all(from l in Link, where: l.blog_id == ^blog_id) end def list_blog_links_by_user_id(user_id) do Repo.all(from l in Link, where: l.user_id == ^user_id) end def create_link(user_id, blog_id, attrs \\ %{}) do %Link{} |> Link.changeset(Map.merge(attrs, %{user_id: user_id, blog_id: blog_id})) |> Repo.insert() end end
```

Okay, what's going on here? We have defined very basic functions in our context modules, such as CRUD actions (create, read, update, delete). The surprising thing here is the connection between `link` and `user` - normally we would associate the `link` table, just with the `blogs` (and then, by `joins`, get given links by user). But as stated at the very beginning of this chapter, this is intentional! We can fetch all of the `links` by `user_id`!

Let's talk about the advantages and disadvantages of this approach:

### Pros

-   data isolation - foreign keys ensure that each record is associated with a specific tenant, preventing data leakage between tenants. This isolation makes the data store more secure and reliable,
-   simplifies queries - filtering records by `tenant_id` becomes straightforward, making it easy to retrieve information specific to a tenant,
-   ease of implementation - adding a `tenant_id` to tables and establishing foreign key relationships is a relatively simple and well-understood approach. Configuring the database schema this way ensures that each tenant's data is properly stored and managed,
-   performance - thanks to having `tenant_id` in each table, we can omit multiple join statements, which can lead to query speed-up.

### Cons

-   redundancy - including `tenant_id` in many tables can lead to huge redundancy, which might require additional storage space,
-   the potential of data inconsistencies - if not carefully managed, there is a risk of data inconsistencies (orphaned records, mismatched tenant IDs, missing foreign keys constraints, or inconsistent deletions),
-   scalability - as the number of tenants grows, tables can become very large, which might impact performance (hence, solutions such as partitioning or sharding might be needed)

## Triplex - external [Elixir library](https://curiosum.com/blog/elixir-2021-now-and-future) for managing multi-tenancy applications!

Triplex is an [Elixir library](https://curiosum.com/blog/elixir-2021-now-and-future), which helps manage multi-tenancy by automatically creating and handling tenant-specific PostgreSQL schemas. It simplifies tenant operations like migrations, schema creation, and switching between tenants.

Let's create the relation between `tenants` and `users`, and for each newly created user add its own prefix.

### Sample implementation in [Elixir](https://curiosum.com/blog/elixir-programming-language-guide)

For the last time, let's start by creating a bare [Phoenix](https://curiosum.com/blog/phoenix-framework-guide) app:

```bash
curiosum@> mix phx.new triplex_usage curiosum@> cd triplex_usage triplex_usage@> mix ecto.setup
```

and let's add and set up Triplex in our app - firstly, let's add deps in `mix.exs`:

```elixir
def deps do [ {:triplex, "~> 1.3.0"}, ] end
```

and run the following command in the terminal:

```bash
triplex_usage@> mix deps.get
```

as the final step, we need to add the following line in the `config/config.exs` file:

```elixir
config :triplex, repo: TriplexUsage.Repo
```

and that's for the configuration! (if you use MySQL, not PSQL as me, please refer to their documentation on how to proceed!).

Next, let's create the `tenants` migration

```bash
triplex_usage@> mix ecto.gen.migration create_tenants_table
```

```elixir
defmodule TriplexUsage.Repo.Migrations.CreateTenantsTable do use Ecto.Migration def change do create table(:tenants) do add :uuid, :string timestamps() end end end
```

Next, let's define the users table - to run migrations across tenant schemas, we will use the Triplex migration alias:

```bash
triplex_usage@> mix triplex.gen.migration create_users
```

and magically need folder just showed in the `priv` directory

```bash
triplex_usage/priv/repo/tenant_migrations/20240715205517_create_users.exs
```

Let's define our `users` schema

```elixir
defmodule TriplexUsage.Repo.Migrations.CreateUsers do use Ecto.Migration def change do create table(:users) do add :name, :string add :email, :string add :tenant_id, references(:tenants, prefix: "public") timestamps() end end end
```

and migrate them

```bash
triplex_usage@> mix ecto.migrate && mix triplex.migrate
```

(check whether the result from running migrations is fine!)

Let's define again the schema modules

```elixir
defmodule TriplexUsage.User do use Ecto.Schema import Ecto.Changeset schema "users" do field :name, :string field :email, :string belongs_to(:tenant, TriplexUsage.Tenant) timestamps() end def changeset(user, attrs) do user |> cast(attrs, [:name, :email]) |> validate_required([:name, :email]) end end
```

```elixir
defmodule TriplexUsage.Tenant do use Ecto.Schema import Ecto.Changeset schema "tenants" do field :uuid, :string timestamps() end def changeset(tenant, attrs) do tenant |> cast(attrs, [:uuid]) |> validate_required([:uuid]) end def create_tenant() do Triplex.create(Ecto.UUID.generate()) end end
```

and define the `tenants` module, in which we will handle the logic for managing the app

```elixir
defmodule TriplexUsage.Tenants do alias Ecto.Multi alias TriplexUsage.Tenant alias TriplexUsage.User alias TriplexUsage.Repo @tenant_id_key :tenant_id def set_schema(schema) do Process.put(@tenant_id_key, schema) end def get_schema do Process.get(@tenant_id_key) || "public" end @doc """ Steps: 1. Create prefix/schema for given user, 2. Write the result into `tenants` table, 3. Create user and associate it with the tenant """ def create_user do generate_schema() |> Triplex.create_schema(Repo, fn schema, repo -> {:ok, _result} = Triplex.migrate(schema, repo) Multi.new() |> Multi.run(:create_tenant, create_tenant(schema)) |> Multi.run(:create_user, create_user(schema)) |> Repo.transaction() end) end defp generate_schema, do: Ecto.UUID.generate() defp create_tenant(schema) do fn _repo, _attrs -> set_schema("public") %Tenant{} |> Tenant.changeset(%{uuid: schema}) |> Repo.insert() end end defp create_user(schema) do fn _repo, %{create_tenant: %Tenant{id: tenant_id}} -> set_schema(schema) %User{} |> User.changeset(%{ name: "test user", email: "test email", tenant_id: tenant_id }) |> Repo.insert() end end end
```

Ok, let's describe step by step what's going on here:

-   `set_schema` and `get_schema` are the getter and setter for the `prefix`, which will be later on used in the Repo call (we already defined those in the previous examples)
-   `create_user` - the simple process of creating a user in our application, what we do here is:
    
    1.  generate a new schema ID,
    2.  create the tenant schema, via the `Triplex.create_schema/2`,
    3.  migrate the tenant's schema, via the `Triplex.migrate/2`,
    4.  and then finally, runs the transaction to insert the tenant and corresponding to it user.

> Be careful with the `prefixes` you use while using your repo, in our example the tenants table should be created on the "public" prefix (`set_schema("public")`), and the user itself on given schema (`set_schema(schema)`)

Before testing it out, we have to specify one more thing in our repository module:

```elixir
defmodule TriplexUsage.Repo do use Ecto.Repo, otp_app: :triplex_usage, adapter: Ecto.Adapters.Postgres @impl true def default_options(_operation) do [prefix: Triplex.to_prefix(TriplexUsage.Tenants.get_schema())] end end
```

Once we have everything, we are ready to test things out in `iex` terminal!

```elixir
iex(1)> TriplexUsage.Tenants.create_user() {:ok, "a7aa361b-0b5c-4649-93ea-d0b6ab18e7f4"}
```

Let's check what it looks like in the database tables (I am again using PSQL to query existing entries, as you wish you can use repo functions to get [Elixir structs](https://curiosum.com/blog/elixir-trickery-cheating-on-structs)):

```sql
triplex_usage_dev=# SELECT * FROM tenants; -[ RECORD 1 ]------------------------------------- id | 9 uuid | a7aa361b-0b5c-4649-93ea-d0b6ab18e7f4 inserted_at | 2024-07-15 21:48:17 updated_at | 2024-07-15 21:48:17 triplex_usage_dev=# SELECT * FROM "a7aa361b-0b5c-4649-93ea-d0b6ab18e7f4".users; -[ RECORD 1 ]-------------------- id | 1 name | test user email | test email tenant_id | inserted_at | 2024-07-15 21:48:17 updated_at | 2024-07-15 21:48:17
```

Neat! Our goal has been reached!

## Final words

Multi-tenancy is a powerful tool for creating scalable, secure, and efficient applications. By isolating tenant data while sharing infrastructure, you can offer tailored environments for each client. Libraries like Triplex simplify managing tenant-specific PostgreSQL schemas, enabling seamless access to map external data accurately across tenants. This approach ensures consistent value, optimal performance, and error-free operation, even when accessed through web browsers. Embracing multi-tenancy leverages the full power of modern computers, making it an ideal solution for growing applications.

The app examples are available at [github](https://github.com/itamm15/elixir_multitenancy)!

Happy coding, and I hope, it is not working only on my computer :D

## Questions to Consider When Evaluating Multitenancy for Your Project

1.  **What is the scale of your database instance?**
2.  **What are your data isolation requirements?**
3.  **What is your budget for infrastructure and maintenance?**
4.  **How important is customization for each tenant?**
5.  **What are the security implications?**
6.  **What is the complexity of your existing infrastructure?**

## FAQ

### What is multitenancy in Elixir?

Multitenancy in Elixir refers to a software architecture where a single instance of an application serves multiple customers (tenants), isolating their data while sharing resources.

### What are the benefits of multitenancy?

Benefits include efficient resource utilization, cost savings, simplified maintenance, scalability, and secure data isolation for different tenants.

### What types of multitenancy are there?

Types include Shared Database, Shared Schema; Shared Database, Separate Schema; Separate Database; and Hybrid Multitenancy, each with varying degrees of isolation and resource sharing.

### How can multitenancy be implemented in Elixir?

In Elixir, multitenancy can be implemented using schema prefixes in Ecto, foreign keys, or external libraries like Triplex.

### What are schema prefixes in Ecto?

Schema prefixes in Ecto allow logical grouping of database objects under different namespaces, enabling data isolation within a single database.

### How to create schema prefixes in Ecto?

Create schema prefixes by defining them in migrations and using the `prefix` option in Ecto queries to specify the schema.

### What is the role of foreign keys in multitenancy?

Foreign keys associate records with specific tenants, ensuring data integrity and isolation by linking each resource to a tenant identifier.

### Can external libraries help with multitenancy in Elixir?

Yes, libraries like Triplex can manage multi-tenant applications in Elixir, offering additional features and simplifications for handling multitenancy.

### What are the challenges of multitenancy?

Challenges include increased complexity in database management, potential higher operational costs, and ensuring data isolation and security.

### Who benefits from multitenancy?

Multitenancy is beneficial for SaaS providers, government and educational institutions, CRM systems, and financial and banking institutions.
