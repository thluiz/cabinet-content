---
created: 2024-04-11T11:33:32 (UTC -03:00)
tags: [fsharp,templates,html,dotnet,software,coding,development,engineering,inclusive,community]
source: https://dev.to/tunaxor/generating-html-in-f-4h5b
author: Angel Daniel Munoz Gonzalez
---

# Generating HTML in F# - DEV Community

> ## Excerpt
> Simple things in FSharp   Hello there, this is the 4th entry in Simple Things F#.  Today...

---
## [](https://dev.to/tunaxor/generating-html-in-f-4h5b#simple-things-in-fsharp)Simple things in FSharp

Hello there, this is the 4th entry in Simple Things F#.

Today we'll be talking about producing HTML strings (useful to create reports, pdfs and things like those). Producing HTML in F# is quite simple to be honest, there are a bunch of libraries that allow that we'll be looking at some I've used at some point

-   [Giraffe.ViewEngine](https://giraffe.wiki/view-engine)
-   [Feliz.ViewEngine](https://github.com/dbrattli/Feliz.ViewEngine)
-   [Scriban](https://github.com/scriban/scriban)

The first two are F# DSL's to build HTML, the last one is an HTML scripting language for .NET (there's also [Giraffe.Razor](https://github.com/giraffe-fsharp/Giraffe.Razor) but I'll skip that one for today)

### [](https://dev.to/tunaxor/generating-html-in-f-4h5b#giraffe)Giraffe

When it comes to HTML DSL's this is the most "traditional" one since it's not a flavor liked my many these days but that doesn't take away it's usefulness. Giraffe View Engine uses XmlNode as it's building block so you can produce XML as well with Giraffe View Engine! (points for that)

the structure of a node function in Giraffe is as follows

-   `tagName [(* attribute list *)] [(* node list *)]`

for example creating `<div class="my-class"></div>` would be something like this

-   `div [ _class "my-class" ] []`

> attributes are prefixed with an underscore `_` to prevent clashing with reserved words in F#

Let's take a look at a simple page with a header  

```
<span>#</span><span>r</span> <span>"nuget: Giraffe.ViewEngine"</span>

<span>open</span> <span>Giraffe</span><span>.</span><span>ViewEngine</span>


<span>let</span> <span>view</span> <span>=</span>
    <span>html</span> <span>[]</span> <span>[</span>
        <span>head</span> <span>[]</span> <span>[</span> <span>title</span> <span>[]</span> <span>[</span> <span>str</span> <span>"Giraffe"</span> <span>]</span> <span>]</span>
        <span>body</span> <span>[]</span> <span>[</span> <span>header</span> <span>[]</span> <span>[</span> <span>str</span> <span>"Giraffe"</span> <span>]</span> <span>]</span>
    <span>]</span>
<span>let</span> <span>document</span> <span>=</span> <span>RenderView</span><span>.</span><span>AsString</span><span>.</span><span>htmlDocument</span> <span>view</span>

<span>printfn</span> <span>"%s"</span> <span>document</span>
```

> To Run this, copy this content into a file named `script.fsx` (or whatever name you prefer) and type:
> 
> -   `dotnet fsi run script.fsx`

we assign the result of the html function to view, then we just render the document as a string, we can then follow up and create a file with the IO API's we saw earlier in this series.

> If you're using Saturn/Giraffe as your web framework you don't need to do the manual render they provide helpers that take care of this (as you'll see on the next entry in this series)

you can also create functions to pre-define aspects of your views and override values if you deem it necessary  

```
<span>#</span><span>r</span> <span>"nuget: Giraffe.ViewEngine"</span>

<span>open</span> <span>Giraffe</span><span>.</span><span>ViewEngine</span>


<span>let</span> <span>card</span> <span>attributes</span> <span>=</span> 
    <span>article</span> <span>[</span> <span>yield</span><span>!</span> <span>attributes</span><span>;</span> <span>_</span><span>class</span> <span>"card is-green"</span><span>]</span>

<span>let</span> <span>cardFooter</span> <span>attributes</span> <span>=</span>
    <span>footer</span> <span>[</span> <span>yield</span><span>!</span> <span>attributes</span><span>;</span> <span>_</span><span>class</span> <span>"card-footer is-rounded"</span><span>]</span>

<span>let</span> <span>cardHeader</span> <span>attributes</span> <span>=</span>
    <span>header</span> <span>[</span> <span>yield</span><span>!</span> <span>attributes</span><span>;</span> <span>_</span><span>class</span> <span>"card-header no-icons"</span><span>]</span>

<span>let</span> <span>mySection</span> <span>=</span> 
    <span>div</span> <span>[]</span> <span>[</span>
        <span>card</span> <span>[</span> <span>_</span><span>id</span> <span>"my-card"</span> <span>]</span> <span>[</span>
            <span>cardHeader</span> <span>[]</span> <span>[</span>
                <span>h1</span> <span>[]</span> <span>[</span> <span>str</span> <span>"This is my custom card"</span><span>]</span>
                <span>img</span> <span>[</span> <span>_</span><span>src</span> <span>"https://some-image.com"</span><span>;</span> <span>_</span><span>class</span> <span>"card-header-image"</span> <span>]</span>
            <span>]</span>

            <span>p</span> <span>[]</span> <span>[</span> <span>str</span> <span>"this is the body of the card"</span> <span>]</span>

            <span>cardFooter</span> <span>[</span> <span>_</span><span>data</span> <span>"my-attr"</span> <span>"extra attributes"</span> <span>]</span> <span>[</span>
                <span>p</span> <span>[]</span> <span>[</span> <span>str</span> <span>"This is my footer"</span><span>]</span>
            <span>]</span>
        <span>]</span>
    <span>]</span>

<span>// note that we used htmlNode this time instead of htmlDocument</span>
<span>let</span> <span>document</span> <span>=</span> <span>RenderView</span><span>.</span><span>AsString</span><span>.</span><span>htmlNode</span> <span>mySection</span>

<span>printfn</span> <span>"%s"</span> <span>document</span>
```

> To Run this, copy this content into a file named `script.fsx` (or whatever name you prefer) and type:
> 
> -   `dotnet fsi run script.fsx`

so creating new "tags" is basically just creating a new function that accepts attributes and contents.

### [](https://dev.to/tunaxor/generating-html-in-f-4h5b#feliz)Feliz

The original Feliz DSL was made by [@zaidajaj](https://dev.to/zaidajaj) (he produces AMAZING OSS work for F# you should check his github profile) to use in [Fable](https://fable.io/docs/) applications which are built most of the time on top of React.js so if you take a look at the following content you'll see that `view` is a type of `ReactElement` but don't worry it's just a type, there's no javascript running here it's just a way to make use of a really good DSL (which reduces typing and improves readability) to produce HTML as well in the backend.

> I'm not ultra well versed in composition techniques with Feliz so take the following examples with a grain of salt (you can also check the Elmish book [https://zaid-ajaj.github.io/the-elmish-book/](https://zaid-ajaj.github.io/the-elmish-book/)) for more information about Fable and Feliz  

```
<span>#</span><span>r</span> <span>"nuget: Feliz.ViewEngine"</span>

<span>open</span> <span>Feliz</span><span>.</span><span>ViewEngine</span>

<span>let</span> <span>view</span> <span>=</span> 
    <span>Html</span><span>.</span><span>html</span> <span>[</span>
        <span>Html</span><span>.</span><span>head</span> <span>[</span> <span>Html</span><span>.</span><span>title</span> <span>"Feliz"</span> <span>]</span>
        <span>Html</span><span>.</span><span>body</span> <span>[</span>
            <span>Html</span><span>.</span><span>header</span> <span>[</span> <span>prop</span><span>.</span><span>text</span> <span>"Feliz"</span> <span>]</span>
        <span>]</span>
    <span>]</span>

<span>let</span> <span>document</span> <span>=</span> <span>Render</span><span>.</span><span>htmlDocument</span> <span>view</span>

<span>printfn</span> <span>"%s"</span> <span>document</span>
```

> To Run this, copy this content into a file named `script.fsx` (or whatever name you prefer) and type:
> 
> -   `dotnet fsi run script.fsx`

when you open the `Feliz.ViewEngine` namespace you have access to the `Html` and `prop` types these have all the tags and attributes you may need to construct HTML content. if you are feeling tired of writing `prop` and `Html` you can even `open type` those classes and access all of their static methods  

```
<span>#</span><span>r</span> <span>"nuget: Feliz.ViewEngine"</span>

<span>open</span> <span>Feliz</span><span>.</span><span>ViewEngine</span>
<span>open</span> <span>type</span> <span>Html</span>
<span>open</span> <span>type</span> <span>prop</span>

<span>let</span> <span>view</span> <span>=</span> 
    <span>html</span> <span>[</span>
        <span>head</span> <span>[</span> <span>title</span> <span>"Feliz"</span> <span>]</span>
        <span>body</span> <span>[</span>
            <span>header</span> <span>[</span> <span>text</span> <span>"Feliz"</span> <span>]</span>
        <span>]</span>
    <span>]</span>

<span>let</span> <span>document</span> <span>=</span> <span>Render</span><span>.</span><span>htmlDocument</span> <span>view</span>

<span>printfn</span> <span>"%s"</span> <span>document</span>
```

> To Run this, copy this content into a file named `script.fsx` (or whatever name you prefer) and type:
> 
> -   `dotnet fsi run script.fsx`

that will save you a few keystrokes as well! you might run into some naming clashes but you can also just use qualified `prop` and `Html` if you need it.

let's continue with the card component to see how you can compose different view elements. There are a few differences when you use the Feliz DSL, primarily these are because how the React API is defined, you can't mix properties with children, so you have to pass `prop.children` to be able to define child elements for your components  

```
<span>#</span><span>r</span> <span>"nuget: Feliz.ViewEngine"</span>

<span>open</span> <span>Feliz</span><span>.</span><span>ViewEngine</span>
<span>open</span> <span>type</span> <span>Html</span>
<span>open</span> <span>type</span> <span>prop</span>

<span>// custom card, but you can't customize it's classes only the children elements</span>
<span>let</span> <span>card</span> <span>(</span><span>content</span><span>:</span> <span>ReactElement</span> <span>seq</span><span>)</span> <span>=</span> 
    <span>article</span> <span>[</span>
        <span>className</span> <span>"card is-green"</span>
        <span>children</span> <span>content</span>
    <span>]</span>
<span>// this card footer will allou you to define any property  of the footer element</span>
<span>let</span> <span>cardFooter</span> <span>content</span> <span>=</span>
    <span>footer</span> <span>[</span>
        <span>className</span> <span>"card-footer is-rounded"</span>
        <span>yield</span><span>!</span> <span>content</span>
    <span>]</span>

<span>let</span> <span>slotedHeader</span> <span>(</span><span>content</span><span>:</span> <span>ReactElement</span> <span>seq</span><span>)</span> <span>=</span> 
    <span>header</span> <span>[</span>
        <span>className</span> <span>"card-header"</span>
        <span>// pass the contents directly to the children property</span>
        <span>children</span> <span>content</span>
    <span>]</span>

<span>let</span> <span>customizableHeader</span> <span>content</span> <span>=</span> 
    <span>header</span> <span>[</span>
        <span>className</span> <span>"card-header"</span>
        <span>// allow any property to be set</span>
        <span>yield</span><span>!</span> <span>content</span>
    <span>]</span>

<span>let</span> <span>card1</span> <span>=</span> 
    <span>div</span> <span>[</span>
        <span>card</span> <span>[</span>
            <span>// our slottedHeader only allows to pass children not props</span>
            <span>slotedHeader</span> <span>[</span>
                <span>h1</span> <span>[</span> <span>text</span> <span>"This is my custom card"</span><span>]</span>
                <span>// className "" &lt;- can't do this</span>
            <span>]</span>
            <span>p</span> <span>[</span> <span>text</span> <span>"this is the body of the card"</span> <span>]</span>
            <span>cardFooter</span> <span>[</span>
                <span>custom</span><span>(</span><span>"data-my-attr"</span><span>,</span> <span>"extra attributes"</span><span>)</span>
                <span>children</span> <span>(</span><span>p</span> <span>[</span><span>text</span> <span>"This is my footer"</span><span>])</span>
            <span>]</span>
        <span>]</span>
    <span>]</span>

<span>let</span> <span>card2</span> <span>=</span> 
    <span>div</span> <span>[</span>
        <span>card</span> <span>[</span>
            <span>/// our customizable header allows us</span>
            <span>/// to pass properties as well as children elements</span>
            <span>customizableHeader</span> <span>[</span>
                <span>children</span> <span>(</span><span>h1</span> <span>[</span> <span>text</span> <span>"This is my custom card"</span><span>])</span>
                <span>className</span> <span>"custom class"</span> 
            <span>]</span>
            <span>p</span> <span>[</span> <span>text</span> <span>"this is the body of the card"</span> <span>]</span>
            <span>cardFooter</span> <span>[</span>
                <span>custom</span><span>(</span><span>"data-my-attr"</span><span>,</span> <span>"extra attributes"</span><span>)</span>
                <span>children</span> <span>(</span><span>p</span> <span>[</span><span>text</span> <span>"This is my footer"</span><span>])</span>
            <span>]</span>
        <span>]</span>
    <span>]</span>

<span>let</span> <span>r1</span> <span>=</span> <span>Render</span><span>.</span><span>htmlView</span> <span>card1</span>
<span>let</span> <span>r2</span> <span>=</span> <span>Render</span><span>.</span><span>htmlView</span> <span>card2</span>

<span>printfn</span> <span>"%s</span><span>\n\n</span><span>%s"</span> <span>r1</span> <span>r2</span>
```

> To Run this, copy this content into a file named `script.fsx` (or whatever name you prefer) and type:
> 
> -   `dotnet fsi run script.fsx`

But there's a thing that will happen here, unlike Giraffe.ViewEngine Feliz doesn't strip existing props so our header will end up like this  

```
<span>&lt;header</span> <span>class=</span><span>"card-header"</span> <span>class=</span><span>"custom class"</span><span>&gt;</span>
    <span>&lt;h1&gt;</span>This is my custom card<span>&lt;/h1&gt;</span>
<span>&lt;/header&gt;</span>
```

In HTML the last defined property always wins, so keep in mind that depending on your intent you might need properties or children ant that might be the deciding factor between using one way or the other.

### [](https://dev.to/tunaxor/generating-html-in-f-4h5b#scriban)Scriban

If you like me can't simply just leave HTML because of _reasons_ there are also Text based alternatives like [Scriban](https://github.com/scriban/scriban) which allow you to just write an html file and just fill it with data  

```
<span>#</span><span>r</span> <span>"nuget: Scriban"</span>

<span>open</span> <span>Scriban</span>

<span>type</span> <span>Product</span> <span>=</span> <span>{</span> <span>name</span><span>:</span> <span>string</span><span>;</span> <span>price</span><span>:</span> <span>float</span><span>;</span> <span>description</span><span>:</span> <span>string</span> <span>}</span>

<span>let</span> <span>renderProducts</span> <span>products</span> <span>=</span> 
    <span>let</span> <span>html</span> <span>=</span> 
        <span>"""
        &lt;ul id='products'&gt;
        &#123;&#123; for product in products &#124;&#124;
          &lt;li&gt;
            &lt;h2&gt;&#123;&#123; product.name &#124;&#124;&lt;/h2&gt;
                 Price: &#123;&#123; product.price &#124;&#124;
                 &#123;&#123; product.description | string.truncate 15 &#124;&#124;
          &lt;/li&gt;
        &#123;&#123; end &#124;&#124;
        &lt;/ul&gt;
        """</span>
    <span>let</span> <span>result</span> <span>=</span> <span>Template</span><span>.</span><span>Parse</span><span>(</span><span>html</span><span>)</span>
    <span>result</span><span>.</span><span>Render</span><span>({|</span> <span>products</span> <span>=</span> <span>products</span> <span>|})</span>

<span>let</span> <span>result</span> <span>=</span>
    <span>renderProducts</span> <span>[</span>
        <span>{</span> <span>name</span> <span>=</span> <span>"Shoes"</span><span>;</span> <span>price</span> <span>=</span> <span>20</span><span>.</span><span>50</span><span>;</span> <span>description</span> <span>=</span> <span>"The most shoes you'll ever see"</span><span>}</span>
        <span>{</span> <span>name</span> <span>=</span> <span>"Potatoes"</span><span>;</span> <span>price</span> <span>=</span> <span>1</span><span>.</span><span>50</span><span>;</span> <span>description</span> <span>=</span> <span>"The most potato you'll ever see"</span> <span>}</span>
        <span>{</span> <span>name</span> <span>=</span> <span>"Cars"</span><span>;</span> <span>price</span> <span>=</span> <span>10</span><span>.</span><span>3</span><span>;</span> <span>description</span> <span>=</span> <span>"The most car you'll ever see"</span> <span>}</span>
    <span>]</span>

<span>printfn</span> <span>"%s"</span> <span>result</span>
```

> To Run this, copy this content into a file named `script.fsx` (or whatever name you prefer) and type:
> 
> -   `dotnet fsi run script.fsx`

If you have ever used jinja, moustache, handlebars, liquid and similar template engines it will look familiar to you basically here we define an HTML string (which can be read from an HTML file in disk) we parse it, then render it with a data source (in case we need one)

> scriban has a TON of helpers in it's scripting language (like those pipes that are truncating a string to 15 characters)

If you want to compose components in the scriban templates the approach is way way different  

```
<span>#</span><span>r</span> <span>"nuget: Scriban"</span>

<span>open</span> <span>System</span>
<span>open</span> <span>Scriban</span>

<span>type</span> <span>Product</span> <span>=</span> 
    <span>{</span> <span>name</span><span>:</span> <span>string</span><span>;</span>
      <span>price</span><span>:</span> <span>float</span><span>;</span> 
      <span>details</span> <span>:</span> <span>{|</span> <span>description</span><span>:</span> <span>string</span> <span>|}</span> <span>}</span>
<span>// create a fragment/component html string</span>
<span>let</span> <span>detailDiv</span> <span>=</span> 
    <span>"""
    &lt;details&gt;
        &lt;summary&gt; &#123;&#123; product.details.description | string.truncate 15 &#124;&#124; &lt;summary&gt;
        &#123;&#123; product.details.description &#124;&#124;
    &lt;/details&gt;
    """</span>

<span>let</span> <span>renderProducts</span> <span>products</span> <span>=</span> 
    <span>let</span> <span>html</span> <span>=</span> 
        <span>// here with the help of sprintf</span>
        <span>// and &#123;&#123; "%s" | object.eval_template &#124;&#124;</span>
        <span>// we use F# to pre-process the template</span>
        <span>sprintf</span>
            <span>"""
            &lt;ul id='products'&gt;
            &#123;&#123; for product in products &#124;&#124;
              &lt;li&gt;
                &lt;h2&gt;&#123;&#123; product.name &#124;&#124;&lt;/h2&gt;
                     Price: &#123;&#123; product.price &#124;&#124;
                     &#123;&#123; "</span><span>%</span><span>s</span><span>" | object.eval_template &#124;&#124;
              &lt;/li&gt;
            &#123;&#123; end &#124;&#124;
            &lt;/ul&gt;
            """</span>
            <span>detailDiv</span>
    <span>let</span> <span>result</span> <span>=</span> <span>Template</span><span>.</span><span>Parse</span><span>(</span><span>html</span><span>)</span>
    <span>result</span><span>.</span><span>Render</span><span>({|</span> <span>products</span> <span>=</span> <span>products</span> <span>|})</span>

<span>let</span> <span>result</span> <span>=</span>
    <span>renderProducts</span> <span>[</span>
        <span>{</span> <span>name</span> <span>=</span> <span>"Shoes"</span>
          <span>price</span> <span>=</span> <span>20</span><span>.</span><span>50</span>
          <span>details</span> <span>=</span> 
            <span>{|</span> <span>description</span> <span>=</span> <span>"The most shoes you'll ever see"</span> <span>|}</span> <span>}</span>
        <span>{</span> <span>name</span> <span>=</span> <span>"Potatoes"</span>
          <span>price</span> <span>=</span> <span>1</span><span>.</span><span>50</span>
          <span>details</span> <span>=</span>
            <span>{|</span> <span>description</span> <span>=</span> <span>"The most potato you'll ever see"</span>  <span>|}</span> <span>}</span>
        <span>{</span> <span>name</span> <span>=</span> <span>"Cars"</span>
          <span>price</span> <span>=</span> <span>10</span><span>.</span><span>3</span>
          <span>details</span> <span>=</span>
            <span>{|</span> <span>description</span> <span>=</span> <span>"The most car you'll ever see"</span>  <span>|}</span> <span>}</span>
    <span>]</span>

<span>printfn</span> <span>"%s"</span> <span>result</span>
```

> To Run this, copy this content into a file named `script.fsx` (or whatever name you prefer) and type:
> 
> -   `dotnet fsi run script.fsx`

now, keep in mind that you're using F# and this you can modify the strings before passing them to the final template before parsing it but this leads to handling strings here and there and possibly getting into regex territory and to be honest I don't like that. I think this approach is best suited to templates you already set and know what the model for them is and that they are not super dynamic, you can still perform a lot of dynamic operations with the scriban scripting capabilities inside the template.

also keep in mind that if you really need it, you can parse and compile multiple HTML templates and then pass them together to a layout and just do a final render but I'm not aware of how performant/useful that is in practice

## [](https://dev.to/tunaxor/generating-html-in-f-4h5b#closing-thoughts)Closing Thoughts

So that's it generating HTML isn't complex and it might be useful for you, perhaps you want to build a resume from JSON to HTML, perhaps you need to build a report from the last year sales or another similar use case.

As always feel free to ping me on Twitter or the comments below 😁
