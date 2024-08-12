---
created: 2024-07-30T19:28:47 (UTC -03:00)
tags: [webdev,javascript,beginners,programming,software,coding,development,engineering,inclusive,community]
source: https://dev.to/vyan/how-to-structure-your-backend-code-in-nodejs-expressjs-2bdd?context=digest
author: 
---

# How to Structure Your Backend Code in Node.js (Express.js) - DEV Community

> ## Excerpt
> When developing a Node.js application using Express.js, structuring your codebase effectively is...

---
When developing a Node.js application using Express.js, structuring your codebase effectively is crucial for maintainability, scalability, and ease of collaboration. A well-organized project structure allows you to manage complexity, making it easier to navigate and understand the code. In this blog, we'll explore a typical folder structure for an Express.js application and explain the purpose of each directory and file.

## [](https://dev.to/vyan/how-to-structure-your-backend-code-in-nodejs-expressjs-2bdd?context=digest#project-structure-overview)Project Structure Overview

Here’s a common folder structure for an Express.js application:  

```
📁
├── 📄 app.js
├── 📁 bin
├── 📁 config
├── 📁 controllers
│   ├── 📄 customer.js
│   ├── 📄 product.js
│   └── ...
├── 📁 middleware
│   ├── 📄 auth.js
│   ├── 📄 logger.js
│   └── ...
├── 📁 models
│   ├── 📄 customer.js
│   ├── 📄 product.js
│   └── ...
├── 📁 routes
│   ├── 📄 api.js
│   ├── 📄 auth.js
│   └── ...
├── 📁 public
│   ├── 📁 css
│   ├── 📁 js
│   ├── 📁 images
│   └── ...
├── 📁 views
│   ├── 📄 index.ejs
│   ├── 📄 product.ejs
│   └── ...
├── 📁 tests
│   ├── 📁 unit
│   ├── 📁 integration
│   ├── 📁 e2e
│   └── ...
├── 📁 utils
│   ├── 📄 validation.js
│   ├── 📄 helpers.js
│   └── ...
└── 📁 node_modules
```

### [](https://dev.to/vyan/how-to-structure-your-backend-code-in-nodejs-expressjs-2bdd?context=digest#explanation-of-each-directory-and-file)Explanation of Each Directory and File

#### [](https://dev.to/vyan/how-to-structure-your-backend-code-in-nodejs-expressjs-2bdd?context=digest#-raw-appjs-endraw-)`app.js`

The `app.js` file is the entry point of your application. It’s where you initialize the Express app, set up middleware, define routes, and start the server. Think of it as the control center of your web application.  

```
<span>const</span> <span>express</span> <span>=</span> <span>require</span><span>(</span><span>'</span><span>express</span><span>'</span><span>);</span>
<span>const</span> <span>app</span> <span>=</span> <span>express</span><span>();</span>
<span>const</span> <span>config</span> <span>=</span> <span>require</span><span>(</span><span>'</span><span>./config</span><span>'</span><span>);</span>
<span>const</span> <span>routes</span> <span>=</span> <span>require</span><span>(</span><span>'</span><span>./routes</span><span>'</span><span>);</span>

<span>// Middleware setup</span>
<span>app</span><span>.</span><span>use</span><span>(</span><span>express</span><span>.</span><span>json</span><span>());</span>

<span>// Routes setup</span>
<span>app</span><span>.</span><span>use</span><span>(</span><span>'</span><span>/api</span><span>'</span><span>,</span> <span>routes</span><span>);</span>

<span>// Start server</span>
<span>const</span> <span>PORT</span> <span>=</span> <span>config</span><span>.</span><span>port</span> <span>||</span> <span>3000</span><span>;</span>
<span>app</span><span>.</span><span>listen</span><span>(</span><span>PORT</span><span>,</span> <span>()</span> <span>=&gt;</span> <span>{</span>
  <span>console</span><span>.</span><span>log</span><span>(</span><span>`Server running on port </span><span>${</span><span>PORT</span><span>}</span><span>`</span><span>);</span>
<span>});</span>

<span>module</span><span>.</span><span>exports</span> <span>=</span> <span>app</span><span>;</span>
```

