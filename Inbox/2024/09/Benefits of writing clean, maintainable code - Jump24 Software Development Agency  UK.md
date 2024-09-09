---
created: 2024-09-06T14:32:38 (UTC -03:00)
tags: []
source: https://jump24.co.uk/journal/benefits-of-writing-clean-maintainable-code/
author: By Jamie Peters
---

# Benefits of writing clean, maintainable code - Jump24 Software Development Agency | UK

> ## Excerpt
> I have been writing code now for close to 20 years, but for the vast majority of that, I never really paid much attention to how my code looked, how maintainable it was long term, I was only really interested in if it worked and did what it was supposed to, but it was full […]

---
I have been writing code now for close to 20 years, but for the vast majority of that, I never really paid much attention to how my code looked, how maintainable it was long term, I was only really interested in if it worked and did what it was supposed to, but it was full of bad practices and I had a lot of bad habits.

A few years ago, when I discovered the Laravel framework and started really getting into it, I started taking pride in the code I wrote, actually thinking about the code, how it looks, and how easy it is for someone else to maintain and being proud of the code I write, I always try to think of that infamous developer quote

Always code as if the person who ends up maintaining your code will be violent psychopath who knows where you live!

Some of the techniques I try to remember when writing code is primarily to not just write code for a computer to understand, this is especially important for teams, you want the next person to pick up your code to know instantly what that code is doing, and remember there’s no character limit, if your variable name is 20 characters long, but accurately described, then what does it matter?! If another person can look at a block of code and work out what it’s doing, then you know you’ve got great, clean, and maintainable code.

## Why write cleaner code

So, why should you write cleaner code, even if you’re just code your writing for yourself, well, there’s a few reasons

-   **Higher code quality** – This is an obvious one, if you’ve sat and thought about your code, and keeping it as clean as possible, then automatically, that code is a higher quality than it would have been had you just written the shortest amount of code to get the job done.
-   **More Readable** – Again, another obvious side effect of writing clean code is that your code will be far more readable and easier for a human to understand, which means it’s easier for the next person to start working in the same code base.
-   **Easier to maintain** – If your code is now cleaner, easier to read, then it will be easier to understand what that code is doing, and then it is far easier to maintain with less risks involved with not understanding any side effects of editing a specific block.
-   **Self Documenting** – Remember the days where it was encouraged to write huge doc-blocks in your code describing what it does? If your code is readable and easy to follow, then you don’t need them, your code has become self documenting!
-   **Easier to test** – This is a bit more advanced, but if your code is well structured and sticks to common conventions, such as separation of concerns, and single responsibility, then that code is then far easier to test in isolation, giving even more peace of mind about that block of code
-   **Easier for new developers to understand** – Bringing new developers into an existing, established code base can be tricky, but if that code base sticks to common conventions, and the code is clean, high quality and easy to read/understand, then it will be a lot easier and quicker for that new developer to get started, which means they will spend last time getting onboarded, and more time working on your product.

## Simple techniques for writing cleaner code

Now we’ve covered why you should write clean code, the next step is to look at some simple techniques you can start using to try and help you make the change to start writing cleaner code.

### Stick to standards, conventions, and any guidelines in your team

This is a simple one, stick to standards, such as [PSR-12,](https://www.php-fig.org/psr/psr-12/) and stick to common conventions conventions, such as the standard file structure in a Laravel app, they exist for a reason, and the more you stick to them, the easier it is for you, and others, to work in your code base, there’s some tools such as [Laravel Pint](https://laravel.com/docs/10.x/pint), [PHP CS Fixer](https://github.com/PHP-CS-Fixer/PHP-CS-Fixer) that can check and update your code so it follows PSR standards, and can even be customised to follow your own teams coding guidelines.

### Use Types

From a personal point of view, I’m pro strict types, which from writing PHP for 20 years, isn’t something that crosses most people’s minds, but they make code so much easier to read, write and maintain, and wherever possible, every class property, every method parameter and method return type is fully, and strictly typed, and coupled with [Larastan](https://github.com/larastan/larastan)/[PHPStan](https://phpstan.org/), and generic types, it just make code so much more readable and clean, and for me, more enjoyable to write.

### Use descriptive names for variables and methods

This one is key for me, and one of the best things you can start doing to improve your code quality, and also directly leads into self documenting code mentioned above, and is a very simple, quick change you can make to any code you write, take the following example.

On the left, is some code I might have wrote in the past, you might look at it in a few weeks or months and wonder what `$u` or `$p` is, especially if there’s quite a chunk of logic in the middle, compared to the code on the right, which is a lot more readable and understandable.

### Code Spacing

Another key one for me, and one that jumps out for me in code reviews, please, give your code room to breathe! Put a line break between blocks of code, just like a new paragraph in a book or an article, it makes things so much easier to read, again, using the following example.

Apart from a line break before and after the `if` block, the code hasn’t changed, but notice how much more readable the second example is?

### Return Early, no else blocks, start negative!

Now you’ve started using descriptive names for your variables and methods, and given your code a bit of breathing room, try to slightly rethink how you look at a conditional block of code, its three different techniques, but they all work well together, first, **return early**, if you’re in a function, and you can return early because the next block of code is skipped over or doesn’t matter, then do it! This will often go hand in hand with **avoiding else blocks**, it’s a tricky one to get your head around first, but if you’re returning from your truthy block in your if statement, there’s no need for the else block, because you know if your code execution reached that block, then your conditional was falsey, and finally, **start negative**, do the falsey checks first, any verification and validation, and then at the end of the method, you know everything checks out.

As always, take this example

In the first example, we first check that the user authored the post, if they did, and the post is popular, they cant delete it, but if it isn’t popular, they can delete it, otherwise, if the user is an admin, they can delete it no matter what.

In the refactor (Also with additional breathing room as mentioned above) we first check that if the user is an admin, then just return true, we don’t care about any other logic, next, if the user _didn’t_ author the post, then return false, they can never delete that post, we don’t care about any other checks, but if the user did author that post, then, finally, just return whether the post is not popular, leading to the same logic as the first example, but over less number of lines, but more readable and less complex.

### Avoid single use temporary variables

There’s little point in assigning a variable thats only ever going to be used once, even if its well named it just adds to the mental capacity needed to read and understand that code, so avoid them where possible, any good IDE will highlight any single use variables to you and suggest to remove them.

### Keep indentation to a minimum

If you’ve got a lot of nested if blocks, or nested loops, then your code can start to get very messy, especially with the indentation level, so its one to look out for, I once saw it described as watching out for ‘the flying V duck formation’, I try to not go above 2 or 3 levels of indentation if I can avoid it, it’s not always possible, but it’s a good rule of thumb to have, otherwise, your code might end up looking like this!

![](https://jump24.co.uk/uploads/2024/08/keep-indentation-to-minimum-532x1024.png)

As you can see, even without any actual logic, it just looks a mess, what brace is closing which block, consider going through the techniques discussed above, early returns, no else blocks, maybe extract some of the more complex logic into its own method to make this method so much more readable and easy to maintain.

## Summary

In this blog I’ve covered why you should write clean, maintainable, easy to read code, along with the benefits it brings, and some very simple examples of changes you could make to start making your code better, there are lots more that I haven’t covered, such as avoiding magic numbers or strings (Hint, use enums and constants), extracting complex logic or conditionals into its own method and employing the techniques above (Early return etc) which also helps with the single responsibility principle mentioned above, and many many more.
