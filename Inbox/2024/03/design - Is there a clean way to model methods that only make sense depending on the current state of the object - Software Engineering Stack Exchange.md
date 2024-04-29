---
created: 2023-11-02T12:06:14 (UTC -03:00)
tags: []
source: https://softwareengineering.stackexchange.com/questions/448261/is-there-a-clean-way-to-model-methods-that-only-make-sense-depending-on-the-curr?utm_source=iterable&utm_medium=email&utm_campaign=the-overflow-newsletter
author: Mehdi Charife
---

# design - Is there a clean way to model methods that only make sense depending on the current state of the object? - Software Engineering Stack Exchange

> ## Excerpt
> Let's say that I have a class called Mission. This class is intended to represent a mission requested by a professor working in a faculty. Mission has a private enum field representing whether the

---
What you've done here has a name. This is an internal [Domain Specific Language](https://en.wikipedia.org/wiki/Domain-specific_language) (iDSL). It can be used like this:

```
void execute(PlannedMission plannedMission) {
    plannedMission
        .start()
        .complete()
        .requestReimbursement()
    ;
}
```

Or like this:

```
AbortedMission abort(PlannedMission plannedMission) {
    return plannedMission
        .start()
        .abort()
    ;
}
```

This lets you give names to the different paths (here execute & abort) through your states by listing the transitions in the path. You can even decouple this from the concrete implementation by making `PlannedMission`, and its linked types, extensible (abstract, an interface, whatever, just not final). That way these named paths don't need rewriting even if what they call out to does.

I've built [iDSLs](https://stackoverflow.com/questions/22909717/is-this-monster-builder-a-good-builder-factory-pattern-for-abstracting-long-co) before. Mostly to solve construction problems. They are the overpowered version of [Joshua Bloch's Builder Pattern](https://softwareengineering.stackexchange.com/a/367146/131624) which is really about simulating named arguments in languages that don't have them (like Java) without using setters so your object can be immutable. The using code looks the same but since it only uses one type you can call the methods in any order. The Bloch Builder is not a DSL.

You might look at this using code an be tempted to call it a [Law of Demeter](https://softwareengineering.stackexchange.com/a/284146/131624) violation. If these were randomly collected types and methods from all over the code base I'd agree with you. No one promised you this crazy path would always work so don't whine when it breaks. However, these aren't random types. These were designed to work together. They will change together and be deployed together. These are all friends. Not friends of friends.

DSLs are very powerful and very specific. When you build one inside an existing language (why I called it internal) like Java or C# you get to leverage a lot of existing tooling for your little language.

But yes, setting it up is still a lot of work. Generally, don't reach for this unless it will be widely used in your code base to offset the extra work setting it up.

Right now you only have two paths through this. So there's a good reason this seems over engineered. It is. There's not a lot of need for the flexibility the DSL provides. There's not a lot of reason to break up these steps as different state transitions. But if you really can't stand allowing a `abort` call to happen when in the wrong state then this works.

However, consider the [GoF's State Pattern](https://www.gofpattern.com/behavioral/patterns/state-pattern.php#:%7E:text=The%20State%20design%20pattern%20is,behavior%20when%20the%20state%20changes.). With this design you could always call abort. Even when it didn't make sense. And it wouldn't have to do type checking. Rather, when you send a `abort` message to a completed mission it would quietly do a whole lot of nothing.

This is in line with the Tell, Don't Ask philosophy. You don't have to know how everything turns out. You can make something else deal with that. This borrows from my favorite pattern. The [null object pattern](https://en.wikipedia.org/wiki/Null_object_pattern). Sometimes quietly doing nothing is the thing to do. You've likely already used it. The empty string ("") is a null object. We use it to tell things to print nothing all the time.

So you have a choice. Obsessively control the callable messages so they are always correct to send or just deal with whatever messages come correctly.

The nice thing about the second one is it allows your abstract mission object to be a lot more useful.