#### [](https://dev.to/vyan/how-to-structure-your-backend-code-in-nodejs-expressjs-2bdd?context=digest#-raw-bin-endraw-)`bin`

The `bin` directory typically contains scripts for starting your web server. These scripts can be used to set environment variables or manage different environments (e.g., development, production).

**Example: `bin/www`**  

```
<span>#!/usr/bin/env node
</span>
<span>const</span> <span>app</span> <span>=</span> <span>require</span><span>(</span><span>'</span><span>../app</span><span>'</span><span>);</span>
<span>const</span> <span>debug</span> <span>=</span> <span>require</span><span>(</span><span>'</span><span>debug</span><span>'</span><span>)(</span><span>'</span><span>your-app:server</span><span>'</span><span>);</span>
<span>const</span> <span>http</span> <span>=</span> <span>require</span><span>(</span><span>'</span><span>http</span><span>'</span><span>);</span>

<span>const</span> <span>port</span> <span>=</span> <span>normalizePort</span><span>(</span><span>process</span><span>.</span><span>env</span><span>.</span><span>PORT</span> <span>||</span> <span>'</span><span>3000</span><span>'</span><span>);</span>
<span>app</span><span>.</span><span>set</span><span>(</span><span>'</span><span>port</span><span>'</span><span>,</span> <span>port</span><span>);</span>

<span>const</span> <span>server</span> <span>=</span> <span>http</span><span>.</span><span>createServer</span><span>(</span><span>app</span><span>);</span>
<span>server</span><span>.</span><span>listen</span><span>(</span><span>port</span><span>);</span>
<span>server</span><span>.</span><span>on</span><span>(</span><span>'</span><span>error</span><span>'</span><span>,</span> <span>onError</span><span>);</span>
<span>server</span><span>.</span><span>on</span><span>(</span><span>'</span><span>listening</span><span>'</span><span>,</span> <span>onListening</span><span>);</span>

<span>function</span> <span>normalizePort</span><span>(</span><span>val</span><span>)</span> <span>{</span>
  <span>const</span> <span>port</span> <span>=</span> <span>parseInt</span><span>(</span><span>val</span><span>,</span> <span>10</span><span>);</span>
  <span>if </span><span>(</span><span>isNaN</span><span>(</span><span>port</span><span>))</span> <span>return</span> <span>val</span><span>;</span>
  <span>if </span><span>(</span><span>port</span> <span>&gt;=</span> <span>0</span><span>)</span> <span>return</span> <span>port</span><span>;</span>
  <span>return</span> <span>false</span><span>;</span>
<span>}</span>

<span>function</span> <span>onError</span><span>(</span><span>error</span><span>)</span> <span>{</span>
  <span>if </span><span>(</span><span>error</span><span>.</span><span>syscall</span> <span>!==</span> <span>'</span><span>listen</span><span>'</span><span>)</span> <span>throw</span> <span>error</span><span>;</span>
  <span>const</span> <span>bind</span> <span>=</span> <span>typeof</span> <span>port</span> <span>===</span> <span>'</span><span>string</span><span>'</span> <span>?</span> <span>'</span><span>Pipe </span><span>'</span> <span>+</span> <span>port</span> <span>:</span> <span>'</span><span>Port </span><span>'</span> <span>+</span> <span>port</span><span>;</span>
  <span>switch </span><span>(</span><span>error</span><span>.</span><span>code</span><span>)</span> <span>{</span>
    <span>case</span> <span>'</span><span>EACCES</span><span>'</span><span>:</span>
      <span>console</span><span>.</span><span>error</span><span>(</span><span>bind</span> <span>+</span> <span>'</span><span> requires elevated privileges</span><span>'</span><span>);</span>
      <span>process</span><span>.</span><span>exit</span><span>(</span><span>1</span><span>);</span>
      <span>break</span><span>;</span>
    <span>case</span> <span>'</span><span>EADDRINUSE</span><span>'</span><span>:</span>
      <span>console</span><span>.</span><span>error</span><span>(</span><span>bind</span> <span>+</span> <span>'</span><span> is already in use</span><span>'</span><span>);</span>
      <span>process</span><span>.</span><span>exit</span><span>(</span><span>1</span><span>);</span>
      <span>break</span><span>;</span>
    <span>default</span><span>:</span>
      <span>throw</span> <span>error</span><span>;</span>
  <span>}</span>
<span>}</span>

<span>function</span> <span>onListening</span><span>()</span> <span>{</span>
  <span>const</span> <span>addr</span> <span>=</span> <span>server</span><span>.</span><span>address</span><span>();</span>
  <span>const</span> <span>bind</span> <span>=</span> <span>typeof</span> <span>addr</span> <span>===</span> <span>'</span><span>string</span><span>'</span> <span>?</span> <span>'</span><span>pipe </span><span>'</span> <span>+</span> <span>addr</span> <span>:</span> <span>'</span><span>port </span><span>'</span> <span>+</span> <span>addr</span><span>.</span><span>port</span><span>;</span>
  <span>debug</span><span>(</span><span>'</span><span>Listening on </span><span>'</span> <span>+</span> <span>bind</span><span>);</span>
<span>}</span>
```

