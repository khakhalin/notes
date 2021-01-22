# Design Patterns

#oop

Parents: [[oop]]
See also: [[solid]]

A famous programming paradigm, and a name of a famous book (1994), about optimized, standardized ways to design interactions between classes and objects in object-oriented programming. About 23 or so (some sites say, 26) archetypical solutions and interfaces that software engineers learn by heart. Also aids with refactoring (minimizes, structures, and ideally - eliminates it). Also facilitates interactions between developers (a common vocab).

Are split into 3 categories: **Creational** (about how to init classes), **Structural** (about how to organize class composition), and **Behavioral** (about communication between classes).

Of these patters, some are obscure, but some are really important. The short list:
* Factory - to cleverly create custom objects of different classes, but with the same interface
* Builder
* Prototype
* Singleton - a class that only has one instance
* Strategy
* Observer
* Adapter
* Model view Controller
* Inversion of Control
* Chain of Responsibility
* Decorator
* Iterator
* Command

# Factory

Instead of a sophisticated object capable of all possible alternative behaviors, create a bunch of sister classes (probably all inheriting to an abstract class, defining an interface), and a separate "Factory" class to be called instead of a constructor, every time when a new object is needed. And then let this "Factory" (object creator) create whatever subtype of an object that you need. 

A full proper realization of this pattern, for 2 alternate behaviors, includes 6 classes:
1. Abstract product that defines the interface for products. Like `Npc`, with `Npc.move()` etc.
2. Concrete product 1, say, `Human` that inherits to `Npc`, but has its own implementation of `move()`.
3. Concrete product 2, like `Demon`, with same interface, but diff implementation of `move()`
4. Abstract creator that defines creator's interfaces. Like `Creator.new_npc()`
5. Concrete creator 1 `Creator_humans` that inherits to `Creator`, but creates `Human`.
6. Concrete creator 2: same, but for demons.

Now in a human village you can set a creator of humans, and in demon village do `creator = new Creator_demons`, but `john = creator.new_nps()` will always work, and `john.walk()` will always work, even tho in some villages john will be a demon, and in some - a human.

In practice, at least in my experience, the name "Factory" seems to be used extremely loosely! Essentially, there are several low-key versions of this pattern, like passing a type of product as a parameter `a = make_new('demon')`, or as a class method `a = npc.demon()`. It's also not necessary that products of different behaviors would actually have different types; it's enough that the creation and "tune-up" of these customized products was automated and abstracted. But the version with heterogeneous products united by a common interface is the most "full" one. 

In general, it's recommended to go for this pattern if creation of objects is complicated enough, for this fancy assortment of classes to still be a net win, in terms of code complexity. As complexity grows, it may evolve into **Abstract Factory**, **Prototype**, or **Builder**.

Footnotes:
* https://en.wikipedia.org/wiki/Factory_method_pattern
* https://refactoring.guru/design-patterns/factory-method
* https://realpython.com/factory-method-python/
* https://realpython.com/instance-class-and-static-methods-demystified/

# Abstract Factory

Same as **Factory**, but returns objects of different types.

> I'm not sure how whether these objects are supposed to adhere to the same interface; if not - then how to work with them, and if yes - then why was this interface story a part of a "Factory" description above. Can it be that RefactoringGuru site described essentially an abstract factory by mistake, while writing about a simple Factory? No idea, but parking for now.

Footnotes:
* https://refactoring.guru/design-patterns/abstract-factory

# Builder

Also similar to **Factory**, but with step-by-step creation. 

> If I got the spirit right, then both Keras and Matplotlib essentially follow a builder philosophy for their object creation, don't they?

Footnotes: https://refactoring.guru/design-patterns/builder

# Decorator

A class that is used (called?) as a wrapper around the inner class (potentially several inner classes), and that in turn calls them as needed. Often, adds some functionality. Often, has the same outer interface as the inner, wrapped class.

> I'm not actually sure whether it should say "often", "always", "sometimes", or something else. Does a Decorator have to be used in stead of the inner class? Like, is a decorator always a wrapper? Because if yes, then it always has some functionality. But then the example given by the RefactoringGuru, about creating a common point of entry for several types of notification, is not really a Decorator (it changes the interface), but rather an Adapter pattern, no?

A related term: **Wrapper**, which also wraps the inner method, but 1) definitely uses same interface as the core inner method (and so can be used in its stead), and 2) performs some additional manipulations on either inputs or outputs.

If you need to decorate not just one class, but an entire environment, it's called a **Facade**. If you're working with one class (_is it true?_), but you change the interface, it's called an **Adapter**.

Footnotes:
* https://refactoring.guru/design-patterns/decorator

# Facade

Similar to **Decorator**, but for an entire complex system, environment, package. Say, if you have a data pipeline that involves creation of several intermediate objects and what not, you may want to abstract it away into one black-box-like object that just has the job done. To hide all this inner complexity from your main code.

# Adapter

Also decorates a class, but changes the outward-facing interface, to "adapt it" for use in a higher-level class.

# Singleton

There's only one instance of this class. Should contain some protections against a creation of another copy.

# Refs

Wiki
* https://en.wikipedia.org/wiki/Software_design_pattern
* https://en.wikipedia.org/wiki/Design_Patterns

Methodical introductions:
* https://refactoring.guru/design-patterns
* https://dzone.com/articles/what-is-design-pattern

Lists of which ones are most common:
* https://medium.com/educative/the-7-most-important-software-design-patterns-d60e546afb0e
* http://www.vishalchovatiya.com/factory-design-pattern-in-modern-cpp/