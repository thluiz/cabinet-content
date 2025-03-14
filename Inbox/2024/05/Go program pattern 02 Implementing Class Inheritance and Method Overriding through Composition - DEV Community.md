---
created: 2024-05-01T09:38:03 (UTC -03:00)
tags: [go,pattern,software,coding,development,engineering,inclusive,community]
source: https://dev.to/huizhou92/go-program-pattern-02-implementing-class-inheritance-and-method-overriding-through-composition-313i
author: huizhou92
---

# Go program pattern 02: Implementing Class Inheritance and Method Overriding through Composition - DEV Community

> ## Excerpt
> In the previous tutorial, I have already introduced that Go language, unlike object-oriented...

---
In the previous tutorial, I have already introduced that Go language, unlike object-oriented programming languages such as Java and PHP, does not support keywords like `class` to define classes. Instead, it uses the `type` keyword combined with basic types or structures to define the type system. Additionally, it does not support explicitly defining inheritance relationships between types using the `extends` keyword.

> This article is first published in the medium MPP plan. If you are a medium user, please follow me in [medium](https://medium.hxzhouh.com/). Thank you very much.

Strictly speaking, Go language is not an object-oriented programming language, at least not the best choice for object-oriented programming (Java is the most established one). However, we can simulate object-oriented programming based on some features provided by Go.

To implement object-oriented programming, we must implement the three major features of object-oriented programming: encapsulation, inheritance, and polymorphism.

## [](https://dev.to/huizhou92/go-program-pattern-02-implementing-class-inheritance-and-method-overriding-through-composition-313i#inheritance)Inheritance

Next is **inheritance**. Although Go does not directly provide syntax for inheritance, we can indirectly achieve similar functionality through **composition**. Composition means embedding one type into another type to build a new type structure.

In traditional object-oriented programming, explicitly defining inheritance relationships has two drawbacks: one is that it leads to increasingly complex class hierarchies, and the other is that it affects the extensibility of classes. Many software design patterns advocate using composition instead of inheritance to improve class extensibility.

Let's take an example. Suppose we want to create a UI component library. We have a `Widget` structure type with two properties, `x` and `y`, representing the length and width of the component.

If we want to define a class representing `Label`, we can do it like this:

```
<span>type</span> <span>Label</span> <span>struct</span> <span>{</span>
    <span>Widget</span>
    <span>text</span> <span>string</span>
<span>}</span>
```

Here, `Label` inherits all the properties of `Widget` and adds a new property `text`. Similarly, we can define the `Button` and `ListBox` classes:  

```
<span>type</span> <span>Button</span> <span>struct</span> <span>{</span>
    <span>Label</span>
<span>}</span>
<span>type</span> <span>ListBox</span> <span>struct</span> <span>{</span>
    <span>Widget</span>
    <span>text</span>  <span>[]</span><span>string</span>
    <span>index</span> <span>int</span>
<span>}</span>
```

## [](https://dev.to/huizhou92/go-program-pattern-02-implementing-class-inheritance-and-method-overriding-through-composition-313i#polymorphism)Polymorphism

First, we define two interfaces, `Painter` for painting and `Clicker` for clicking:  

```
<span>type</span> <span>Painter</span> <span>interface</span> <span>{</span>
    <span>Paint</span><span>()</span>
<span>}</span>
<span>type</span> <span>Clicker</span> <span>interface</span> <span>{</span>
    <span>Click</span><span>()</span>
<span>}</span>
```

Then, the components implement these interfaces:  

```
<span>func</span> <span>(</span><span>label</span> <span>Label</span><span>)</span> <span>Paint</span><span>()</span> <span>{</span>
    <span>// display label</span>
    <span>fmt</span><span>.</span><span>Printf</span><span>(</span><span>"%p:Label.Paint(%q)</span><span>\n</span><span>"</span><span>,</span> <span>&amp;</span><span>label</span><span>,</span> <span>label</span><span>.</span><span>text</span><span>)</span>
<span>}</span>

<span>func</span> <span>(</span><span>button</span> <span>Button</span><span>)</span> <span>Paint</span><span>()</span> <span>{</span>
    <span>// display button</span>
    <span>fmt</span><span>.</span><span>Printf</span><span>(</span><span>"Button.Paint(%q)</span><span>\n</span><span>"</span><span>,</span> <span>button</span><span>.</span><span>text</span><span>)</span>
<span>}</span>
<span>func</span> <span>(</span><span>button</span> <span>Button</span><span>)</span> <span>Click</span><span>()</span> <span>{</span>
    <span>// click button</span>
    <span>fmt</span><span>.</span><span>Printf</span><span>(</span><span>"Button.Click(%q)</span><span>\n</span><span>"</span><span>,</span> <span>button</span><span>.</span><span>text</span><span>)</span>
<span>}</span>

<span>func</span> <span>(</span><span>listBox</span> <span>ListBox</span><span>)</span> <span>Paint</span><span>()</span> <span>{</span>
    <span>// display listBox</span>
    <span>fmt</span><span>.</span><span>Printf</span><span>(</span><span>"ListBox.Paint(%q)</span><span>\n</span><span>"</span><span>,</span> <span>listBox</span><span>.</span><span>text</span><span>)</span>
<span>}</span>
```

`Label` implements `Painter`, and `Button` and `ListBox` implement both `Painter` and `Clicker`.

At the application level, we can use these components like this:  

```
<span>label</span> <span>:=</span> <span>Label</span><span>{</span><span>Widget</span><span>{</span><span>10</span><span>,</span> <span>10</span><span>},</span> <span>"State:"</span><span>}</span>
<span>button1</span> <span>:=</span> <span>Button</span><span>{</span><span>Label</span><span>{</span><span>Widget</span><span>{</span><span>10</span><span>,</span> <span>70</span><span>},</span> <span>"OK"</span><span>&#124;&#124;</span>
<span>button2</span> <span>:=</span> <span>NewButton</span><span>(</span><span>50</span><span>,</span> <span>70</span><span>,</span> <span>"Cancel"</span><span>)</span>
<span>listBox</span> <span>:=</span> <span>ListBox</span><span>{</span><span>Widget</span><span>{</span><span>10</span><span>,</span> <span>40</span><span>},</span>
    <span>[]</span><span>string</span><span>{</span><span>"AL"</span><span>,</span> <span>"AK"</span><span>,</span> <span>"AZ"</span><span>,</span> <span>"AR"</span><span>},</span> <span>0</span><span>}</span>
<span>for</span> <span>_</span><span>,</span> <span>painter</span> <span>:=</span> <span>range</span> <span>[]</span><span>Painter</span><span>{</span><span>label</span><span>,</span> <span>listBox</span><span>,</span> <span>button1</span><span>,</span> <span>button2</span><span>}</span> <span>{</span>
    <span>painter</span><span>.</span><span>Paint</span><span>()</span>
<span>}</span>
<span>fmt</span><span>.</span><span>Println</span><span>(</span><span>"========================================="</span><span>)</span>
<span>for</span> <span>_</span><span>,</span> <span>clicker</span> <span>:=</span> <span>range</span> <span>[]</span><span>Clicker</span><span>{</span><span>listBox</span><span>,</span> <span>button1</span><span>,</span> <span>button2</span><span>}</span> <span>{</span>
    <span>clicker</span><span>.</span><span>Click</span><span>()</span>
<span>}</span>
<span>fmt</span><span>.</span><span>Println</span><span>(</span><span>"========================================="</span><span>)</span>
<span>for</span> <span>_</span><span>,</span> <span>widget</span> <span>:=</span> <span>range</span> <span>[]</span><span>interface</span><span>{}{</span><span>label</span><span>,</span> <span>listBox</span><span>,</span> <span>button1</span><span>,</span> <span>button2</span><span>}</span> <span>{</span>
    <span>widget</span><span>.</span><span>(</span><span>Painter</span><span>)</span><span>.</span><span>Paint</span><span>()</span>
    <span>if</span> <span>clicker</span><span>,</span> <span>ok</span> <span>:=</span> <span>widget</span><span>.</span><span>(</span><span>Clicker</span><span>);</span> <span>ok</span> <span>{</span>
        <span>clicker</span><span>.</span><span>Click</span><span>()</span>
    <span>}</span>
<span>}</span>
```

Go language is different from object-oriented programming languages like Java and PHP in that it does not provide keywords specifically for referencing parent class instances (such as `super`, `parent`, etc.). In Go language, the design philosophy is simplicity, without any unnecessary keywords. All calls are straightforward.

## [](https://dev.to/huizhou92/go-program-pattern-02-implementing-class-inheritance-and-method-overriding-through-composition-313i#summary)Summary

Let's summarize briefly. In Go language, the concept of classes in traditional object-oriented programming is intentionally weakened, which is in line with Go's philosophy of simplicity. The "classes" defined based on structures are just ordinary data types, similar to built-in data types. Built-in data types can also be transformed into "classes" that can contain custom member methods using the `type` keyword.

All methods associated with a data type collectively form the method set of that type. Like other object-oriented programming languages, methods within the same method set cannot have the same name. Additionally, if they belong to a structure type, their names cannot overlap with any field names in that type.

## [](https://dev.to/huizhou92/go-program-pattern-02-implementing-class-inheritance-and-method-overriding-through-composition-313i#references)References

-   [How to pass a 'child' struct into a function accepting 'parent' struct?](https://stackoverflow.com/questions/37011799/how-to-pass-a-child-struct-into-a-function-accepting-parent-struct)
-   [Check if a struct has struct embedding at run time](https://stackoverflow.com/questions/61585699/check-if-a-struct-has-struct-embedding-at-run-time)
-   [GO编程模式：委托和反转控制](https://coolshell.cn/articles/21214.html)
