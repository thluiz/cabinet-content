---
created: 2023-07-20T11:00:47 (UTC -03:00)
tags: []
source: https://softwareengineering.stackexchange.com/questions/378748/whats-the-difference-between-observer-pattern-and-listeners
author: user315772user315772
        
            11111 gold badge11 silver badge33 bronze badges
---

# design - What's the difference between Observer pattern and listeners? - Software Engineering Stack Exchange

> ## Excerpt
> I have used some kind of "listeners" where I have an interface implemented by classes that need to be notified of some event (e.g.: CurrencyListener, with a method currencyUpdated(Currency

---
This _is_ the Observer pattern - it's the exact same thing.

> I have used some kind of "listeners" where I have an interface implemented by classes that need to be notified of some event (e.g.: CurrencyListener, with a method currencyUpdated(Currency currency))

In the Observer pattern, you have an abstraction (an interface or a base class) _Observer_ which defines the things the observer can observe (or listen for). Your CurrencyListener is the _Observer_.

The other part of the pattern is the _Observable_, which is the object that sends notifications, which may or may not be itself an abstraction, or part of some hierarchy. I.e., the pattern _does not require_ the ConcreteObservable subclases to be present. In the Go4 Patterns book, one of the roles for a ConcreteObservable is described as

> "stores state of interest to ConcreteObserver objects."

If a ConcreteObserver can has no need for any special state, or can work with just the interface provided by the Observable base class, then _there's no need_ for a ConcreteObservable.

> Then, the object that needs to send a notification, has a list of listeners (List) and just iterates over this list invoking the currencyUpdated(Currency currency) method

The _Observer_ provides methods for the _Observables/Listeners_ to register with it, and it internally maintains a collection of these listeners. That's exactly what your object does.

Again, it's just the classic Observer pattern. The Observers and the Observables are independent of each other because they both depend on the abstraction provided by the Observer interface, and because they rely on the client code that uses them to hook up the Observers/Listeners with the Observable.

P.S. There's a number of variations on the pattern, and a number of features in different languages that are just the Observer pattern in disguise. For example, in C#, events are clearly a variation of the pattern. Another example of a variation would be what's usually called a Messenger (or an Event Aggregator, as Martin Fowler calls it), an can be found in some libraries and frameworks - it's a class that acts as as an intermediary between the observers and the observables. The structure is slightly different, but it is based around the Observer pattern; in fact, the Go4 book describes something similar near the end of the Implementation section in the description of the pattern (they call it ChangeManager).
