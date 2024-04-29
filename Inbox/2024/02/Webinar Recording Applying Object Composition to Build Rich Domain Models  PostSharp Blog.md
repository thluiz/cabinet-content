---
created: 2023-10-30T09:08:43 (UTC -03:00)
tags: []
source: https://blog.postsharp.net/post/webinar-recording-object-composition.html
author: 
---

# [Webinar Recording] Applying Object Composition to Build Rich Domain Models | PostSharp Blog

> ## Excerpt
> How can we extend rich domain classes to support complex requirements?

---
-   [Video Content](https://blog.postsharp.net/post/webinar-recording-object-composition.html#video-content)
-   [Webinar Transcript](https://blog.postsharp.net/post/webinar-recording-object-composition.html#webinar-transcript)
    -   [Limitations of Inheritance Approach](https://blog.postsharp.net/post/webinar-recording-object-composition.html#limitations-of-inheritance-approach)
    -   [Composition Approach](https://blog.postsharp.net/post/webinar-recording-object-composition.html#composition-approach)
    -   [The Visitor Pattern and Double Dispatch Principle](https://blog.postsharp.net/post/webinar-recording-object-composition.html#the-visitor-pattern-and-double-dispatch-principle)
    -   [Difficulty with Visitor Pattern](https://blog.postsharp.net/post/webinar-recording-object-composition.html#difficulty-with-visitor-pattern)
-   [More Complex Examples](https://blog.postsharp.net/post/webinar-recording-object-composition.html#more-complex-examples)
    -   [Accumulating State of Visitor](https://blog.postsharp.net/post/webinar-recording-object-composition.html#accumulating-state-of-visitor)
-    [](https://blog.postsharp.net/post/webinar-recording-object-composition.html#h2)
-   [Q&A](https://blog.postsharp.net/post/webinar-recording-object-composition.html#q-a)
    -   [About the speaker, Zoran Horvat](https://blog.postsharp.net/post/webinar-recording-object-composition.html#about-the-speaker-zoran-horvat)

How can we extend rich domain classes to support complex requirements?

In this webinar, Zoran Horvat will show why an object composition approach is favored over class inheritance when it comes to code reuse and polymorphism.

Watch the webinar to learn:

-   How class inheritance can lead to combinatorial explosion of classes
-   What the limitations of object composition are
-   What design patterns help consume composed objects
-   Techniques for creating rich features on composed objects

<iframe src="https://player.vimeo.com/video/195774910" frameborder="0" width="500" height="375"></iframe>

[Applying Object Composition to Build Rich Domain Models](https://vimeo.com/195774910) on [Vimeo](https://vimeo.com/).

[Download slides.](http://www.slideshare.net/sharpcrafters/applying-object-composition-to-build-rich-domain-models)

[Download code samples.](http://bit.ly/object-composition-code-samples)

## Video Content

1.  Limitations of Inheritance Approach (3:00)
2.  Composition Approach (12:45)
3.  Visitor Pattern and Double Dispatch Principle (20:47)
4.  Difficulty with Visitor Pattern (36:49)
5.  More Complex Examples (40:07)
6.  Accumulating State of Visitor (43:15)
7.  Q&A (55:08)

## Webinar Transcript

Zoran:

Hello, this is Zoran Horvat speaking. I will be delivering a webinar and here with me is Alex from PostSharp.

Alex: Hello everyone.

Zoran:

Well, as you can see, this webinar is on the topic of creating rich domain models. I believe most or all of you have already suffered a lot building rich domain models, so in this webinar, I will show you one approach to that problem, so let's get started.

You may know me from [Pluralsight](https://www.pluralsight.com/authors/zoran-horvat), where I have delivered five courses this far and the sixth one is on route. The most interesting one regarding this webinar would probably be this course on design patterns called [Managing Responsibilities](https://www.pluralsight.com/courses/tactical-design-patterns-dotnet-managing-responsibilities). If you want to watch more of this, you can [go to Pluralsight](https://www.pluralsight.com/authors/zoran-horvat) and you can search my name and get into the courses.

In this webinar, we will talk mostly about the Visitor design pattern. In this course on design patterns on Pluralsight, you would find visitor and also chain of responsibility, which may also be applied to the same problem that I will show today.

The problem. Let's start with an example. I have come up with an example which doesn't require any particular domain knowledge. It has to do with animals because I suppose everyone knows a lot about animals and you will not have to learn any specific business domain to follow this webinar.

Suppose that we have a company which deals with animals. It needs to categorize them to do stuff to them. Let's see where that will lead us. For example, we could have a number of objects, each representing one animal like a cow, horse or lizard, snail, catfish, parrot, eagle and now we have to do something with these objects. Suppose that the first requirement is to categorize them somehow and what we do, what we will try to do will be to create a hierarchy of classes which is categorizing the objects based on their features so that two objects would share the same base type either directly or indirectly and through that base type, we would be able to access common features of those two objects.

### Limitations of Inheritance Approach

In the first part of the presentation, I will show you the approach with the class inheritance. You know the old saying in object oriented programming, favor composition over inheritance. Now I will show you why. Suppose we have started to categorize these animals into ground animals, water and airborne animals. Suppose that doesn't satisfy all the requirements right now. Suppose that we want to work with the mammals only, so here they are.

We can add another level of inheritance, another layer of derived classes, which only covers these two animals that we are interested in. At this level of complexity, we are quite happy. We can apply business logic that we have with mammals and that looks like this for example. We might have some object of type animal, but it's not really animal. It is some class derived from animal and now we check whether that is the mammal. If it is, we can cast this abstract animal object into a mammal object. For example, a tiger or anything else and then we can apply the domain logic, which is strongly tied to mammals.

For example, our business might pull the tail of an animal and then I don't know, run away. At this point, we are in a good situation when talking about the program. We can access a concrete feature of a concrete object, concrete class. No matter the fact that we have started from an abstract animal object. In the end, we could pick a concrete feature of a concrete class, so this looks good, but let's continue.

You know about special cases of mammals, namely whales and dolphins. Well, they are also mammals and they don't live on the ground. They live in water. If we wanted to also implement the same feature on these two kinds of animals, then we would have to introduce mammals in one additional place in the class hierarchy. If you think about this situation here, if you tried to, I don't know, map it to any real business domain that you may be working in, you would recognize a situation that you have certainly seen before.

There is this first front of classes that derive directly from the animal and at this level, everything is straight. We have ground, water, air. Everything is right, but then if you want to add one more, one additional kind of feature, not one feature, but additional aspect of an animal, then we start splitting ground animals into mammals, water animals into mammals, and we start duplicating logic.

Now, both of these would have some features that their parents do not have, like giving birth to live younglings as opposed to laying eggs. We have the feature of the mammal, another feature of a mammal which doesn't exist in any common ancestor. If you get back to this piece of domain code, then we see that it is not complete. To complete it, we also have to check whether it is another kind of mammal and then to repeat everything, only this time, with a different type.

Now, observe that this is not code duplication because for example, water mammals do not have tails. They have fins and we are not going to run, but to swim in the sea, so obviously we cannot reuse this code here because we are not working with the same type. There's nothing in common that we could use in these two examples.

Alex:

If I can ask a quick question here. What if we try to change the class hierarchy for example to fix this issue and for example, pull mammal implementation up in the hierarchy and then push what is up down and try to somehow simplify it?

Zoran:

Yes. Yes, that is a good question. The approach would be to pull the mammal up and in that way, make both of these have the same class or the same ancestor. However, if you do that, then ground and water animals would have to split under them.

Alex: Yeah.

Zoran:

We would virtually end up with the same problem, only different set of objects. Basically the problem is that we are solving a problem that we have with an inappropriate tool. The problem is that we have distinct features of an animal and we are trying to turn those distinct, unrelated features into a hierarchy which they do not exhibit and so we suffer. We suffer in a very painful way, as it says here.

We are duplicating domain logic, but we are not duplicating the code. The code is not the same because it is operating on different classes which are having no common ancestor. We can come up with more counter examples. You know about emus and ostriches as flightless birds, so if you wanted to add them, they would fit into the ground animal category, but they are still birds, so we have birds here and we have flightless birds there.

If we wanted to do something with birds again, we have code duplication. If you wanted to add even more specific mammals like flying squirrel or a bat, which are proper mammals, we would see that they are airborne animals. Or if we wanted to add a flying fish, it is still a fish, but it sometimes glides outside of the water. Here is the hierarchy of classes that is covering these features. Only this small, limited number of features of these dozen of animals, we have a dozen of classes because we have to split every single class into two or three of them to cover specific and duplicated features in every part of the hierarchy.

My point here is that attempting to solve a complicated domain problem with class hierarchy very quickly leads up into this situation in which this small hierarchy already is. Only the first layer is clean and everything under that is a mess and if you go to the second, the second layer, now you see duplication in three places, but if you get even lower, you see that even those are cut into smaller compartments because we want to know that the flying squirrel doesn't really fly. It glides. It needs wind or something to fly, but a bat needs nothing. It has full-blown flying ability.

If you want to explain an entire domain model with a class hierarchy, it is very soon going to become unbearable. You won't be able to complete it, now again, this is an example with animals. You can make it less funny by applying it to a financial market or to bank or to insurance company or I don't know, production plant and you would see numbers. I mean, dozens of special cases that you cannot fit anywhere. For example, just try to imagine amphibians. Where would I put amphibians here? I have no clue.

Now, imagine an amphibian which is a mammal who knows how to fly and you will see that there is absolutely no place where to put it in this hierarchy of classes. I have seen very large hierarchies in real business domains and they were really impossible to manage. There was so much duplicated or almost duplicated code that it was impossible to manage.

### Composition Approach

We come to the primary topic of this presentation in which we are going to talk about a different approach to solving the same problem. It is composition. We are not going to inherit classes. Any of these animals is not going to be every of these ancestor classes. It is going to be just an animal and now animal is not going to derive from whatever. It is going to contain features, so we had a special kind of feature which is classification. It is biological classification and we had mammals, birds, bony fishes or reptiles or gastropods. That is a snail.

This diagram says that animal is going to contain an instance of a class deriving from classification. It is going to contain an object mammal. Let's see another. We had environment in which the animal is living, so it could live on the ground, in the water. It could be seen in the air. We would add an object of environment, even more, we could add multiple objects because some animals, like amphibians could be seen on the ground and in the water, no problem.

Some others like birds are living on the ground and sometimes flying in the air, no problem. Two objects again. Even more than that, we could refine some of these objects and say, there are two kinds of water. Not every fish is living in the fresh water or in the salt water. The third aspect of those animals that I had were their abilities. They could walk or run if they are on the ground. They could fly. Now, fly could come in two flavors, gliding or full flight. Swimming could be even more complicated.

Now, as you can seem the animal could also have multiple abilities. There's nothing to prevent that. As you can see, I have split that large and cumbersome hierarchy into three distinct, smaller hierarchies and none of these hierarchies alone is exhibiting any of the problems of the previous one because each of them is dealing with only one aspect of an animal, and you can always add multiple objects if you want to represent multiple abilities or environments or whatever.

Now, these hierarchies can also start growing and become not easy to manage, like abilities are very complicated right now. If that happens, then just apply composition to the abilities again. You could have, I don't know, ground abilities, air abilities, and water abilities like a product between these two hierarchies and you could again, compartmentalize the object and the composite from multiple smaller objects that would explain it closer.

This looks like a solution, right? And it is really not. The problem here is now the animal class is encapsulating its traits. We don't know what is inside and if you follow the encapsulation principle, which is a very important principle in programming. Okay, I will give a sentence on that just after. If you follow encapsulation, then we are in trouble we don't know whether this animal is a mammal. We cannot see that. If animal broke encapsulation and let us access its classification directly from the outside, then we come to the reason why encapsulation is good. Animal can never change its implementation of classification ever in the future because somebody's depending on a concrete implementation of this feature.

That is why encapsulation is good. We are keeping the right to change the way animal finds out who it is. For example, this animal could contain an object of classification, but also it would contain a reference, a dependency on some classifier. Something, I don't know and whenever we ask the animal what class you are, what biological class do you belong to, that animal object could contact its dependency and say, now, look, somebody's asking. You tell me, who am I? That object would tell it's a mammal or a bird or whatever and the animal would return that. That is what we get if we preserve encapsulation. 

We need some, I don't know how to say, a good, legal, legitimate way to poke into the animal object without violating its encapsulation. One of the ways to deal with that is the visitor pattern.

Now, before explaining the visitor pattern, I will show you what these animal objects will look like, so that will give you the idea, if you're already familiar with the visitor, that will give you the idea what we will try to visit. If you have a cow, its class is mammal, it's environment is ground. It has two abilities, to walk and run. The horse is the same.

Emu and ostrich are are almost the same, only they are birds. Now we can have about lizards, we can walk. We can have snail who is gastropod and I don't now how to call its movements, whether they are walk. Now, whale and dolphin are mammals. Now you can see an example of what was almost impossible in that hierarchy, the approach with the inheritance. Now, they are also mammals, as cow and horse, but they share nothing else with them. They dwell in water. They can dive, live under water, anything, so there's no resemblance with cow and horse.

That is exactly what I wanted to achieve with the composition. That is something that you cannot do with inheritance. We have, I don't know, two kinds of fish, birds and flying squirrel and a bat, who also share a little of common features. We have this table which is explaining how we can construct an animal. We can construct a lizard by telling that its biological class is reptile, that it lives on the ground, which means give it an object of type reptile, give it an object of type ground, give it an object of type walk and then the lizard will be able to walk. And so comes the visitor. 

### The Visitor Pattern and Double Dispatch Principle

Here is a principal idea of the visitor pattern. There we can talk about a hierarchy, like these abilities, running, flying and swimming. Now, take these three concrete classes. They will have to do something. What will they do? For example, we might say that ability is defining an interface use. Like run, okay, start running. Or fly, start flying. Or start swimming. This is the common feature of all elements of the hierarchy.

However, after a while, we come up with a different idea that abilities might sometimes be suspended. Like I could break a leg and not be able to run. Ability might also have a suspend feature. Alright, we implement three more functions here and we are fine. Now we have six functions in total. However, later on, we come with another idea, advertise an ability. Like in the mating season, you can see someone running and jumping around with no obvious reason, so it would not be the same as use, but it will be, I don't know. It could still run, fly or swim, but in a completely different setting. Where does this lead us? If we have built this hierarchy of classes, we don't want to change them every now and then.

If you have this hierarchy, which might be overwhelmed with new features that are using them, then adding all those features to those classes might not be the best idea. Instead, we could turn this hierarchy or view it sideways, like let's see using feature, like something that develops here and then let the using feature use the run ability or use the fly ability or use the swim ability. The features are becoming classes and these are becoming their arguments. That is how we come to the visitor.

Some ability of visitor who'd visit the ability class and the class would tell, "Okay, now use me. Do whatever you do to myself, to this object." This class, AbilityVisitor would now have one method for each element of the hierarchy. Do you see how the things, they are skewed by 90 degrees? You can see that now we have one visitor method, but in three flavors, run flavor, fly flavor and swim flavor. Each of them would implement an entire ability for each of the classes in the hierarchy.

Now, when I said that the problem with the original design, where ability based class had all three methods is the problem when we add new features, then we have to add them to each of the classes. It's obvious that visitors are suffering the same kind of problems. If we add a new class, then we have to add a new visit method. It is not really better than the previous solution. It is just a different view on the methods.

We come to concrete visitors. Like useVisitor or suspendVisitor or advertiseVisitor. Those are the three visitors, which are implementing the three functions, the three features, but each of them is implementing an entire feature for an entire hierarchy, each type in the hierarchy. How does the visitor work? We take some ability. We don't now what concrete type this is, ability object and we call accept visitor. This is still something that is derived from ability visitor. In a concrete override, override of the accept, all these three members of the hierarchy would have to override the accept method.

In each of them, we just say accept this, visitor you are, accept me. Now guess what? Since this class is concrete, this will statically link to this method, visit here, visit of run because we are in the run class. Accepting method would look the same in the fly, but it will call visit of fly instead. This is the basic principle of the visitor.

It is called double dispatch. We have one dispatch to let two objects meet each other, like concrete object derived from this class, meeting an abstract visitor and then the concrete visitor, this one. It must be something concrete, is meeting concrete object run and that is how these two objects finally find each other and then you can implement the feature on the run object, on a strongly typed run object. That is the major benefit that comes with the visitor design pattern. That is the major benefit and now you know everything else is probably drawbacks.

As any design pattern, this looks deceptively simple. This looks like you implement entire universe by applying visitor pattern to everything. However you don't. I already said we have the same problem with added classes. If you have a dynamic hierarchy, if you have interfaces here and there, which are indeed, you don't know who's going to implement them, this is not going to work. However, when it comes to those animals, I will show you the situation in which the visitor can be applied and it will be applied pretty well.

The rest of the demonstration will be code and let's get started. This is the animal. The animal class is having a public name, but it doesn't do anything. More important parts are private. One classification, a list of environments where it can be seen and the list of abilities it has. I mean, the constructor animal requires name, classification, and one environment. Later on, we can add new environment. We can add as much environments as we like and we can add as many abilities as we like.

These are the animals. This interface is letting me compose an animal, compose it from its features. I have prepared this static utility class in which I have shown you how you can actually compose a cow. Its name is cow. It is a mammal. It has an object of type mammal inside of it. It has an object of type ground inside of it. It has the ability to walk, which is an object walk and another object of type run, so it can run. That is the cow. You have horse, emu, ostrich, everything. All the animals are here.

Now, of course you would never construct objects in this way. I have just tried to mimic a repository of animals. The only really important member in this utility class is this static property which is getting all the animals. You can imagine that this is the day and you have loaded all these objects and attributes from the database and constructed each of these animals. This will be the entry point for us. I have a sequence containing all the animals. Those are all these animals from the diagram plus a salmon, which I have added because it is going to provoke a bug and I will show you that in a minute.

Let's try to use these animals, all animals to perform a couple of tasks. One thing will be to find all the mammals and say hello to them. This is what I want to do. I want to iterate. The example is obviously in C#, but I think I haven't used any specific features of the language, whatever your native language is, you should be able to follow the example. Here I'm iterating through the collection of all animals and this and this.

Now, let's explain this. This is the visitor. It is a visitor which says hello to mammals. How does it do that?

This is the classification visitor. Now, a word of warning: if you have been using visitor before, or if you have learned it from the Gang of Four book or any other book on the subject, you didn‘t probably not see the implementation like this one. I don't like to tie too much to any concrete implementation of any concrete design pattern. The point here is to recognize what is the core of the pattern and the core of the visitor pattern is double dispatch mechanism. That is the pattern also called double dispatch pattern. If you remove everything else from any explanation of the visitor pattern, including that diagram that I've shown you, this implementation is not equal to that diagram.

Just as a demonstration, how much I do not want to tie to any concrete implementation of any design pattern. If you strip off everything else and only leave the double dispatch mechanism, then you are suddenly free to implement the design pattern in a way which is just right for your problem at hand. One of the problems that I had was that I couldn't find common ancestor for the two classes. You will see in this implementation that now we can visit a common ancestor of two features of an animal. It will be like magic.

Now, the difference between this implementation and what you could find in literature is that now I also have visit base class. Not only five concrete biological classes, but also visit the base. If you want to just see whether an animal has a classification or it is, I don't know, some alien, then you would listen to this, not to any of these.

Okay, now concrete visitors, this is the classification. This is the classification visitor there. You can see that the classification, abstract class has a virtual, which is overridable method accept. It is accepting any classification visitor and it is calling its visit this. If I choose to go to the definition. Where is go to definition here? Here it is.

It will go to concrete visit of classification. None other but this concrete one. Now, let's see more. Classification for example, as a descendant mammal. Mammal is derived from classification. It overrides accept and then says, base, do whatever you like, which means that we will end up in this method first, but then visit me and I am a mammal. This ends up. Look where it is if I go to definition. I'm visiting concrete visit method. I'm invoking concrete visit method, which accepts mammal, so it goes to concrete classification visitor.

Now, I want to say hello to all the mammals, so do is to accept an animal that I have to visit and then in this public say hello method, I say, "You animal, whoever you are, accept me, concrete visitor." Then the animal will do the magic. Let's see. This is the animal class. It says my classification, you accept this visitor. We get to a concrete classification and it might be a mammal. If it is a mammal, it will again call this visit of mammal, but not in the base visitor, but in the concrete visitor.

I don't know if you could get a grasp of what I did. This looks like magic, but that is the basic double dispatch principle, so we had concrete classification object and we have concrete classification visitor, so in the end, concrete visitor will meet concrete classification. If we ever get into this method, that means that the object animal had an object mammal inside of it, which means that we can say hello to it because it is a mammal. If we do not get into the visit of mammal method for this animal target, because we are visiting that object, it's not a mammal. That is how a visitor pattern works.

### Difficulty with Visitor Pattern

Alex:

Okay, so if I can just maybe have a quick question. First there's a note that we actually have a number of questions from our listeners, but we will answer them at the end of presentation, so please keep asking questions. We will read them and answer then, but regarding the question I had, I think you already touched upon it already in the slides. I noticed in the base class, you basically have a method for every classification. We have classification visitor, base class. It means you need to maintain this class when you are adding a new classification subclasses, right? This is some negative part of that or that we need basically to add method every time you-

Zoran:

Yes, yes. I have only touched that question and here's the good point to show the consequences. If you add another type to the classification hierarchy, we must add one more method like this for that new type, which has a couple of drawbacks. One is that we have to change a class which is not really related to the new type, which is bad. The other important aspect is that now everybody else would have to be aware of the new class, but I have made that a little bit easier, by making these base implementations actually have no implementation. These are not abstract, so nobody has to override them. Only who is interested in.

Zoran:

If you take this mammals one, it overrides one of them and then the next result is that if you add a new class and this visitor knows nothing, it has no business with that new kind of animal or new kind of classification. We won't have to change this concrete visitor. It is somewhere in between.

Alex: Yeah, yeah. You don't need to change all the classes.

Zoran:

That is the major difficulty with the visitor.

Alex: Okay, thank you.

Zoran:

Thanks. I will run this and you will see that it will actually, if you see. I hope the font is not too small, you will see that I'm saying hello actually to all the ... Oh, this is terrible. Let's go back. Alright, so I have picked cow, horse, whale, dolphin, flying squirrel, and bat. All those objects from distinct parts of that class hierarchy from the beginning are now in the same bag and they have been recognized because each of them had a mammal object inside of it and none other animal object had the mammal object inside of it. That is how the visitor works.

## More Complex Examples

Okay, I will show you more complicated things. Now, I want to say hello to all the animals under the sea, so that is a completely different thing that is environment and I have environment visitor, which is visiting all of these kinds of environments and now I have a lot of water creatures, which is catching, you see who? Water. Now, water is derived from environment, but it has salt descendant and it has freshwater descendant. Now, I don't want to have to override visit freshwater and then separately have to visit saltwater. I don't want to have to duplicate logic in two places just because there are two kinds of water.

I want to catch their base class and then visit that. That is the change that I made to the original visitor pattern in this example. That is why saltwater for example, when it accepts a visitor calls, this is another implementation, just a different way to call the base, no problem. It calls visit with this as water, as a base class and then visits this as the saltwater, so we will have two invocations on the visitor for the same object. Now, let's see how it works.

I'm using the same sequence of operations. I'm just creating a different visitor, like water creature visitor, giving it each of the animals and to each of them, trying to say hello. What will happen? Now we have whale, dolphin, flying fish, cat fish and two salmons, so we said hello to the salmon twice because if you take a look at the salmon animal, it has been ... In the animals, it is somewhere here. Salmon. Look, it has two environments, freshwater and that is one and the other is saltwater because it lives its entire life in the sea and then goes back to the river.

I said the salmon, hello twice. 

### Accumulating State of Visitor

There is one very interesting feature of the visitor pattern. It is called accumulating visitor. I will show you that in the next example. Accumulating visitor does not do its operation as soon as it can, but it accumulates an object which it has visited. I will show you on the abilities visitor. You will see that. Let me just find any of the abilities. Oh, I cannot find.

Oh, here it is. This will be the next exercise. When we want to take a picture of anything that flies. Now, we have two kinds of flying. We have gliding and full flight. I'm visiting the base class again. It is fly, which derives from ability, but that class has two descendants. One is glide, as you can see and another is full flight, so if you take an animal which can glide and fly, which knows both things, then we would again visit it twice. Well, now instead of taking a picture of this animal here, I will accumulate it in this private property and just keep going. Then I will wait it again when I see the same, when I see another flying ability in the same object, in the same target. I'll override this again. I will override it as many times as it gets into the visit fly, but after the fact.

Here's the take picture method. After all the visitor methods have been unfolded, I will remain with this recognized target, which is either null, if I didn't recognize any flying ability or not a null. If it is not a null, then this animal has at least one flying ability and I can take a picture of it. I hope you followed this. It is another piece of convoluted logic. Not easy, not quite easy to follow. However, this is the example which is doing precisely that, and I will take a picture of each of the flying animals, including mammal bat and the squirrel which is also a mammal and a fish and two proper birds.

I have found all kinds of flying abilities in all of these animals that I have. Now, there's another exercise which is telling all the mammals to run. Now, let me show that very quickly. I think you have already got grasp of the basic idea. I have scare mammals visitor, however, when it visits mammal, it uses another visitor to scare it, which I must explain what it means to scare an animal. It means to make it run, so there's no point in scaring an animal which would stay in place, like scaring a snail.

Scare animal is different. It is ability visitor, which is trying to see if the animal can run and then you know what it does, it calls a feature of the run object because in this object, in this method, we have a strongly typed run object and we can use the feature which only the run class has in this entire system. It's just going to say that it is now running. You will have the opportunity to download this source code and that will help you take more time to see all this convoluted logic.

I will just run this example and show you that really, the task was to scare mammals, so I have found a cow, a horse and a flying squirrel. Those are all the mammals that can run. I didn't try to scare away, oh, because it doesn't run. This would be two level visitor, so to call it.

Alex:

Before you go to the next example, I can also ask a question because this looks interesting. This scare mammals visitor, it looks like it instead can be implemented as a chain instead, so if you have a concept of chain of visitors, you can say the first visitor will say, "Filter out mammals." Then you just have another visitor and that one will for example, tell them to run and so you can dynamically build the chain of visitors.

Zoran: Yeah, that question is funny because that is the next task.

Alex: Okay.

Zoran:

I'm doing precisely that. Yes, this scare mammals visitor which is tightly coupled to scare animal visitor, which is then working on the abilities, this code is very hard to follow. It's very hard to understand how it works and why it works. It is basically ugly if you have to construct all these objects and match their types. It is hard to manage. If you could, I don't know, close this, encapsulate this convoluted logic into something which is easier to use, that will be great. Here's what I have prepared for you. I have prepared a solution, which says, all animals of classification mammal. I believe this looks much better than using the visitor.

Now, in this example, I just want to join all the mammal names and to print them out. Let's just find mammals. This Ofclassification is an extension method on the sequence of animals. Internally, it is using some filter that I have made. Now, classification filter is the new kind of visitor that I made and it is visiting classification, some type, which is derived from classification, which I don't know in advance. It is a generic class.

Then this concrete visitor is expecting this classification and then checking if that has a runtime type, which is equal to what I expect. If it does, then add it or construct the result, which contains only that animal, which I am visiting. This is extremely ugly code and I don't ever want to see it in my custom code. However, this is the utility class and it has a great value because it can hide this class and this Enumerable extensions class which is using it. They're really ugly, but they are utilities that I will write now, test now and never touch again.

I won't have to rely on any visitor which is just looking for a classification of an animal. This is done forever. An interesting aspect of this solution is that it has ultimately given up the idea of the visitor because I am receiving the type T, which I don't know what it is and I'm not visiting any concrete classification, so you could even I don't know. This is not the visitor, although it derives from the visitor class, but now, you can see how good it is to have something like that because when I have all the animals, I can say of classification mammal and it will return me. You can see, it will return a sequence of animals, but now I know that only the mammals will be in that sequence and it really looks like magic.

I can pick names of all those animals that I have got back and those will be the names of mammals. Let's run it and it will be correct, you see, only cow, horse, whale, dolphin, flying squirrel and that in a chained sequence of calls. The last example in this presentation will be the same thing, only to scare the mammals. Now I have a filter of mammals. I have another extension method which is using an ability filter. I'm looking for the ability to run, which is ability of T. I don't know which ability it is. It works exactly the same as the mammal filter or the classification filter. It is the same idea behind and the same run time implementation. At this point after all classification, I still have a sequence of animals.

Now it comes with use ability. I want to use ability run and this method is asking me to give a lambda, which receives an animal, which would run and the run object. Guess what I am doing? I'm telling that now I'm scaring away this animal. I have access to concrete animal object which can run and ease mammal and I can use its own internal feature to make it run.

Now, this is magic. This is something that is not violating encapsulation. It has those problems if you imagine a new kind of animal. Alright, it has its limitations, but this is really magic. I can run this and it says, "Alright, cow, horse, and flying squirrel." Each of you run and they say, "We started running," so this is obviously working and it is certainly readable.

I would stop the presentation at this point. I hope you have enjoyed it. Now you can ask a few questions and all the questions that we cannot answer right now will be answered in textual form after the webinar.

## Q&A

**Q: Is it possible to implement the same interface called I mammal? This is regarding mammal and water mammal and when you tried to cast to each of the concrete types. This is about user interfaces in this hierarchy of animals.**

A: Yes, that is a good question and that is what many people actually do and let me disappoint you. They do that to postpone the disaster in fact. Here's the problem. Basically the question is related to single inheritance languages and all this presentation was in one of those, C#, and the same stands for Java. Now, if you do implement interfaces on classes that are not related to a common ancestor, you can do that, but then you have to implement the same logic twice in them. For the first point is that you still have duplicate logic, only it has moved to classes. You're not going to reduce your code.

The second thing is very often, you do not have an interface which is the same for two distinct members of the large hierarchy because doing stuff to objects may involve a sequence of calls, calling protocol to complete an operation. Well, that calling protocol is not going to be the same in one and the other case and you will actually not have the common interface. It might get you to some extent and then you will get in a quick sand actually. You won't be able to implement more features.

**Q: Why do you show such unreal examples? No serious developer would create object in property getters?**

A: Yeah, because I didn't have the database under. I said I have just constructed objects in those getters to have them. You can ignore all those property getters and only view the last, the last one which returns the sequence of them all. That is the only one which I actually used. I know it's not realistic, but it resembles a repository for example.

**Q: How do you persist encapsulated domain entities to database without breaking encapsulation?** 

A: Yes, you have many problems if you try to persist a domain model when domain model becomes complicated. If you have started, if you have got to the point where you cannot extend your huge domain model anymore, then you have already been suffering a lot with persisting that model before. The answer is that probably at some prior stage, you will already separate domain model from persistence model and then persistence model would break encapsulation and let the data out and domain model would try to only expose methods.

**Q: How to control if visitor always matches concrete object and there is no implicit conversion?**

A: Visitor is designed to match concrete objects. If there is the need to support out-of-band objects, like objects from one hierarchy that can be converted to objects from a different hierarchy, then the compiler will have hard time trying to understand which method of the visitor to invoke.

Major selling point of the Visitor pattern is that it makes obvious to the compiler which method to invoke in every possible situation. The variation – which concrete implementation of a method to invoke – has been turned into an object, a visitor object. That makes it possible to reach enormous flexibility at run time, while remaining within premises of strong types all the time. Not so many designs can do that.

**Q: What happened to simplicity? Seems like you are a making an alternative to LINQ-to-Objects to avoid having to deal with a solution, that you call ugly. It seems over-complicated.**

A: There are no elegant solutions in complex domains, and certainly no simple ones. It’s only different places to hold complexity. The goal of the game is to make client code simple.

In Visitor pattern, concrete visitors will encapsulate complexity. Chaining calls approach has only saved the caller from having to know the visitor protocol.

One alternative was mentioned by a viewer: Make classes with no common base class implement common interfaces. This is a viable solution unless distinct classes come with different method dependencies and different calling protocols. At that moment, common interface idea is off.

Another alternative is Chain of Responsibility pattern, which I have applied to this same problem in the Tactical Design Patterns in .NET: Managing Responsibilities course on Pluralsight. It works nicely, but fails to differentiate subtle cases, like what happens if an object contains more than one component of requested type.

The worst options are bare hierarchy of classes and public composition (in which the object exposes its content). Class hierarchy promotes code duplication. Public composition prevents future maintenance of the composed class.

**Q: Would it be possible for OfClassification<T> method to return an IEnumerable<T> instaled of IEnumerable<Animal>?**

A: Classification objects do not reference their containing Animal – that was the design decision. It might be better to expect OfClassification<T> to return IEnumerable<Tuple<Animal, T>>, which then gives entire information to the consumer.

Another question here is how will this method behave if one animal object contains multiple classification objects of the same type – will it return multiple records then? That is one of the difficulties in approach I have shown in the webinar.

**Q: Why not use dynamic to implement double dispatch instead of the visitor pattern?**

A: Because you don’t know whether the dynamic object will even have the desired feature (like the Start() method which only exists on the Run class). Fundamental feature of double dispatch mechanism is that it lets two concrete objects meet each other.

**Q: How to apply the Visitor pattern and composition altogether with dependency injection?**

A: Visitor can be injected using common DI techniques. It boils down to selecting a concrete visitor, which is exactly what DI frameworks are good at. There are subtle issues here, though, but they are not hard to resolve. For example, I have requested target objects in all visitor constructors. That would have to be redesigned to fit DI, but as I said, it’s not too hard.

Composition does not fit the DI pattern since there is no object that implements certain type. It is rather an object which is constructed in certain way. We could argue that composite objects are closer to the Builder pattern which codifies the process of building a complex object. In that respect, IoC container (which implements DI) would rather resemble Abstract Factory, which is of lower complexity level compared to the Builder.

**Q: How do you persist encapsulated domain entities to database without breaking encapsulation?**

A: The persistence problem is not related to application of Visitor pattern, or object composition, as it exists in class hierarchies as well.  
There are two levels at which we can attack it. One is to use ORM which can access private data members of a class. Entity Framework and Hibernate can both access private members. You can use that approach to some extent.

In really complex domains differences between OO model and relational model are significant. When you see that persistence is affecting domain model in adverse ways, it is probably time to separate domain model from persistence model. You can then apply a mapping framework to convert proper OO model into flat persistence model and vice versa.

**Q: Can we call this pattern in borderlines of SOLID principles?**

A: Depending on application, Visitor pattern may be seen as enforcement to SOLID principles, or as an adversary. Here are some guidelines on that:

S – Single responsibility – Each concrete visitor deals with only one feature. Visitor pattern enforces SRP more than the original hierarchy of classes which were doing more than one thing each.

O – Open for extension, closed for modification – If you have a fixed hierarchy, then you will never have to modify abstract visitor. That is the primary niche of the Visitor pattern. It is natively applicable to fixed hierarchies – like classes of animal species, or types of bank accounts, etc. It also supports easy extension, because extending the system boils down to implementing new concrete visitor.

L – Liskov substitution principle – Methods of original concrete classes are now moved to concrete visitors. LSP is violated if a derived class adds method preconditions. Therefore, if original classes had new preconditions added, then concrete visitors hill have to add them too. Conversely, Visitor pattern will violate LSP to exactly the same extent as original classes did.

I – Interface segregation – Original class can implement one interface per concrete operation. With Visitor pattern, each concrete visitor would represent one such interface. Segregation then boils down to selection of a visitor object, which is even more flexible than compile-time interface segregation. This means that concrete visitors will satisfy ISP at least to the same extent as original classes did.

D – Dependency inversion – Since each concrete visitor is a separate class, and all visitors share the same base type, it is possible to inject them as polymorphic dependencies. That is the premise of inversion of control principle. Also, this was the case with original classes which had the same feature. Therefore, DI is supported by both the class hierarchy and by the corresponding hierarchy of visitors.

Conclusion is that the only true difficulty with the Visitor pattern is when the hierarchy of classes is not stable. If we have to add new classes to the hierarchy, then we will have troubles implementing and maintaining the hierarchy of visitors.

**Q: How will be the application performance with this approach when we need create several methods?**

A: From source code perspective nothing will change. Solution will contain exactly the same number of methods, only they will be moved to different classes.

From run time performance point of view, there is only a negligent penalty. Where one virtual method used to be called, now we will have two virtual methods called. That does not give sufficient motivation to consider performance loss due to application of the Visitor pattern.

### About the speaker, Zoran Horvat

![Matt Warren](https://blog.postsharp.net/assets/images/blog/2016-12-15-webinar_recording_object_composition/zoranhorvat.jpg)

After fifteen years in development and after leading a dozen of development teams, Zoran has turned to training, publishing and recording video courses for Pluralsight, eager to share practical experience with fellow programmers. Zoran is currently working as CEO and principal consultant at Coding Helmet Consultancy, a startup he has established to streamline in-house training and production of video courses. [Zoran's blog](http://codinghelmet.com/).