#### [](https://dev.to/vyan/how-to-structure-your-backend-code-in-nodejs-expressjs-2bdd?context=digest#-raw-config-endraw-)`config`

The `config` directory holds configuration files for your application, such as database connections, server settings, and environment variables.

**Example: `config/index.js`**  

```
<span>module</span><span>.</span><span>exports</span> <span>=</span> <span>{</span>
  <span>port</span><span>:</span> <span>process</span><span>.</span><span>env</span><span>.</span><span>PORT</span> <span>||</span> <span>3000</span><span>,</span>
  <span>db</span><span>:</span> <span>{</span>
    <span>host</span><span>:</span> <span>'</span><span>localhost</span><span>'</span><span>,</span>
    <span>port</span><span>:</span> <span>27017</span><span>,</span>
    <span>name</span><span>:</span> <span>'</span><span>mydatabase</span><span>'</span>
  <span>}</span>
<span>};</span>
```

#### [](https://dev.to/vyan/how-to-structure-your-backend-code-in-nodejs-expressjs-2bdd?context=digest#-raw-controllers-endraw-)`controllers`

Controllers contain the logic for handling incoming requests and generating responses. Each file in the `controllers` directory typically corresponds to a different part of your application (e.g., customers, products).

**Example: `controllers/customer.js`**  

```
<span>const</span> <span>Customer</span> <span>=</span> <span>require</span><span>(</span><span>'</span><span>../models/customer</span><span>'</span><span>);</span>

<span>exports</span><span>.</span><span>getAllCustomers</span> <span>=</span> <span>async </span><span>(</span><span>req</span><span>,</span> <span>res</span><span>)</span> <span>=&gt;</span> <span>{</span>
  <span>try</span> <span>{</span>
    <span>const</span> <span>customers</span> <span>=</span> <span>await</span> <span>Customer</span><span>.</span><span>find</span><span>();</span>
    <span>res</span><span>.</span><span>json</span><span>(</span><span>customers</span><span>);</span>
  <span>}</span> <span>catch </span><span>(</span><span>err</span><span>)</span> <span>{</span>
    <span>res</span><span>.</span><span>status</span><span>(</span><span>500</span><span>).</span><span>json</span><span>({</span> <span>message</span><span>:</span> <span>err</span><span>.</span><span>message</span> <span>});</span>
  <span>}</span>
<span>};</span>
```

#### [](https://dev.to/vyan/how-to-structure-your-backend-code-in-nodejs-expressjs-2bdd?context=digest#-raw-middleware-endraw-)`middleware`

Middleware functions are used to process requests before they reach the controllers. They can handle tasks like authentication, logging, and request validation.

**Example: `middleware/auth.js`**  

