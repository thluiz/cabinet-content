---
created: 2024-08-21T10:01:08 (UTC -03:00)
tags: []
source: https://notation.so/?ref=producthunt
author: 
---

# Notation

Notation allows you to write markdown and automatically publish it Notion.

Once it's in Notion, it can be an internal thing, or you can ship it as a public website.

You also get all of Notion's AI, search, and formatting for free.

> ## Excerpt
> Once the binary is there, you can edit the config at ~/.notation/Notation.toml (more detail below).

---
## Hi, this is Notation 👋

![image](https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/a36c667d-d0ad-4902-b37b-c4e8cca1479b/demo/w=2048,quality=90,fit=scale-down)

## Install to ~/.notation

```shell
curl -sSL https://github.com/kristian1108/notation/raw/master/install.sh | bash
```

Once the binary is there, you can edit the config at `~/.notation/Notation.toml` (more detail below).

## Design Philosophy

### 1 — Documentation should not be managed separately from the repo.

Documentation describes code. It should live with the code, have dinner with the code, go to bed with the code, and get up in the morning with the code.

### 2 — I should not have to login to some separate service to update docs after I merge and deploy code.

That's hard to keep track of. Engineers cannot be trusted to workout outside the repo. Online documentation tools give me JIRA vibes. No thank youuu.

### 3 — The only frontend framework I want to think about is Markdown.

Docusaurus is an interesting product but it's too complicated. I do not want to wrestle with React. I do not want to deploy a website.

### 4 — Some people are not engineers. They should be able to read my docs.

Users, product mommies, etc.

### 5 — Docs should be highly searchable.

Good search is a hard problem so better let someone else worry about that.

### 6 — Documentation should be subject to the same review & CI process as code.

See point 2.

## Solution

Notation allows you to write markdown and automatically publish it Notion.

Once it's in Notion, it can be an internal thing, or you can ship it as a public website.

You also get all of Notion's AI, search, and formatting for free.

## Example

The website you're reading right now is managed by Notation. Here's the [source Markdown](https://github.com/kristian1108/notation/blob/master/docs/intro.md).

## Usage

1.  Write your documentation in markdown.
2.  Create a Notion page to host your documentation.
3.  Grab an API key from Notion (help below)
4.  Throw that API key in a `~/.notation/Notation.toml` file (detail below)
5.  Run `notation ship --src </path/to/you/docs>`
6.  See your documentation in Notion. Use built-in AI search, nice formatting, table of contents, etc

## Deployment Options

1.  Keep your documentation internal to your own Notion workspace for your team, OR
2.  Ship it as a Notion page [like this](https://private-marmot-67c.notion.site/Notation-), OR
3.  Deploy it as a full-on website with [super.so](https://super.so/), for example: [notation.so](https://notation.so/).

## A Little Sugar

There are a few nice extras that Notation provides:

### 1 — intro.md

If you want to ship content to the parent page directly, you can put it in an `intro.md` file in the directory. Instead of creating a subpage for this content, it will display directly on the parent.

For example, consider this file structure:

```
docs/
  intro.md
  getting_started.md
  api/
    endpoints.md
```

This will get rendered in Notion as:

```
Parent Page
  ... whatever content is in intro.md ...  
  - Getting Started (Subpage)
  - API (Subpage)
    - Endpoints (Subpage)
```

### 2 — arguments

Notation supports these arguments passed as CLI flags at the top of your Markdown file:

1.  `title` - the title of the page in Notion
2.  `emoji` - the page icon in Notion

For example:

```
--title "Getting Started" --emoji 🚀

# My Title of my markdown file
My content that will be rendered on a page titled "Get Started" with the rocket emoji.
```

## FAQ

### What markdown features do you support?

-   headers
-   paragraphs
-   code blocks
-   lists (ordered, unordered)
-   tables
-   links
-   images (although not local, you need to host yourself and put the link)
-   relative page links (to other pages in the same repo, which will turn into Notion page links)
-   arbitrary directory structure (will turn into subpages, subsubpages, etc.)

### How do I configure Notation?

Create a `Notation.toml` file alongside the notation binary (usually in `~/.notation/`).

```toml
# ~/.notation/Notation.toml [notion] secret = "" # this is the title of the page that will host your new documentation parent_page = ""
```

### How do I set all this up?

First, you need to have a notion account. Sign up here: [Notion](https://www.notion.so/)

Next, you need to create a page to host your documentation.

![image](https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/47409f7a-132a-4817-a751-999e5cd8af81/add_a_page/w=2048,quality=90,fit=scale-down)

Now, give that page a name:

![image](https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/c33c699a-21d7-4096-bf48-75885e5a9c43/notation_parent_name/w=2048,quality=90,fit=scale-down)

In your `Notation.toml` file, just write down this name (make sure it's a unique name within your space):

```toml
# Notation.toml [notion] secret = "" parent_page = "Your Notation Parent" # <----- this name
```

Now you need to create an integration in Notion.

Go to `Settings & members` in the top right of your Notion home.

And then click `Connections` --> `Develop or manage integrations`.

![image](https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/aee90748-db10-4ab5-a652-0bee1e246f85/add_integration/w=2048,quality=90,fit=scale-down)

Now you want to create a new integration.

![image](https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/71433fb9-428a-46fe-b153-f825e46d51bb/new_integration/w=2048,quality=90,fit=scale-down)

Give it a name, assign it to one of your workspaces, and select `internal` as the type of integration.

![image](https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/f097b953-effd-441c-84cd-fdc070d614de/configure_integration/w=2048,quality=90,fit=scale-down)

Now just grab the secret:

![image](https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/e062daba-3188-4f19-94be-7ba34d78686f/grab_secret/w=2048,quality=90,fit=scale-down)

And throw it in your `Notation.toml`!

```toml
# Notation.toml [notion] secret = "your_new_integration_secret" # <----- right here parent_page = "Your Notation Parent"
```

Last thing, you need to connect your Notion page to this integration.

Back on your parent Notion page, click the three dots in the top right corner, and then go down to `Connections`, find your new Notion integration, and click it.

![image](https://images.spr.so/cdn-cgi/imagedelivery/j42No7y-dcokJuNgXeA0ig/5b18bd95-7464-4a39-af75-e1d7ef502828/connect_to_page/w=2048,quality=90,fit=scale-down)

That's it! Enjoy!

## Help

If you have any questions, feel free to create an issue in the [Github](https://github.com/kristian1108/notation). I'll be actively monitoring. Thanks :)
