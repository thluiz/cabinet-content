---
created: 2024-08-28T15:02:02 (UTC -03:00)
tags: []
source: https://curiosum.com/blog/elixir-anti-patterns?utm_medium=email&utm_source=elixir-radar
author: Mateusz Tatarski
---

# Common Anti-Patterns in Elixir Projects and How to Avoid Them | Curiosum

> ## Excerpt
> Learn practical strategies to enhance your code quality, safeguard your applications, and ensure your Elixir projects are efficient, secure, and maintainable.

---
We all love Elixir and know how great it is, but that's not what this article is about. Today we will focus on things in Elixir that may cause you to shoot yourself in the foot (or blow your whole leg off).

Table of contents

1.  [Mass assignment vulnerability through the Ecto changeset](https://curiosum.com/blog/elixir-anti-patterns?utm_medium=email&utm_source=elixir-radar#mass-assignment-vulnerability-through-the-ecto-changeset)
2.  [Using Ecto schemas in database migrations](https://curiosum.com/blog/elixir-anti-patterns?utm_medium=email&utm_source=elixir-radar#using-ecto-schemas-in-database-migrations)
3.  [Dynamic atom creation](https://curiosum.com/blog/elixir-anti-patterns?utm_medium=email&utm_source=elixir-radar#dynamic-atom-creation)
4.  [Directly interpolating user input into the query string (SQL injection)](https://curiosum.com/blog/elixir-anti-patterns?utm_medium=email&utm_source=elixir-radar#directly-interpolating-user-input-into-the-query-string-(sql-injection))
5.  [Rendering data from untrusted user input in web page (XSS attack)](https://curiosum.com/blog/elixir-anti-patterns?utm_medium=email&utm_source=elixir-radar#rendering-data-from-untrusted-user-input-in-web-page-(xss-attack))
6.  [Small Elixir code pitfalls you should be aware of](https://curiosum.com/blog/elixir-anti-patterns?utm_medium=email&utm_source=elixir-radar#small-elixir-code-pitfalls-you-should-be-aware-of)
7.  [Sobelow library will fix most of the issues](https://curiosum.com/blog/elixir-anti-patterns?utm_medium=email&utm_source=elixir-radar#sobelow-library-will-fix-most-of-the-issues)

We'll explore some of the most common pitfalls and prevalent anti-patterns encountered in Elixir projects. By understanding and avoiding these common mistakes, you can ensure that your Elixir codebase remains clean, efficient, and idiomatic. Whether you're a seasoned Elixir programmer or just beginning your journey with the language, being aware of these anti-patterns will help you write better, more robust code and ensure high code quality.

## Mass assignment vulnerability through the Ecto changeset

### Mass Assignment

Mass assignment is a vulnerability that allows users modify data items that they should not normally be allowed to access, for example password, permissions or administrator status. It occurs when data from the user is not appropriately filtered and verified on the server side. The attacker can add new parameters to the HTTP request, and this way he is allowed to create or update data that he shouldn't be able to.

As a developer, you should keep in mind what fields you cast in the changeset. In most cases, casting all of your schema fields in changeset is not a good idea, as you allow a user to update them.

### **Vulnerable code example**

Consider a typical [Phoenix application](https://curiosum.com/blog/deploying-phoenix-app-with-mix-releases) with a **User** schema. This schema includes fields for username, email and whether the user is an admin:

```elixir
schema "users" do field :username, :string field :email, :string field :admin, :boolean, default: false end def changeset(user, attrs) do user |> cast(attrs, [:username, :email, :admin]) |> validate_required([:username, :email]) |> unique_constraint(:email) end
```

In this example, the **changeset/2** function allows casting and validating multiple attributes, including sensitive fields like **admin**. If your application exposes this changeset directly to users through a web form or API endpoint, it could allow malicious users to modify fields they shouldn't have access to. For instance, a user could attempt to grant themselves admin privileges by including the **admin** field in their request parameters:

```undefined
params = %{"username" => "new_username", "admin" => true}
```

In that case, you should be careful what fields you cast in the changeset.

### Solution

The solution to this problem is to create a system of multiple changesets, each corresponding to a given action:

```elixir
def user_changeset(user, attrs) do user |> cast(attrs, [:username, :email]) |> validate_required([:username, :email]) |> unique_constraint(:email) end def admin_changeset(user, attrs) do user |> cast(attrs, [:admin]) |> validate_inclusion(:admin, [true, false]) end
```

There is nothing wrong to create multiple changesets for your fields in your schema. In the above example we created two changesets. **user\_changeset/2** could be used for creating/updating user and **admin\_changeset/2** could be used to grant permissions to admins only on the admin side of the application.

## Using Ecto schemas in database migrations

Migrations should be immutable and reproducible. They represent a historical record of how your database schema has evolved. When you use Ecto schemas directly in migrations, you risk tying your migration logic to the current state of your application code. If your schema definitions change in future code versions, running old migrations can lead to unexpected behavior or errors.

### Examples

Let's say we have table and schema **users** in our application.

Migration:

```elixir
def change do create table(:users) do add(:email, :string) add(:permission, :string) end end
```

Schema:

```elixir
defmodule Users do schema "users" do field :email, :string field :permission, :string end end
```

Now we want to update **permission** field in **users** table. This is just an example, so do not think about the sense of this operation.

```elixir
def up do import Ecto.Query, only: [from: 2] from(u in Users, where: u.permission == "user") |> Repo.update_all(set: [permission: "default"]) end def down do ... end
```

In the future, our project will develop. We decide that we delete the **permission** field, and for example, move it to a new table. We create a new migration and update our schema:

```elixir
defmodule Users do schema "users" do field :email, :string end end
```

```elixir
def change do alter table(:users) do remove :permission end end
```

After that, if someone wants to run migrations on a fresh database, his migrations will fail as the schema will not have a permission field, and an update permission field migration will fail. To solve that, you should not use schemas in migrations, or you can define schemas in the migration file that would be used only for this migration task. You can also use raw SQL in migration files or write [schemaless queries](https://hexdocs.pm/ecto/schemaless-queries.html).

## Dynamic atom creation

Atoms in Elixir are constants whose value is their own name. They are often used for labeling and to represent a state or condition in a readable way. For instance, **:ok** and **:error** are common atoms used in Elixir to denote success and failure states, respectively. Atoms are stored in an atom table. This table is not garbage collected, which means once an atom is created, it stays in the table for the lifetime of the VM process. This table has a default limit of **1048576** atoms. If the limit is hit at runtime, the application will crash.

### Example

Let's say that in your application you want to convert string values to atoms. You take those strings from user input on your web page or from an API request. In that case, you lost control of how many atoms are created in the applications.

```elixir
def index(conn, %{"value" => string} = params) do atom = String.to_atom(string) ... render(conn, "index.html") end
```

This way, we allowed the user to generate multiple random atoms. If he generates more atoms than **1048576,** he will crash our application. To fix that, you should operate on strings if possible or use **String.to\_existing\_atom().** This way, you will parse the string only for atoms that already exist in the atom table. Overall, you should consider all possibilities of creating atoms, not only using **String.to\_atom/1** function calls but also:

-   **erlang.binary\_to\_term/2**
-   atom interpolation
-   **Jason.decode/2** with **\[keys: :atoms\]** option (is using **String.to\_atom/1** under the hood)
-   **Poison.Parser.parse/2** with **%{keys: :atoms}** option

You should not let atoms be generated from untrusted input and have control over how atoms are generated in your app. Dynamic atom creation in our app is not always bad, but as always in development, you need to know what you are doing.

## Directly interpolating user input into the query string (SQL injection)

### SQL injection

SQL injection is a common attack vector where an attacker can execute arbitrary SQL code on a database by exploiting vulnerabilities in the application's handling of user input. In Elixir applications, especially those using Ecto for database interactions, it's crucial to handle user input safely to prevent such attacks. Ecto effectively protects us against such an attack, but the attack is still possible if the programmer makes mistakes in his code, which are presented below.

### **Vulnerable Code Example**

Let's assume we have a simple [Phoenix application](https://curiosum.com/blog/deploying-phoenix-app-with-mix-releases) where users can search for products by name.

```elixir
def search(conn, %{"query" => query}) do sql = "SELECT * FROM products WHERE name = '#{query}'" products = Ecto.Adapters.SQL.query!(Repo, sql, []) render(conn, "index.html", products: products) end
```

In the above code, the user input (query) is directly interpolated into the SQL query string. An attacker could exploit this by providing a malicious input. For instance, if an attacker submits the following query:

```sql
' OR 1=1; --
```

The resulting SQL would be:

```sql
SELECT * FROM products WHERE name = '' OR 1=1; --'
```

This would execute the query, and then returns all the products in an database.

### **Safe Code Example**

To prevent SQL injection, you should use parameterized queries provided by Ecto, which automatically handle escaping user input. Here's the corrected version of the source code:

```elixir
def search(conn, %{"query" => query}) do products = list_products(query) render(conn, "index.html", products: products.rows) end def list_products(query) do # Safe: Using parameterized query to prevent SQL injection from(product in "products", as: :product) |> where([product: product], product.name == ^query) |> Repo.all() end
```

In the safe version, the query variable is passed as a parameter to the SQL query. Ecto ensures that the input is properly escaped, preventing any SQL injection attempts. The placeholder $1 is used in the query string, and the actual user input is passed in as part of the parameter list (\[query\]).

By using parameterized queries or other mechanisms that safely escape user input, you can protect your Elixir applications from SQL injection attacks. It's essential to be aware of these practices to ensure the security and integrity of your application and its data.

## Rendering data from untrusted user input in web page (XSS attack)

### Cross-Site Scripting (XSS)

Cross-Site Scripting (XSS) is a type of security vulnerability that allows an attacker to inject malicious scripts into trusted websites viewed by other users. This attack occurs when untrusted data is displayed on a web page without proper validation or escaping, allowing attackers to execute their scripts in the context of the target users browsers.

[Phoenix](https://curiosum.com/blog/phoenix-framework-guide), which is most commonly used for building applications in Elixir, provides very good security from XSS attacks, and it's not easy to make a mistake and create an XSS vulnerability in your application.

### **Vulnerable Code Example**

Let's assume we have a controller with an index function that renders a page:

```elixir
def index(conn, %{"message" => message}) do render(conn, "index.html", message: message) end
```

and then in template:

```elixir
<h2>Message: </h2> <%= raw(@message) %>
```

Assuming that you put in url such query: `?message=<script>alert(1)</script>` it will render this alert on your web page.

You can create vulnerability on XSS attack not only using **raw/1** function but also using:

-   [Phoenix.Controller.html/2](https://hexdocs.pm/phoenix/Phoenix.Controller.html#html/2)
-   using **put\_resp\_content\_type** and **send\_resp** functions

### Preventing XSS

As described above, [Phoenix](https://curiosum.com/blog/phoenix-framework-guide) is doing a good job of preventing XSS attacks in applications. So overall, don't use the above functions on untrusted user input, and everything should be just fine. Except that, you can also:

-   Sanitize Input: remove or escape any characters that can be interpreted as code.
-   Use Safe Functions: When rendering user input in HTML, use functions that automatically escape HTML characters.
-   Content Security Policy (CSP): Implement CSP headers to restrict where scripts can be loaded from. This can prevent the execution of unauthorized scripts.
-   Validate and Encode: Validate user input on both client-side and server-side. Encode data appropriately when rendering it in HTML, JavaScript, or other contexts.

## Small Elixir code pitfalls you should be aware of

In this section, I will present small examples of strange things in [Elixir](https://curiosum.com/blog/elixir-programming-language-guide) that you may not have known about and which may cause problems in the future when working on the Elixir codebase.

### Pattern matching on empty map %{}

Looking at how pattern matching works for lists and keyword lists, you may think that if you want to pattern match on an empty map, you do something like this:

```elixir
def function(%{}), do: :empty_map def function(not_empty_map), do: :not_empty_map
```

and you would be wrong because the first function clause matches any map, no matter if it has keys or not. If you want to pattern match on an empty map, you should use the guard clause, as in the example below:

```elixir
def function(map) when map == %{}, do: :empty_map def function(not_empty_map), do: :not_empty_map
```

It works that way because whenever you want to pattern match any map, you would have to provide all the keys, which would not be very ergonomic. Instead, when pattern matching on maps, we can get a single key.

### Pattern matching on keyword list does not work as on maps

Another pitfall you can fall into is pattern matching on the keyword list. And don't get me wrong, there is nothing wrong with that but it is important to know how it works. So we saw how pattern matching works on an empty map before. Using a keyword list, you can pattern match in different ways. Pattern matching works on an empty list without a guard:

```elixir
# it works! def function([]), do: :empty_list def function(_), do: :not_empty_list
```

but something to remember is that when pattern matching on a keyword list, the order of keys is important, which is different from the case of a map where order of keys is not important at all. Example:

```elixir
def function([first_arg: _, second_arg: _]), do: "first function clause" def function(_), do: "second function clause" function([second_arg: "arg", first_arg: "arg"]) # it will return "second function clause" so it will not match first clause!
```

Small thing, but worth mentioning and remembering.

### You can compare every type with every type

Unlike many other programming languages that restrict comparisons to values of the same type, Elixir allows comparisons between any types. This flexibility can be incredibly useful, but it also requires a solid understanding to avoid unexpected results. Example:

```undefined
"1" > 2 # it returns true
```

Most people who do not know [Elixir](https://curiosum.com/blog/elixir-programming-language-guide) would think that it would raise an exception or return false, but in Elixir, the comparison between types works a little bit differently. This is based on a specific term ordering, which can be summarized as follows (from lowest to highest):

1.  Numbers
2.  Atoms
3.  References
4.  Functions
5.  Ports
6.  PIDs
7.  Tuples
8.  Maps
9.  Lists
10.  Binaries

Example:

```undefined
1 < :atom # true, because numbers are lower in term ordering than atoms :atom < [1, 2, 3] # true, because atoms are lower in term ordering than lists {1, 2} < [1, 2] # true, because tuples are lower in term ordering than lists
```

## Sobelow library will fix most of the issues

Using [Sobelow](https://github.com/nccgroup/sobelow) library will prevent most of the cases described here. So it is important to add this library to your Elixir project and add it to your CI pipeline. List of checks that Sobelow can perform:

-   Insecure configuration
-   Known-vulnerable Dependencies
-   Cross-Site Scripting
-   SQL injection
-   Command injection
-   Code execution
-   Denial of Service
-   Directory traversal
-   Unsafe serialization

Sobelow will not prevent the mass assignment vulnerability through the Ecto changeset. You should also check official Elixir docs with [Elixir anti-patterns](https://hexdocs.pm/elixir/what-anti-patterns.html).

## FAQ

### What are common anti-patterns in Elixir projects?

Common anti-patterns in Elixir projects include mass assignment vulnerability, using Ecto schemas in database migrations, dynamic atom creation, SQL injection, and rendering untrusted user input (XSS attack).

### What is mass assignment vulnerability in Elixir?

Mass assignment vulnerability occurs when user input is not properly filtered, allowing users to modify protected fields like admin status. To avoid this, use multiple changesets tailored for different actions.

### Why should Ecto schemas not be used in migrations?

Using Ecto schemas in migrations ties migration logic to the current state of the application code, leading to potential errors if the schema changes in the future. Instead, use raw SQL or define schemas within the migration file.

### How can dynamic atom creation be harmful in Elixir?

Dynamic atom creation can lead to the exhaustion of the atom table, causing the application to crash. Avoid creating atoms from untrusted input and use `String.to_existing_atom/1` where necessary.

### What is SQL injection and how can it be prevented in Elixir?

SQL injection is an attack where arbitrary SQL code is executed by exploiting user input vulnerabilities. Prevent it by using parameterized queries provided by Ecto, which safely handle user input.

### How does Cross-Site Scripting (XSS) occur and how can it be prevented?

XSS occurs when untrusted data is rendered in a web page without proper validation or escaping. Prevent it by sanitizing input, using safe functions for rendering HTML, implementing Content Security Policy (CSP), and validating user input.

### What are some small Elixir code pitfalls to be aware of?

Small pitfalls include incorrect pattern matching on empty maps, keyword lists, and the flexibility of comparing every type with every type in Elixir.

### How does pattern matching on empty maps work differently than expected?

Pattern matching on an empty map using `%{}` matches any map. To match specifically an empty map, use a guard clause.

### How does term comparison work in Elixir?

Elixir allows comparisons between any types based on a specific term ordering, from numbers to binaries. This flexibility can lead to unexpected results if not properly understood.

### What library can help prevent many Elixir anti-patterns?

The Sobelow library helps prevent various security issues such as insecure configurations, SQL injection, and XSS by performing checks and should be added to the CI pipeline.