```
<span>module</span><span>.</span><span>exports</span> <span>=</span> <span>(</span><span>req</span><span>,</span> <span>res</span><span>,</span> <span>next</span><span>)</span> <span>=&gt;</span> <span>{</span>
  <span>const</span> <span>token</span> <span>=</span> <span>req</span><span>.</span><span>header</span><span>(</span><span>'</span><span>Authorization</span><span>'</span><span>);</span>
  <span>if </span><span>(</span><span>!</span><span>token</span><span>)</span> <span>return</span> <span>res</span><span>.</span><span>status</span><span>(</span><span>401</span><span>).</span><span>json</span><span>({</span> <span>message</span><span>:</span> <span>'</span><span>Access Denied</span><span>'</span> <span>});</span>

  <span>try</span> <span>{</span>
    <span>const</span> <span>verified</span> <span>=</span> <span>jwt</span><span>.</span><span>verify</span><span>(</span><span>token</span><span>,</span> <span>process</span><span>.</span><span>env</span><span>.</span><span>JWT_SECRET</span><span>);</span>
    <span>req</span><span>.</span><span>user</span> <span>=</span> <span>verified</span><span>;</span>
    <span>next</span><span>();</span>
  <span>}</span> <span>catch </span><span>(</span><span>err</span><span>)</span> <span>{</span>
    <span>res</span><span>.</span><span>status</span><span>(</span><span>400</span><span>).</span><span>json</span><span>({</span> <span>message</span><span>:</span> <span>'</span><span>Invalid Token</span><span>'</span> <span>});</span>
  <span>}</span>
<span>};</span>
```

#### [](https://dev.to/vyan/how-to-structure-your-backend-code-in-nodejs-expressjs-2bdd?context=digest#-raw-models-endraw-)`models`

Models define the structure of your data and handle interactions with the database. Each model file typically corresponds to a different data entity (e.g., Customer, Product).

**Example: `models/customer.js`**  

```
<span>const</span> <span>mongoose</span> <span>=</span> <span>require</span><span>(</span><span>'</span><span>mongoose</span><span>'</span><span>);</span>

<span>const</span> <span>customerSchema</span> <span>=</span> <span>new</span> <span>mongoose</span><span>.</span><span>Schema</span><span>({</span>
  <span>name</span><span>:</span> <span>{</span>
    <span>type</span><span>:</span> <span>String</span><span>,</span>
    <span>required</span><span>:</span> <span>true</span>
  <span>},</span>
  <span>email</span><span>:</span> <span>{</span>
    <span>type</span><span>:</span> <span>String</span><span>,</span>
    <span>required</span><span>:</span> <span>true</span><span>,</span>
    <span>unique</span><span>:</span> <span>true</span>
  <span>},</span>
  <span>createdAt</span><span>:</span> <span>{</span>
    <span>type</span><span>:</span> <span>Date</span><span>,</span>
    <span>default</span><span>:</span> <span>Date</span><span>.</span><span>now</span>
  <span>}</span>
<span>});</span>

<span>module</span><span>.</span><span>exports</span> <span>=</span> <span>mongoose</span><span>.</span><span>model</span><span>(</span><span>'</span><span>Customer</span><span>'</span><span>,</span> <span>customerSchema</span><span>);</span>
```

#### [](https://dev.to/vyan/how-to-structure-your-backend-code-in-nodejs-expressjs-2bdd?context=digest#-raw-routes-endraw-)`routes`

Routes define the paths to different parts of your application and map them to the appropriate controllers.

**Example: `routes/api.js`**  

```
<span>const</span> <span>express</span> <span>=</span> <span>require</span><span>(</span><span>'</span><span>express</span><span>'</span><span>);</span>
<span>const</span> <span>router</span> <span>=</span> <span>express</span><span>.</span><span>Router</span><span>();</span>
<span>const</span> <span>customerController</span> <span>=</span> <span>require</span><span>(</span><span>'</span><span>../controllers/customer</span><span>'</span><span>);</span>

<span>router</span><span>.</span><span>get</span><span>(</span><span>'</span><span>/customers</span><span>'</span><span>,</span> <span>customerController</span><span>.</span><span>getAllCustomers</span><span>);</span>

<span>module</span><span>.</span><span>exports</span> <span>=</span> <span>router</span><span>;</span>
```

