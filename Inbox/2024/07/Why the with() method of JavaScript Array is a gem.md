---
created: 2024-07-22T15:41:06 (UTC -03:00)
tags: []
source: https://blog.greenroots.info/javascript-array-method-with-immutability
author: Tapas Adhikary
---

# Why the with() method of JavaScript Array is a gem?

> ## Excerpt
> JavaScript array methods offer plenty to web developers already. The methods like map(), reduce(), filter() , some(), every(), and many more have helped build our JavaScript muscles for better logic and algorithms.
The with() method is a relatively n...

---
JavaScript array methods offer plenty to web developers already. The methods like `map()`, `reduce()`, `filter()` , `some()`, `every()`, and many more have helped build our [JavaScript muscles](https://blog.greenroots.info/build-your-javascript-muscles-with-map-reduce-filter-and-other-array-iterators) for better logic and algorithms.

The `with()` method is a relatively new inclusion to the JavaScript array method list, and its only intention is to change the value of a given index in an array in the "immutable" way. A big deal? Yes! Let's learn.

> Btw, please do not confuse the `with()` method from the JavaScript Array with the `with` statement from the language. The [with statement has been deprecated](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/with) whereas, the with() method is a new inclusion to the language. People say JavaScript is weird, naming things is possibly a reason for it!

This article is also available as a video tutorial now. You can check it out here:

<iframe width="100%" height="100%" src="https://www.youtube.com/embed/TFsria_3sw8?feature=oembed" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen="" title="Why Do You Need To Know JavaScript Array's with() Method 🔥"></div></iframe>

## The traditional way to seek and change

Let us consider an array of numbers,

```
<span>const</span> numbers = [<span>1</span>, <span>2</span>, <span>3</span>, <span>4</span>, <span>5</span>];
```

How would you change the element of an array for a given index? Suppose you need to change the element `3` which is at the index `2`(remember, the array index in JavaScript starts with 0) with a new element, say, `6`.

The usual instinct would be to do this,

```
numbers[<span>2</span>] = <span>6</span>;

<span>console</span>.log(numbers); <span>// [1, 2, 6, 4, 5];</span>
```

This works! However, there is a problem. We have mutated(aka, permanently changed) the original array. When you deal with data in your application, a data structure like an array should hold the original(unchanged) data as a "source of truth". You wouldn't want it to be modified by any functions and logic to achieve better predictability in your code.

`Immutability` is a mechanism to ensure that we can not change something(an array, object, etc). However, if we need a modified version of it, we need to first create a copy of the original and then make modifications to the copied version. This ensures that the original data is never changed by anything in the application, knowingly or unknowingly.

So, how about changing the element of an array at a given index, in an immutable way? It means every time we change an element in an array, we do not mutate the original array, rather we get back a new copy of the original array with the change applied.

## The with() method and immutability

ECMAScript 2023 introduced a new method called `with()` to solve this problem of mutability and achieve immutability while changing an element of an array. So now, instead of using a given index on the array directly, we can use the `with()` method to change the element value.

The `with()` method takes two parameters:

-   `index` - An index value which can start from `0`, and also can be a negative number. The negative index counts backwards from the end of the array.
    
-   `value` - The value to change at a given index.
    

Let us now apply the `with()` method on the same array to change the element at a given index,

```
<span>const</span> numbers = [<span>1</span>, <span>2</span>, <span>3</span>, <span>4</span>, <span>5</span>];

<span>const</span> newArray = numbers.with(<span>2</span>,<span>6</span>);

<span>console</span>.log(numbers); <span>// Unchanged =&gt; [1, 2, 3, 4, 5];</span>
<span>console</span>.log(newArray); <span>// Changed(A new copy) =&gt; [1, 2, 6, 4, 5];</span>
```

As you see, the `with()` method produces a new copy of the original array with the change applied. The original array is unchanged.

## The with() method and the negative index

One more drawback of the direct index-based element seeking to get an element at a given index is that you can not use the negative number as an index value. Try something like the code snippet below, you will get an `undefined`.

```
numbers[<span>-2</span>]; <span>// undefined</span>
```

Also, trying to set a value on a negative index will have unexpected results,

```
<span>const</span> numbers = [<span>1</span>, <span>2</span>, <span>3</span>, <span>4</span>, <span>5</span>];
numbers[<span>-2</span>] = <span>8</span>;

<span>console</span>.log(numbers); <span>// [1, 2, 3, 4, 5, -2: 8]</span>
```

Did you see that? You have ended up adding something completely different than what you anticipated. But, using the `with()` method, you can change an element of an array at a negative index. The negative index starts from the end of the array with an index value of `-1` and move backwards.

```
<span>const</span> numbers = [<span>1</span>, <span>2</span>, <span>3</span>, <span>4</span>, <span>5</span>];

<span>const</span> anotherArray = numbers.with(<span>-2</span>, <span>8</span>);
<span>console</span>.log(anotherArray); <span>// [1, 2, 3, 8, 5]</span>
```

In the above code, we have changed the second element of an array from the end. Hence the element `4` in the array is replaced with the element `8`.

You can use the [at() method](https://blog.greenroots.info/why-do-you-need-to-know-about-the-javascript-array-at-method) of JavaScript array to read an element using a negative index.

```
anotherArray.at(<span>-2</span>); <span>// 8</span>
```

## Method chaining

As the `with()` method returns an array, you can chain the output of the `with()` method along with other array methods, like map(), filter(), reduce(), etc.

In the code snippet below, we replace the second element of the `ages` array with a new element, and then map the output array to multiply each element by four.

```
<span>const</span> ages = [<span>12</span>, <span>23</span>, <span>56</span>];

<span>const</span> mappedAges = ages.with(<span>1</span>, <span>32</span>).map(<span><span>x</span> =&gt;</span> x * <span>4</span>);

<span>console</span>.log(mappedAges); <span>// [48, 128, 224]</span>
```

## Can you please give a real-life use case of the with() method?

Sure! When I published a [YouTube video](https://www.youtube.com/watch?v=TFsria_3sw8) on this topic, I got a question about the real-life use case of the `with()` method. The exciting thing is, that I discovered the with() method when I was building a use case that needs the feature it offers. The image below provides a summary of the use case.

![JavaScript Array with() method use case](https://cdn.hashnode.com/res/hashnode/image/upload/v1720409375079/edb88aee-5f93-4c9b-8c42-cb0b4ecfaabd.png?auto=compress,format&format=webp)

## Conclusion

That's all for now. I hope you agree with me that the `with()` method is a true gem providing us immutability and negative index accessibility with JavaScript Arrays. You can check out these articles to learn more about JavaScript arrays and their unique use cases:

-   [Build your JavaScript Muscles with map, reduce, filter and other array iterators](https://blog.greenroots.info/build-your-javascript-muscles-with-map-reduce-filter-and-other-array-iterators)
    
-   [5 ways to merge arrays in JavaScript and their differences](https://blog.greenroots.info/5-ways-to-merge-arrays-in-javascript-and-their-differences)
    
-   [Why do you need to know about Array-like Objects?](https://blog.greenroots.info/why-do-you-need-to-know-about-array-like-objects)
    
-   [Why do you need to know about the JavaScript Array at() method?](https://blog.greenroots.info/why-do-you-need-to-know-about-the-javascript-array-at-method)
    

___

Liked it? Please let me know with your comments and likes. ❤️

Let's connect. I share knowledge on web development, content creation, Open Source, and careers on these platforms.

-   [**tapaScript on YouTube**](https://www.youtube.com/tapasadhikary?sub_confirmation=1)
    
-   [**X(Twitter)**](https://twitter.com/tapasadhikary)
    
-   [**LinkedIn**](https://www.linkedin.com/in/tapasadhikary/)
    
-   [**GitHub**](https://github.com/atapas)
