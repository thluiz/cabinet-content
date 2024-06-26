---
created: 2024-06-13T21:52:27 (UTC -03:00)
tags: []
source: https://jrsinclair.com/articles/2024/how-to-compose-functions-that-take-multiple-parameters-epic-guide/
author: Written by James Sinclair  10th June 2024
---

# How to compose JavaScript functions that take multiple parameters (the epic guide)

> ## Excerpt
> Function composition is beautiful. It lets us create elegant function pipelines. And when everything lines up, the data flows like maple syrup over pancakes. But what happens when the functions don’t line up? What if some of those functions expect more than one argument? What do we do?

---
Function composition is beautiful. In an [earlier article](https://jrsinclair.com/articles/2022/javascript-function-composition-whats-the-big-deal/), we looked at tools like `compose()` and `flow()`. These composition functions allow us to create function pipelines. They line functions up so that the output from one function flows straight into the next. And when these functions all work together, data flows like maple syrup over pancakes. But what happens when the functions don’t line up? What if some of them expect more than one argument? What do we do then? How do we compose functions with multiple parameters?

There’s a short answer to this question. We can’t.

Only unary functions compose.<sup><a href="https://jrsinclair.com/articles/2024/how-to-compose-functions-that-take-multiple-parameters-epic-guide/#notes-fn-6" id="notes-fnref-6" data-footnote-ref="" aria-describedby="footnote-label">1</a></sup> Any more than one argument and composition doesn’t work. At least, not in a helpful way. We can’t compose functions that take more than one parameter.

If composing functions with multiple parameters is impossible, why write this article? Is the title an outright lie?

Surely, there must be _something_ we can do. After all, those quirky functional programmers are forever praising the joys of composition. Why would it be so popular if there’s no way to work with multi-argument functions? There must be a way to make it work.

And there is a way.

We cheat.

We work around the limitation by changing our functions. That is, we can wrap or modify them. We _transform_ our multi-argument functions into unary functions. In this article, we’ll look at five techniques for doing that. (There are more, but these are the most common.)

1.  Composite data structures
2.  Partial application
3.  Currying
4.  Using `ap()` for the get/set problem
5.  Using `flatMap()` for the config problem

## Composite data structures

Let’s start with the simplest form of this issue. Suppose we have one function that needs two arguments. We also have another function that returns two values. … Except, we already have a problem. Functions can’t return multiple values. Each function can return exactly one value. No more.<sup><a href="https://jrsinclair.com/articles/2024/how-to-compose-functions-that-take-multiple-parameters-epic-guide/#notes-fn-4" id="notes-fnref-4" data-footnote-ref="" aria-describedby="footnote-label">2</a></sup>

Fortunately, JavaScript provides numerous ways to combine several values. The most common approach is to use a composite data structure. That is, arrays and objects.<sup><a href="https://jrsinclair.com/articles/2024/how-to-compose-functions-that-take-multiple-parameters-epic-guide/#notes-fn-1" id="notes-fnref-1" data-footnote-ref="" aria-describedby="footnote-label">3</a></sup> For example, we know returning two values from a function is impossible. But returning two values in a single array is no problem at all.

You may have seen this already if you work with a front-end framework. React’s [`useState()`](https://react.dev/reference/react/useState) function returns a value and a setter function in an array. It looks something like this:

```javascript
const temperatureStatePair = useState(23); const temperature = temperatureState[0]; const setTemp = temperatureState[1];
```

If we make use of destructuring, we can condense that to a single line:

```javascript
const [temperature, setTemp] = useState(23);
```

[SolidJS has a similar concept](https://docs.solidjs.com/guides/state-management), signals. It uses the same pattern.

```javascript
const [temperature, setTemp] = createSignal(23);
```

Now, suppose we’re working on a user interface (UI) for a thermostat. And we’d like to allow people to switch between degrees Celsius and Fahrenheit. To accomplish that, we might write a conversion function. This fancy conversion function would convert both `temperature()` and `setTemp()` for us:

```javascript
const celsiusToFahrenheit = t => t * 9 / 5 + 32; const fahrenheitToCelsius = t => (t - 32) * 5 / 9; const stateCelsiusToFahrenheit = (temperature, setTemp) => { const tempF = celsiusToFahrenheit(temperature); const setTempF = (tempC) => setTemp(fahrenheitToCelsius(tempC)); return [tempF, setTempF]; }
```

Our function converts from Celsius to Fahrenheit on the way out (`tempF`). And it converts from Fahrenheit to Celsius on the way in (`setTempF()`). However, notice that our function takes two parameters.

It would be nice to compose `useState()` and `stateCelsiusToFahrenheit()`. Currently, we can’t. But since we’re writing the function, we can change how it expects to receive arguments. We can write it so that it expects a single array instead of two parameters. Using argument destructuring, the change is just two characters:

```javascript
const stateCelsiusToFahrenheit = ([temperature, setTemp]) => { const tempF = celsiusToFahrenheit(temperature); const setTempF = (tempC) => setTemp(celsiusToFahrenheit(tempC)); return [tempF, setTempF]; }
```

With that done, we can compose with `useState()` using a `compose()` function:<sup><a href="https://jrsinclair.com/articles/2024/how-to-compose-functions-that-take-multiple-parameters-epic-guide/#notes-fn-3" id="notes-fnref-3" data-footnote-ref="" aria-describedby="footnote-label">4</a></sup>

```javascript
const compose = (...fns) => x0 => fns.reduceRight( (x, f) => f(x), x0 ); const useFahrenheit = compose( stateCelsiusToFahrenheit, useState );
```

And we can use our shiny new `useFahrenheit()` function inside a component like so:

```javascript
// Set the initial temperature to 23°C (~74°F) const [tempF, setTempF] = useFahrenheit(23);
```

This works, but it’s not so great that we have to set the initial temperature in Celsius. We can compose another function into our pipeline to fix that, too:

```javascript
// With compose(), data flows from the bottom function to the top. // We add fahrenheitToCelcius() to the bottom of the list so that // it transforms the initial input value. const useFahrenheit = compose( stateCelsiusToFahrenheit, useState, fahrenheitToCelsius, ); // Set the initial temperature to 74°F (~23°C) const [tempF, setTempF] = useFahrenheit(74);
```

We’re now storing our state as degrees Celsius but working with Fahrenheit for the UI. And that’s neat but also somewhat pointless. At least, the way we’re using it here is pointless. We’ve coded this up so that we can now _only_ use Fahrenheit. As such, we might as well store the temperature as Fahrenheit if we’re not sharing that state anywhere. We’ll return to this in a moment and do something more useful. For now, we have a working example. We can see how to use arrays to compose multi-argument functions.

### What about objects?

Let’s return to our first `useState()` example. We pulled values out by referencing array indices:

```javascript
const temperatureStatePair = useState(23); const temperature = temperatureState[0]; const setTemp = temperatureState[1];
```

This works fine, but you may have noticed that the ordering is arbitrary. That is, there’s no particular meaning to the value being in slot zero. Nor is there any meaning to the setter function being slot one. We could swap them around, and it would make little difference.<sup><a href="https://jrsinclair.com/articles/2024/how-to-compose-functions-that-take-multiple-parameters-epic-guide/#notes-fn-2" id="notes-fnref-2" data-footnote-ref="" aria-describedby="footnote-label">5</a></sup> We must remember which thing is in each arbitrary slot.

An alternative is to use objects instead. This way, we can give meaningful names to each value slot. For example, we could rewrite `stateCelsiusToFahrenheit()` to return an object:

```javascript
const stateCelsiusToFahrenheitObj = ([temperature, setTemp]) => { const tempF = celsiusToFahrenheit(temperature); const setTempF = (tempC) => setTemp(celsiusToFahrenheit(tempC)); return { temperature: tempF, setTemperature: setTempF }; };
```

We now return an object instead of an array. It has the keys `temperature` and `setTemperature`. This represents our value and setter function, respectively. Our composition pipeline remains the same:

```javascript
const useFahrenheit = compose( stateCelsiusToFahrenheitObj, useState, fahrenheitToCelsius, );
```

When we _use_ our hook, though, it’s a little different. We destructure the composed function using named properties:

```javascript
const { temperature, setTemperature } = useFahrenheit(74);
```

With this change, it doesn’t matter what order we destructure the values. We could, for example, write:

```javascript
const { setTemperature, temperature } = useFahrenheit(74);
```

The behaviour doesn’t change.

There is a drawback to this approach, though. If we want to change the names of those variables, it becomes rather verbose:

```javascript
const { temperature: tempF, setTemperature: setTempF } = useFahrenheit(74);
```

We have a trade-off. Using arrays gives a little more flexibility when destructuring. However, using objects gives us more meaningful names and allows us to reorder things. Both approaches have their uses.

Still, whether we choose arrays or objects, we now have a solution for return values. We can shove several values into a composite data structure and return that. Simple. But what if we have a function that _expects_ multiple parameters?

We can use composite data structures here, too. We wrap our multivariate function with an unary function. For example, suppose we have a function `el()` for creating HTML elements as strings:

```javascript
const el = (tag, contents) => `<${tag}>${contents}</${tag}>`;
```

We can’t compose this function with others because it expects multiple parameters. But we can wrap it in another function that expects an array:

```javascript
const elComposable = ([tag, contents]) => el(tag, contents);
```

We might use it like so:

```javascript
// Remember that with compose(), functions compose from // bottom to top const wrapListItem = compose( elComposable, item => ['li', item], ); const wrapList = compose( elComposable, list => ['ul', list.join('')], list => list.map(wrapListItem), ); const characterList = ['Holmes', 'Watson', 'Mrs. Hudson']; const characterListHTML = wrapList(characterList); characterListHTML // ￩ "<ul><li>Holmes</li><li>Watson</li><li>Mrs. Hudson</li></ul>"
```

If we’re feeling fancy, we can even create a utility function. It will convert any function to one that expects its arguments as an array. Once again, argument destructuring makes it super concise:

```javascript
const arrayifyArgs = (fn) => (args) => fn(...args); // Another way to create elComposable() const elComposable = arrayifyArgs(el);
```

Composite data structures will solve just about any composition problem involving multiple parameters. But that’s not the _only_ solution.

## Partial application

We said earlier that our `useFahrenheit()` function was useless. There’s no point in storing degrees Celsius if we only work in Fahrenheit. However, it becomes more interesting if we’re using shared state.

Suppose we replace `useState()` with `useLocalStorage()`. (You can find [a ready-made hook](https://usehooks.com/uselocalstorage) at [usehooks.com](https://usehooks.com/)). This hook has a nice API. It looks like `useState()`, except it takes an extra `key` parameter. We might use it like so:

```javascript
const PREFIX = 'my-clever-prefix-to-prevent-namespace-collisions-'; const [temperature, setTemperature] = useLocalStorage(23, `${PREFIX}temperature`);
```

The shape it returns matches the shape of `useState()`. So it seems like there ought to be a way we can use it in our `compose()` pipeline. One way is to create a new function with the `key` parameter partially applied.

```javascript
const PREFIX = 'my-clever-prefix-to-prevent-namespace-collisions-'; const tempKey = `${PREFIX}temperature`; const useDeconflictedLocalStorage = (initialVal) => useLocalStorage(initialVal, tempKey);
```

We’ve created a new function, `useDeconflictedLocalStorage()`. It ‘fixes’ the `key` for `uselocalStorage()` to a particular value. With that done, we can use `useDeconflictedLocalStorage()` in our function pipeline as before:

```javascript
const useFahrenheit = compose( stateCelsiusToFahrenheitObj, useDeconflictedLocalStorage, fahrenheitToCelsius, ); const { temperature, setTemperature } = useFahrenheit(74);
```

We’re now storing our temperature as degrees Celsius in local storage. But we’re operating in the UI as if everything were in Fahrenheit. This is no longer useless. It allows other parts of the application to read the `localstorage` value in Celsius. Or we might later switch to displaying the raw Celsius values in the UI.

That’s not the only way to do partial application, though. We can also use the `.bind()` method built into every JavaScript function. To illustrate, let’s go back to our `el()` function. We can create functions for different kinds of HTML elements.

```javascript
// The first parameter to .bind() determines what `this` is bound // to when we call the function. Since we don’t care about `this` // for el(), we set it to `null`. const ul = el.bind(null, 'ul'); const li = el.bind(null, 'li');
```

In the code above, we create two new functions. Each partially applies a tag name for the `el()` function. This gives us back a new function. We can then use those functions like so:

```javascript
const listify = compose( ul, list => list.join(''), list => list.map(li), ); const characterList = ['Holmes', 'Watson', 'Mrs. Hudson']; const characterListHTML = listify(characterList); characterListHTML // ￩ "<ul><li>Holmes</li><li>Watson</li><li>Mrs. Hudson</li></ul>"
```

To reiterate, we’ve taken `el()` and created two new functions with a ‘fixed’ parameter. In the process, we’ve converted a binary function<sup><a href="https://jrsinclair.com/articles/2024/how-to-compose-functions-that-take-multiple-parameters-epic-guide/#notes-fn-7" id="notes-fnref-7" data-footnote-ref="" aria-describedby="footnote-label">6</a></sup> into two unary functions.

With this technique, however, the order of the parameters is important. Suppose we reverse the order of the parameters in `el()`:

```javascript
const elReversed = (contents, tag) => `<${tag}>${contents}</${tag}>`;
```

If we try to use `.bind()` with `reversed ()`, we can only fix the `contents` parameter. And, usually, that’s less useful than fixing the `tag`.

Keep this in mind when you’re creating functions. If you’re using `.bind()` or currying, it helps to put the data that changes least first. We can then fix the less volatile parameters and create new functions to use as often as we choose.

Partial application is more powerful than it seems. It opens up a surprising array of tools and techniques. We’ll explore three of them in the rest of this article.

## Currying

We might find ourselves doing a lot of partial application. In that case, we can make life easier by crafting our functions so that just calling them fixes a parameter. For example, consider our `useLocalStorage()` function from earlier. Suppose we wrap it like this:

```javascript
const useLocalStorageCurried = (key) => (initialVal) => useLocalStorage(initialVal, key);
```

We’ve created a new function, `useLocalStorageCurried()`. It takes a single parameter, `key`. When we call it with a `key`, it returns another function that takes a single value, `initialVal`. Calling that function gives us back a pair of values in an array. The same values we’d get from calling `useLocalStorage()`.

If that sounds confusing, don’t worry. It’s much easier to understand by looking at how we’d use it. With `useLocalStorageCurried()`, we can create stores for different purposes. For example:

```javascript
// Create hook functions that store our data in localstorage with the given key. const useTemperature = useLocalStorageCurried('temperature'); const useHeatingStatus = useLocalStorageCurried('heating'); const useCoolingStatus = useLocalStorageCurried('cooling'); // Use our hook functions to manage state. const [temp, setTemp] = useTemperature(23); const [heatingOn, setHeatingStatus] = useHeatingStatus(false); const [coolingOn, setCoolingStatus] = useCoolingStatus(false);
```

Currying is a way of converting multi-argument functions into unary functions. We create a curried function by nesting functions inside each other. Calling the outer function fixes one parameter and returns a new function. Calling that new function fixes the following parameter, and so on. This continues until we have all the arguments we need, and we return the result.

With this technique, we can create a curried version of our `el()` function:

```javascript
const elCurried = (tag) => (contents) => `<${tag}>${contents}</${tag}>`;
```

And while we’re at it, we can create a couple of other utility functions (both curried):

```javascript
const map = (func) => (list) => list.map(func); const join = (joinStr) => (list) => list.join(joinStr);
```

Once we’ve done that, we can use them with `compose()`:

```javascript
const listify = compose( el('ul'), join(''), map(el('li')), ); const characterList = ['Holmes', 'Watson', 'Mrs. Hudson']; const characterListHTML = listify(characterList); characterListHTML // ￩ "<ul><li>Holmes</li><li>Watson</li><li>Mrs. Hudson</li></ul>"
```

Occasionally, when people see code like this, they freak out. That’s because it looks so different from what they’re used to. Let’s break it down to see how it works. Recall that `compose()` pipes data from bottom to top. We’ll go through each part in turn:

-   Our composed `listify()` function expects an array of strings.
-   When we call `listify()`, `compose()` passes this array of strings to `map(el('li'))`.
-   `map(el('li'))` wraps each item in the list with `<li></li>`.
-   Compose then passes that list of strings to `join('')`.
-   `join('')` joins all the list items together, returning a single string value.
-   Compose then passes that single string value to `el('ul')`.
-   `el('ul')` wraps the single string value in `<ul></ul>` and returns our final string.

People who are more intelligent than I have researched this style of coding. They’ve proven that you can write _any_ program using _only_ composition and currying. Some programming languages encourage this way of writing code, too. However, you may face some difficulties if you attempt to write in this style with JavaScript. One of those is what I call the get/set problem.

## Solving the get/set problem with `ap()`

If you write a lot of front-end web applications, you’ll find yourself doing the same thing over and over again. One of those patterns goes like this:

1.  Fetch some data from a remote service;
2.  Combine that data with the local state;
3.  Transform this combined data into a format that suits the UI; and
4.  Render the UI with said data.

While doing this, you’ll often find yourself following another micro-pattern. It’s likely something you do so often that you no longer notice. The micro-pattern goes like this:

1.  Take one or more values out of an object;
2.  Transform those values; and then
3.  Add the result as a new property to the same object.<sup><a href="https://jrsinclair.com/articles/2024/how-to-compose-functions-that-take-multiple-parameters-epic-guide/#notes-fn-5" id="notes-fnref-5" data-footnote-ref="" aria-describedby="footnote-label">7</a></sup>

When working with composable functions, doing the transform bit is easy. But getting the transformed value back into a copy of the starting object is a little trickier. This is because we need three pieces of information:

1.  The input object;
2.  The transformed value; and
3.  The name of the property to set in the new object.

Lining these pieces up in a composition pipeline can be tricky. To make this issue more concrete, though, let’s consider our thermostat example again. Suppose we talk to a service that returns an array of temperature observations. The data might look something like the following:

```javascript
const temps = [ { time: 1715411010990, temp: 21.2, sensor: 'bakerst' }, { time: 1715414610990, temp: 21.5, sensor: 'bakerst' }, { time: 1715418210990, temp: 20.8, sensor: 'bakerst' }, ];
```

We want to take those values and display them in a table. But the times need to be in a readable date format. Given that requirement, we might write a date formatting function like so:

```javascript
const DTFORMAT = {timeStyle: 'medium', dateStyle: 'short'} const formatDateForLocale = (languages) => { const formatter = new Intl.DateTimeFormat(languages, DTFORMAT); return (timestamp) => formatter.format(new Date(timestamp)); } const formatDate = formatDateForLocale(navigator.languages);
```

This gives us a `formatDate()` function that does the transform. We might also create getter and setter functions to get things in and out of an object:

```javascript
const getTimestamp = (tempObj) => tempObj.time; const setReadableDate = (tempObj) => (value) => ({...tempObj, readableTime: value});
```

We have all the pieces to do what we need. But we still require some way to wire them all together. We can do that with a utility called `ap()`. It looks like this:

```javascript
const ap = (binaryCurriedFn) => (unaryFn) => (value) => binaryCurriedFn(value)(unaryFn(value));
```

What does this do? One way to look at it is as follows. This function takes two function arguments and returns an unary function. This unary function passes `value` to _both_ `binaryCurriedFn()` and `unaryFn()`. It then passes the `unaryFn(value)` result as a second argument to `binaryCurriedFn()`. And we get back the result of that final call.

Here’s how we might use it to format our temperature timestamps:

```javascript
const getTimestampAndFormat = compose(formatDate, getTimestamp); const addFormattedDate = ap(setReadableDate)(getTimestampAndFormat); const tempsWithFormattedDates = temps.map(addFormattedDate);
```

We might also want to add Fahrenheit temperatures to our UI. For that, we might create some getter and setter functions like `getTempC()` and `setTempF()`. But we’d be starting to repeat ourselves. What if we created some utility functions for getting and setting?

```javascript
const get = (key) => (obj) => obj[key]; const set = (key) => (obj) => (val) => ({...obj, [key]: val});
```

We can also create a `getSet()` function that will combine them for us:

```javascript
const getSet = (setter) => (getter) => (transform) => ap(setter)(compose(transform, getter));
```

The way to think about `getSet()` is that it takes three input parameters and returns a function. That function:

1.  Extracts a value using `getter()`,
2.  Transforms the value using `transform()`, and
3.  Inserts the transformed value in the object using `setter()`.

We could then use it in a pipeline like so:

```javascript
const transformTempReadings = compose( getSet(set('tempF'))(get('temp'))(celsiusToFahrenheit), getSet(set('readableTime'))(get('time'))(formatDate), ); const readingsForUI = temps.map(transformTempReadings); readingsForUI // ￩ [ { time: 1715411010990, temp: 21.2, sensor: 'Baker St.', readableTime: '11/05/2024, 17:03:30', tempF: 70.16 }, { time: 1715414610990, temp: 21.5, sensor: 'Baker St.', readableTime: '11/05/2024, 18:03:30', tempF: 70.7 }, { time: 1715418210990, temp: 20.8, sensor: 'Baker St.', readableTime: '11/05/2024, 19:03:30', tempF: 69.44 } ]
```

This `getSet()` utility makes it convenient to create pipelines for transforming data. I find myself doing this kind of thing a lot. But there’s another issue with composition: the configuration problem.

## Solving the configuration problem with `flatMap()`

Back to our thermostat example again. Imagine we had some configuration for our app that we load whenever it starts. For example:

```javascript
const config = { locale: 'en-GB', timezone: 'GMT', defaultTarget: 23, defaultUnits: 'Celsius', baseHeatingRate: 3, baseCoolingRate: 5, sensors: { bakerst: {name: 'Baker St.', host: 'bakerst.thermostat.example.com' }, gorvesnorsq: {name: 'Grosvenor Sq.', host: 'grosvenor.thermostat.example.com' }, bedlam: {name: 'Bedlam', host: 'bedlam.thermostat.example.com' }, }, };
```

Let’s assume we’d prefer not to expose our configuration as a global variable. But suppose we are converting our list of sensor readings again. We want to compose three functions that all use the same config object.

For this task, we’ll temporarily ignore our advice about parameter ordering. We’re going to make the object that changes the least (`config`) the last parameter:

```javascript
const formatDateForLocale = (timestamp) => (config) => (new Intl.DateTimeFormat(config.local, {timeStyle: 'medium', dateStyle: 'short'})) .format(new Date(timestamp)); const addReadableDate = (obj) => (config) => ({ ...obj, readableDate: formatDateForLocale(config.locale)(obj.time) }); const addSensorName = (obj) => (config) => ({ ...obj, sensorName: config.sensors[obj.sensor]?.name }); const addTempDiff = (obj) => (config) => ({ ...obj, tempDiff: obj.temp - config.defaultTarget });
```

Each function:

1.  Takes a temperature object,
2.  Then a config object, and
3.  Returns a modified temperature object.

We want to compose these together to make the desired modifications. But we’d also like to chain them together so that they all get the same config object. To make this work, we turn to a utility called `flatMap()` (also known as `chain()`):

```javascript
const flatMap = (binaryCurriedFn) => (unaryFn) => (x) => binaryCurriedFn(unaryFn(x))(x);
```

If you look closely, it’s similar to `ap()`. But it passes the transformed value to `binaryCurriedFunction()` first instead of second.

Using `flatMap()`, we can chain our functions together in a composition flow:

```javascript
const transformTempObjs = compose( flatMap(addTempDiff), flatMap(addSensorName), addReadableDate );
```

Reading from bottom to top again, this pipeline:

1.  Adds a readable date to the object it receives,
2.  Looks up the sensor name for the entry and adds that to the object, and
3.  Adds the difference between the actual and target temperatures to the object.

It’s not so different from any other composition pipeline. The interesting thing here, though, is that these are binary functions, not unary. Our `flatMap()` helper wires the `config` option through to each function. When we run `transformTempObjs()`, it adds all three extra properties to the object:

```javascript
transformTempObjs(temps[0])(config); // ← {"time":1715411010990,"temp":21.25,"sensor":"bakerst","readableDate":"11/05/2024, 17:03:30","sensorName":"Baker St.","tempDiff":-1.75}
```

This is neat, but we have a whole array of objects we wish to convert. And having to pass that `config` object second is inconvenient. It would be nice to flip it around so that `transformTempObjs()` took the `config` object first. We can do that with another neat little utility, `flip()`:

```javascript
const flip = (binaryCurriedFn) => (b) => (a) => binaryCurriedFn(a)(b);
```

We could use this to make `transformTempObjs()` work nicely with an array `.map()` method:

```javascript
const flippedTransformTempObjs = flip(compose( flatMap(addTempDiff), flatMap(addSensorName), addReadableDate )); const transformedReadings = temps.map(flippedTransformTempObjs(config)); console.log(transformedReadings); // ⦘ [ // {"time":1715411010990,"temp":21.25,"sensor":"bakerst","readableDate":"11/05/2024, 17:03:30","sensorName":"Baker St.","tempDiff":-1.75}, // {"time":1715414610990,"temp":21.5,"sensor":"bakerst","readableDate":"11/05/2024, 18:03:30","sensorName":"Baker St.","tempDiff":-1.5}, // {"time":1715418210990,"temp":20.75,"sensor":"bedlam","readableDate":"11/05/2024, 19:03:30","sensorName":"Bedlam","tempDiff":-2.25} // ]
```

We’ve composed binary curried functions so that each one is passed the same config object. This feels magical to me.

## So what?

Imagine if we were to curry every single function we wrote. If all our functions are curried, a world of fascinating utility functions opens up. We’ve used three of these utilities in this article:

1.  `ap()`;
2.  `flatMap()`; and
3.  `flip()`.

There are many more. Functional programmers refer to these little utilities as ‘combinators.’ They’re tools that help us _combine_ functions through composition. [Avaq’s gist provides a more comprehensive list of them](https://gist.github.com/Avaq/1f0636ec5c8d6aed2e45). Playing around with combinators can be a lot of fun. It’s a bit like doing a Sudoku puzzle with types and functions.

The truth is, though, you probably don’t need combinators. Rearranging things into an array or object will usually do the job just fine. Also, sprinkling combinators through your code might make it incomprehensible to your colleagues. That can be a real problem.

So why bother learning about combinators, then? They’re just going to confuse our coworkers. And you have to curry all your functions to use them anyway. What’s the point?

As a JavaScript developer, you will find yourself composing functions frequently. This is true whether you’re conscious of it or not. And sure, you can get by using composite data structures and nothing else. But it’s a bit like limiting yourself to using only a Swiss Army knife and no other tool. Sure, it will work in most situations. But it may not be the best, and other specialised tools are available. Knowing about partial application, currying, and combinators gives you options.
