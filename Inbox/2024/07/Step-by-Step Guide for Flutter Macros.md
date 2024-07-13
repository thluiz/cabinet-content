---
created: 2024-07-11T12:49:15 (UTC -03:00)
tags: []
source: https://chooyan.hashnode.dev/make-your-macros-step-by-step-guide-for-flutter-macros
author: Tsuyoshi Chujo
---

# Step-by-Step Guide for Flutter Macros

> ## Excerpt
> Have you experienced boilerplate issues in developing Flutter apps?
I do. I always write static route() methods for an argument for Navigator.of(context).push() method.
class NextPage extends StatelessWidget {
  static Route route() {
    return Mate...

---
Have you experienced boilerplate issues in developing Flutter apps?

I do. I always write static `route()` methods for an argument for `Navigator.of(context).push()` method.

```
<span><span>class</span> <span>NextPage</span> <span>extends</span> <span>StatelessWidget</span> </span>{
  <span>static</span> Route route() {
    <span>return</span> MaterialPageRoute(builder: (context) =&gt; <span>const</span> NextPage());
  }
}

<span>// usage</span>
Navigator.of(context).push(NextPage.route());
```

This `route()` method is actually useful. On the other hand, however, I wouldn't say I like to implement the method for all the page widgets. That's the issue of boilerplate.

Now, Dart's new feature "macros" would solve the problem.

In this article, I will introduce how we can write macro classes step-by-step considering making one to solve the boilerplate of `route()` method above.

## Step-by-step guide

Before heading to the first step, please note that this article will not cover how we can configure our projects to use the macros feature.

As the feature is still in beta, and our setup will potentially change, we can check the latest information in the Flutter doc below.

[https://dart.dev/language/macros](https://dart.dev/language/macros)

Also, make sure the snippet in this article can be invalid when the feature comes to stable. This article is written based on the Flutter version `Channel master, 3.24.0-1.0.pre.28`.

Now, are you ready to code your macro? Let's get started with "step 0".

## Step 0: Decide what your macro codes

The first and most important step is to think about how your macros work and what issues they solve.

Because macros, in the end, just write boilerplate codes instead of us, we must consider what code they should write automatically.

In this article, our macro's business is generating the code below.

```
 <span>static</span> Route route() {
  <span>return</span> MaterialPageRoute(builder: (context) =&gt; <span>const</span> NextPage());
}
```

The usage looks like below.

```
<span>@RouteMacro</span>
<span><span>class</span> <span>NextPage</span> <span>extends</span> <span>StatelessWidget</span> </span>{
  <span>@override</span>
  Widget build(BuildContext context) {
    <span>return</span> <span>// NextPage UI</span>
  }
}
```

What we have to do is just adding `@RouteMacro` above the class definition, and that generates `route()` method automatically. Don't you think that's awesome?

## Step 1: Define a macro class

Fixing the brief image of what our macro does, our first step is to define a macro class.

Make `route_macro.dart` inside `lib/` folder as we usually do to make dart files.

Then, define `RouteMacro` class with `macro` keyword.

```
macro <span><span>class</span> <span>RouteMacro</span> </span>{
}
```

All the macro classes need a `const` constructor.

```
macro <span><span>class</span> <span>RouteMacro</span> </span>{
  <span>const</span> RouteMacro();
}
```

You can also accept arguments as much as you need in your logic, as long as the arguments are also made with `const`.

## Step 2: Implement relevant macro interface

For macro classes, dart provides a lot of `Macro` interfaces, such as `LibraryTypesMacro`, `ClassDefinitionMacro`, `EnumDeclarationsMacro`, etc.

You can choose relevant interface(s) and implement it(them) in your macro classes overriding a method defined in the interface.

```
macro <span><span>class</span> <span>RouteMacro</span> <span>implements</span> <span>ClassDeclarationsMacro</span> </span>{
  <span>@override</span>
  Future&lt;<span>void</span>&gt; buildDeclarationsForClass(
    ClassDeclaration clazz, 
    MemberDeclarationBuilder builder,
  ) {
  }
}
```

You can choose "relevant" interfaces based on "where" and "what" you want the macro to generate.

Let's say, for instance, if you want to add "declarations" to "classes", the relevant interface would be `ClassDeclarationsMacro`. If you want to add "types" to "libraries", you may want `LibraryTypesMacro`.

For accurate understanding, macros has an idea of "phases", and the interfaces are designed based on that idea. See the page below for more information.

[https://github.com/dart-lang/language/blob/main/working/macros/feature-specification.md#phases](https://github.com/dart-lang/language/blob/main/working/macros/feature-specification.md#phases)

You can check all the `Macro` interfaces in `macros.dart` below. They must inspire what you want to do with Dart macros!

[https://github.com/dart-lang/sdk/blob/main/pkg/\_macros/lib/src/api/macros.dart](https://github.com/dart-lang/sdk/blob/main/pkg/_macros/lib/src/api/macros.dart)

Because we want to add a **declaration** of `route()` method to `NextPage`**class**, our Macros interface would be `ClassDeclarationMacro`. See the code above again for how our `RouteMacro` class looks like now.

## Step 3: Confirm the implementing method and its arguments

As each `Macro` interface has a single abstract method to be implemented, we can override it and implement how our macros behave there.

```
<span>@override</span>
Future&lt;<span>void</span>&gt; buildDeclarationsForClass(
  ClassDeclaration clazz, 
  MemberDeclarationBuilder builder,
) {
  <span>// implement our macro's operation here</span>
}
```

We have two arguments passed, `ClassDeclaration` and `MemberDeclarationBuilder`.

`ClassDeclaration` preserves information about the class where we attach our macro, `NextPage` in this case. Definitions of the class, such as its name, its implementing interfaces, or other modifiers can be detected from `clazz` object.

```
<span>@override</span>
Future&lt;<span>void</span>&gt; buildDeclarationsForClass(
  ClassDeclaration clazz, 
  MemberDeclarationBuilder builder,
) {
  clazz.identifier.name; <span>// -&gt; "NextPage"</span>
  clazz.hasSealed; <span>// -&gt; false</span>
}
```

`MemberDeclarationBuilder` has two main functionalities, detecting declarations of the `clazz` and generating codes.

```
<span>@override</span>
Future&lt;<span>void</span>&gt; buildDeclarationsForClass(
  ClassDeclaration clazz, 
  MemberDeclarationBuilder builder,
) <span>async</span> {
  <span>await</span> builder.fieldsOf(clazz); <span>// -&gt; List&lt;FieldDeclaration&gt;</span>
  builder.declareInLibrary(
    DeclarationCode.fromString(<span>'final foo = 1'</span>),
  ); 
}
```

`.fieldsOf(clazz)` will find all the fields declared in the given `clazz`. As the method returns `Future`, note that we put `await` before calling it and `async` at the method declaration.

That functionality helps us write operations using declarations of fields, methods, or some other elements in the class.

`.declareInLibrary()` and `.declareInType()` are the very methods we need for generating our code.

Both receive `DeclarationCode` that represents generated code, and we have two options for how we describe the code, `DeclarationCode.fromString` and `DeclarationCode.fromParts`. We will discuss how to use them later.

Depending on what interface we implement, the names of the overridden methods and their arguments are slightly different, but the concept doesn't change.

## Step 4: Generate our code!

As we have seen before, our code to be generated here is below.

```
<span>static</span> Route route() {
  <span>return</span> MaterialPageRoute(builder: (context) =&gt; <span>const</span> NextPage());
}
```

And we have `DeclarationCode.fromString` for generating code with a simple `String`. So our macro can be implemented with the code below.

```
<span>@override</span>
Future&lt;<span>void</span>&gt; buildDeclarationsForClass(
  ClassDeclaration clazz, 
  MemberDeclarationBuilder builder,
) <span>async</span> {
  builder.declareInType(
    DeclarationCode.fromString(
      <span>'''
static Route route() {
  return MaterialPageRoute(builder: (context) =&gt; const NextPage());
}
'''</span>,
    ),
  ); 
}
```

It doesn't work, however, because we have two main problems here.

1.  the compiler can't understand where `MaterialPageRoute` and `Route` came from
    
2.  `NextPage` can change depending on what class the macro is attached to
    

So, let's tackle the issues one by one.

### Import `material.dart`

What we have to do when the compiler can't find the definition of types is `import`.

As `MaterialPageRoute` is defined in `material.dart`, we can add `import 'package:flutter/material.dart';` at the top of the generated file.

Here, we have two options for generating our code, `.declareInLibrary()` and `.declareInType()`. What we choose in this case is `.declareInLibrary()` since `import` can't be declared in the type but in the library, or the file in other words.

Thus, add the snippet below.

```
builder.declareInLibrary(DeclarationCode.fromString(
  <span>"import 'package:flutter/material.dart';"</span>,
));
```

This enables the compiler to find `MaterialPageRoute` and `Route`.

### Dynamically change the Widget's name

One macro can be used multiple times at multiple places, so it's irrelevant to hardcode `NextPage` in `RouteMacro`.

Here comes the second option, `DeclarationCode.fromParts`. Take a look at the code below.

```
<span>final</span> widget = clazz.identifier;
builder.declareInType(DeclarationCode.fromParts([
  <span>'''
  static Route route() {
    return MaterialPageRoute(
      builder: (context) =&gt; const '''</span>,
  widget,
    <span>'''(),
    );
  }
  '''</span>,
]),
```

While `DeclarationCode.fromString` just add a code with a single `String`, `DeclarationCode.fromParts` constructs a code with the `List` of `String` and `Identifier`.

`String` can be used for the static snippet, and `Identifier` for the dynamically changing type.

Instead of just embed `String` that represents the name of the widget like `${clazz.identifier.name}`, passing `Identifier` to the List enables our macro to maintain `import` for the classes.

We can check how the generated code looks like by opening the augmentation.

```
<span>import</span> <span>'package:macros_practice/next_page.dart'</span> <span>as</span> prefix0;
...
<span>return</span> MaterialPageRoute(
  builder: (context) =&gt; <span>const</span> prefix0.NextPage(),
);
```

Without declaring `import` by hand, our macro implicitly adds the line with a prefix to avoid a conflict when we have different classes with the same name.

## Step 5: Use the generated code!

That's it! Our `RouteMacro` is now implemented below.

```
<span>import</span> <span>'package:macros/macros.dart'</span>;

macro <span><span>class</span> <span>RouteMacro</span> <span>implements</span> <span>ClassDeclarationsMacro</span> </span>{
  <span>const</span> RouteMacro();

  <span>@override</span>
  Future&lt;<span>void</span>&gt; buildDeclarationsForClass(
    ClassDeclaration clazz,
    MemberDeclarationBuilder builder,
  ) <span>async</span> {
    builder.declareInLibrary(DeclarationCode.fromString(
      <span>"import 'package:flutter/material.dart';"</span>,
    ));

    <span>final</span> widget = clazz.identifier;
    builder.declareInType(DeclarationCode.fromParts([
      <span>'''
static Route route() {
  return MaterialPageRoute(
    builder: (context) =&gt; const '''</span>,
      widget,
      <span>'''(),
  );
}
'''</span>,
      ]),
    );
  }
}
```

We don't need to declare `route()` method at any page widget. What we have to do is just write `@RouteMacro` above the type declaration.

```
<span>@RouteMacro</span>()
<span><span>class</span> <span>NextPage</span> <span>extends</span> <span>StatelessWidget</span> </span>{
  <span>const</span> NextPage({<span>super</span>.key});

  <span>@override</span>
  Widget build(BuildContext context) {
    <span>return</span> <span>const</span> Scaffold();
  }
}
```

This enables us to call `NextPage.route()` wherever we need.

```
onTap: () {
  Navigator.of(context).push(NextPage.route());
}
```

Again, don't forget to refer to the Flutter doc for how to run the app with this experimental feature.

[https://dart.dev/language/macros](https://dart.dev/language/macros)

## Wrap up

So far, we have discussed how we make our own macro classes, and how each API can be used.

We have to be aware that the feature is still experimental and the APIs introduced in this article can be changed when the stable version is released, so it would be important to focus not on concrete usage but on the core concept of it.

Though we have only few resources to learn how to make macros, unfortunately, I hope this article will help your first step jumping into creating your macros!

## Learn more

If you are now interested in creating macros, check the official samples, and it will inspire you more!

[https://github.com/dart-lang/language/tree/main/working/macros/example](https://github.com/dart-lang/language/tree/main/working/macros/example)