#### [](https://dev.to/vyan/how-to-structure-your-backend-code-in-nodejs-expressjs-2bdd?context=digest#-raw-public-endraw-)`public`

The `public` directory contains static files like CSS, JavaScript, and images that are served directly to the client.

**Example: Directory Structure**  

```
public/
├── css/
├── js/
├── images/
```

#### [](https://dev.to/vyan/how-to-structure-your-backend-code-in-nodejs-expressjs-2bdd?context=digest#-raw-views-endraw-)`views`

Views are templates that render the HTML for the client. Using a templating engine like EJS, Pug, or Handlebars, you can generate dynamic HTML.

**Example: `views/index.ejs`**  

```
<span>&lt;!DOCTYPE html&gt;</span>
<span>&lt;html&gt;</span>
<span>&lt;head&gt;</span>
  <span>&lt;title&gt;</span>My App<span>&lt;/title&gt;</span>
  <span>&lt;link</span> <span>rel=</span><span>"stylesheet"</span> <span>href=</span><span>"/css/styles.css"</span><span>&gt;</span>
<span>&lt;/head&gt;</span>
<span>&lt;body&gt;</span>
  <span>&lt;h1&gt;</span>Welcome to My App<span>&lt;/h1&gt;</span>
  <span>&lt;div</span> <span>id=</span><span>"content"</span><span>&gt;</span>
    <span>&lt;</span><span>%</span><span>-</span> <span>content</span> <span>%</span><span>&gt;</span>
  <span>&lt;/div&gt;</span>
<span>&lt;/body&gt;</span>
<span>&lt;/html&gt;</span>
```

#### [](https://dev.to/vyan/how-to-structure-your-backend-code-in-nodejs-expressjs-2bdd?context=digest#-raw-tests-endraw-)`tests`

The `tests` directory contains test files to ensure your application works correctly. Tests are often organized into unit tests, integration tests, and end-to-end (e2e) tests.

**Example: Directory Structure**  

```
tests/
├── unit/
├── integration/
├── e2e/
```

#### [](https://dev.to/vyan/how-to-structure-your-backend-code-in-nodejs-expressjs-2bdd?context=digest#-raw-utils-endraw-)`utils`

Utility functions and helper modules are stored in the `utils` directory. These functions perform common tasks like validation and formatting that are used throughout the application.

**Example: `utils/validation.js`**  

```
<span>exports</span><span>.</span><span>isEmailValid</span> <span>=</span> <span>(</span><span>email</span><span>)</span> <span>=&gt;</span> <span>{</span>
  <span>const</span> <span>re</span> <span>=</span> <span>/^</span><span>[^\s</span><span>@</span><span>]</span><span>+@</span><span>[^\s</span><span>@</span><span>]</span><span>+</span><span>\.[^\s</span><span>@</span><span>]</span><span>+$/</span><span>;</span>
  <span>return</span> <span>re</span><span>.</span><span>test</span><span>(</span><span>String</span><span>(</span><span>email</span><span>).</span><span>toLowerCase</span><span>());</span>
<span>};</span>
```

#### [](https://dev.to/vyan/how-to-structure-your-backend-code-in-nodejs-expressjs-2bdd?context=digest#-raw-nodemodules-endraw-)`node_modules`

The `node_modules` directory contains all the dependencies your project needs. This directory is managed by npm (or yarn) and includes packages installed from the npm registry.

### [](https://dev.to/vyan/how-to-structure-your-backend-code-in-nodejs-expressjs-2bdd?context=digest#conclusion)Conclusion

A well-structured Node.js application using Express.js enhances maintainability, scalability, and collaboration. Each directory and file in the structure serves a specific purpose, from handling configuration and defining routes to managing middleware and rendering views. By organizing your codebase effectively, you can build robust and scalable applications with ease.
