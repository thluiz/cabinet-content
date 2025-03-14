---
created: 2024-09-10T11:22:39 (UTC -03:00)
tags: [python,fastapi,webdev,html,software,coding,development,engineering,inclusive,community]
source: https://dev.to/jagroop2001/building-a-simple-blog-app-using-fastapi-html-css-and-json-1dc?context=digest
author: 
---

# Building a Simple Blog App Using FastAPI, HTML, CSS, and JSON - DEV Community

> ## Excerpt
> In this tutorial, we will create a basic blog app using FastAPI for the backend, HTML and CSS for the...

---
In this tutorial, we will create a basic blog app using **FastAPI** for the backend, **HTML** and **CSS** for the frontend, and a **JSON** file for performing basic CRUD (Create, Read, Update, Delete) operations.  
FastAPI is a modern web framework for building APIs with Python, which is known for its simplicity, speed, and **built-in support for asynchronous operations.**

**Below Implementation would looks like this :**

[![Blog List Page](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F7tm2wi5l1le64behzhcy.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F7tm2wi5l1le64behzhcy.png)

[![Blog Detail Page](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fhra79x9ngfoo53irzbgg.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fhra79x9ngfoo53irzbgg.png)

[![Create New Blog Page](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F6srwvm8fhrb6xmovu0eh.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F6srwvm8fhrb6xmovu0eh.png)

## [](https://dev.to/jagroop2001/building-a-simple-blog-app-using-fastapi-html-css-and-json-1dc?context=digest#prerequisites)**Prerequisites**

Before getting started, ensure you have the following installed:

-   Python 3.7+
-   FastAPI
-   Uvicorn (for running FastAPI applications)

To install FastAPI and Uvicorn, you can use pip:  

```
pip install fastapi uvicorn python-multipart
```

## [](https://dev.to/jagroop2001/building-a-simple-blog-app-using-fastapi-html-css-and-json-1dc?context=digest#project-structure)**Project Structure**

Here's how the project will be structured:  

```
/blog_app
    ├── static
    │   └── style.css
    ├── templates
    │   ├── index.html
    │   ├── post.html
    │   ├── create_post.html
    ├── blog.json
    ├── main.py

```

## [](https://dev.to/jagroop2001/building-a-simple-blog-app-using-fastapi-html-css-and-json-1dc?context=digest#step-1-setting-up-fastapi)**Step 1: Setting Up FastAPI**

Create a `main.py` file which will contain the FastAPI application.  

```
from fastapi import FastAPI, Request, Form
from fastapi.responses import HTMLResponse, RedirectResponse
from fastapi.staticfiles import StaticFiles
from fastapi.templating import Jinja2Templates
import json
import os

app = FastAPI()

app.mount("/static", StaticFiles(directory="static"), name="static")

templates = Jinja2Templates(directory="templates")

# Load or initialize blog data
BLOG_FILE = "blog.json"

if not os.path.exists(BLOG_FILE):
    with open(BLOG_FILE, "w") as f:
        json.dump([], f)


def read_blog_data():
    with open(BLOG_FILE, "r") as f:
        return json.load(f)


def write_blog_data(data):
    with open(BLOG_FILE, "w") as f:
        json.dump(data, f)


@app.get("/", response_class=HTMLResponse)
async def home(request: Request):
    blogs = read_blog_data()
    return templates.TemplateResponse("index.html", {"request": request, "blogs": blogs})


@app.get("/post/{post_id}", response_class=HTMLResponse)
async def read_post(request: Request, post_id: int):
    blogs = read_blog_data()
    post = blogs[post_id] if 0 &lt;= post_id &lt; len(blogs) else None
    return templates.TemplateResponse("post.html", {"request": request, "post": post})


@app.get("/create_post", response_class=HTMLResponse)
async def create_post_form(request: Request):
    return templates.TemplateResponse("create_post.html", {"request": request})


@app.post("/create_post")
async def create_post(title: str = Form(...), content: str = Form(...)):
    blogs = read_blog_data()
    blogs.append({"title": title, "content": content})
    write_blog_data(blogs)
    return RedirectResponse("/", status_code=303)


@app.post("/delete_post/{post_id}")
async def delete_post(post_id: int):
    blogs = read_blog_data()
    if 0 &lt;= post_id &lt; len(blogs):
        blogs.pop(post_id)
        write_blog_data(blogs)
    return RedirectResponse("/", status_code=303)

```

## [](https://dev.to/jagroop2001/building-a-simple-blog-app-using-fastapi-html-css-and-json-1dc?context=digest#step-2-setting-up-html-and-css)**Step 2: Setting Up HTML and CSS**

In the templates folder, create the following HTML files:

`index.html`  
This file will list all the blog posts.  

```
&lt;!DOCTYPE html&gt;
&lt;html lang="en"&gt;
&lt;head&gt;
    &lt;meta charset="UTF-8"&gt;
    &lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt;
    &lt;title&gt;Blog App&lt;/title&gt;
    &lt;link rel="stylesheet" href="/static/style.css"&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h1&gt;Blog Posts&lt;/h1&gt;
    &lt;a href="/create_post"&gt;Create New Post&lt;/a&gt;
    &lt;div&gt;
        {% for post in blogs %}
        &lt;div class="post"&gt;
            &lt;h2&gt;&#123;&#123; post.title &#124;&#124;&lt;/h2&gt;
            &lt;p&gt;&#123;&#123; post.content[:100] &#124;&#124;...&lt;/p&gt;
            &lt;a href="/post/&#123;&#123; loop.index0 &#124;&#124;"&gt;Read more&lt;/a&gt;
            &lt;form method="post" action="/delete_post/&#123;&#123; loop.index0 &#124;&#124;"&gt;
                &lt;button type="submit"&gt;Delete&lt;/button&gt;
            &lt;/form&gt;
        &lt;/div&gt;
        {% endfor %}
    &lt;/div&gt;
&lt;/body&gt;
&lt;/html&gt;

```

`post.html`  
This file will display the full content of a blog post.  

```
&lt;!DOCTYPE html&gt;
&lt;html lang="en"&gt;
&lt;head&gt;
    &lt;meta charset="UTF-8"&gt;
    &lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt;
    &lt;title&gt;&#123;&#123; post.title &#124;&#124;&lt;/title&gt;
    &lt;link rel="stylesheet" href="/static/style.css"&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h1&gt;&#123;&#123; post.title &#124;&#124;&lt;/h1&gt;
    &lt;p&gt;&#123;&#123; post.content &#124;&#124;&lt;/p&gt;
    &lt;a href="/"&gt;Back to Home&lt;/a&gt;
&lt;/body&gt;
&lt;/html&gt;

```

`create_post.html`  
This file will contain the form for creating a new post.  

```
&lt;!DOCTYPE html&gt;
&lt;html lang="en"&gt;
&lt;head&gt;
    &lt;meta charset="UTF-8"&gt;
    &lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt;
    &lt;title&gt;Create a New Post&lt;/title&gt;
    &lt;link rel="stylesheet" href="/static/style.css"&gt;
&lt;/head&gt;
&lt;body&gt;
    &lt;h1&gt;Create a New Post&lt;/h1&gt;
    &lt;form method="post" action="/create_post"&gt;
        &lt;label for="title"&gt;Title&lt;/label&gt;
        &lt;input type="text" name="title" id="title" required&gt;
        &lt;label for="content"&gt;Content&lt;/label&gt;
        &lt;textarea name="content" id="content" required&gt;&lt;/textarea&gt;
        &lt;button type="submit"&gt;Create&lt;/button&gt;
    &lt;/form&gt;
    &lt;a href="/"&gt;Back to Home&lt;/a&gt;
&lt;/body&gt;
&lt;/html&gt;

```

## [](https://dev.to/jagroop2001/building-a-simple-blog-app-using-fastapi-html-css-and-json-1dc?context=digest#step-3-styling-with-css)**Step 3: Styling with CSS**

In the static folder, create a `style.css` file to add some basic styling.  

```
body {
    font-family: Arial, sans-serif;
    padding: 20px;
    background-color: #f0f0f0;
}

h1 {
    color: #333;
}

a {
    text-decoration: none;
    color: #0066cc;
}

.post {
    background-color: #fff;
    padding: 10px;
    margin-bottom: 15px;
    border-radius: 5px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

button {
    background-color: #ff4d4d;
    border: none;
    padding: 5px 10px;
    color: white;
    border-radius: 3px;
    cursor: pointer;
}

button:hover {
    background-color: #ff1a1a;
}

input, textarea {
    width: 100%;
    padding: 8px;
    margin-bottom: 10px;
}

```

## [](https://dev.to/jagroop2001/building-a-simple-blog-app-using-fastapi-html-css-and-json-1dc?context=digest#step-4-running-the-application)**Step 4: Running the Application**

Now that everything is set up, run the FastAPI application using Uvicorn.  

```
uvicorn main:app --reload
```

Visit `http://127.0.0.1:8000` in your browser, and you should see the blog homepage.

As an assignment, you can use a Database 🗄️ instead of just JSON to create a full-stack web app.  
With a database, you can add more features 🌟, improve performance 🚀, and enhance the overall UI/UX 🎨 for a richer user experience.

That's all for this blog! Stay tuned for more updates and keep building amazing apps! 💻✨
