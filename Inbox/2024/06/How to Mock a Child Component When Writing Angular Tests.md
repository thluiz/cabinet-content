---
created: 2024-06-26T15:01:03 (UTC -03:00)
tags: []
source: https://spin.atomicobject.com/mock-child-component-angular/?ref=dailydev
author: Casey Falkowski
---

# How to Mock a Child Component When Writing Angular Tests

> ## Excerpt
> Normally, I am fully in favor of NOT mocking a child component whenever possible, but some situations require it.

---
During a recent software project, I built out a component with child components I did not have control over since they came from an external library. They were also causing me some testing headaches. They had some behaviors on initialization that interfered with my tests, and I needed them to behave in a way that I had control over. While normally I am fully in favor of not mocking child components whenever possible, this was a situation that required it. I had a bit of trouble finding simple, concise info on how to mock child components, so I figured I’d help some others from the stress. Here’s a quick, to the point, explanation on how to mock child components.

## Defining a dummy component to use in the child component’s place.

Say the component you’re testing has a child component called, well, `ChildComponent`, and it has a selector called `child-component`. In my aforementioned case, the child component of the component I was testing had a function that ran on initialization that was causing headaches. For our example, let’s say we have a similar situation. Let’s say our `ChildComponent` has an aptly-named function called `troublemaker()`. Whenever our `ChildComponent` is instantiated, `troublemaker()` gets called and, well, makes trouble.

So what’s the simplest way we could go about fixing this? We make a new component altogether, and inject that component in place of the real child component within the test.

For simplicity, lets just go ahead and define a new component at the top of our testing file. It’ll look like this:

```typescript
@Component({ selector: 'child-component', template: '<div></div>', providers: [ { provide: ChildComponent, useClass: MockChildComponent } ] }) class MockChildComponent { public troublemaker = () => null; }
```

This is pretty straightforward. We’re literally just defining a new component and giving it the exact same selector. We give it a blank template, and in the component class we define a dummy function with the same name as the one giving us problems. In effect, we’re replacing the component with one that is completely non-functional. We could give it some function if our test required that, of course, but in our example it isn’t necessary.

Lastly, make sure you include the mock component class in the declarations when setting up the test bed:

```typescript
beforeEach(() => { TestBed.configureTestingModule({ imports: [ ... ], providers: [ ... ], declarations: [ ... MockChildComponent, ] }) })
```

And with that, your child component is being mocked, and your tests should (hopefully) pass.
