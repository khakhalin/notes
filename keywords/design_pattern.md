# Design Patterns

Parents: [[oop]]
See also: [[solid]]

#oop


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
* Command processor

# Factory

One sentence description: Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.

Instead of making one sophisticated class capable of all several alternative behaviors, create a bunch of sister classes that share the same interface (probably all inheriting to an abstract class, to define this interface), but with potentially very different implementations of these methods. As now you cannot graciously init them via a standard init (each class has its own init, right?), you also need a separate "Factory" class to be called instead of a constructor, every time a new object is needed. This "Factory" class (object creator) will then create whatever subtype of an object that you need. 

> Note: I have a strong suspicion that the RefactoringGuru presents an abstract factory as a factory, by drawing a line further in. Most C++ and JavaScript 

A full proper realization of this pattern, for 2 alternate behaviors, includes 6 classes:
1. Abstract product that defines the interface for products. Like `Npc`, with `Npc.move()` etc.
2. Concrete product 1, say, `Human` that inherits to `Npc`, but has its own implementation of `move()`.
3. Concrete product 2, like `Demon`, with same interface, but diff implementation of `move()`
4. Abstract creator that defines creator's interfaces. Like `Creator.new_npc()`
5. Concrete creator 1 `Creator_humans` that inherits to `Creator`, but creates `Human`.
6. Concrete creator 2: same, but for demons.

Now in a human village you can set some "creator object", or even just "factory method" of your "environment" (village?) object as a creator of humans, while in a demon village you would set it as a creator of demons (like `creator = new Creator_demons`). And so in every village commands like `john = creator.new_nps()` will always work, and later `john.walk()` will also always work, even tho in some environments john will be a demon, and in others - a human.

In general, it's recommended to go for this pattern if creation of objects is complicated enough, for this fancy assortment of classes to still be a net win, in terms of code complexity. As complexity grows, it may evolve into **Abstract Factory**, **Prototype**, or **Builder**.

Footnotes:
* https://en.wikipedia.org/wiki/Factory_method_pattern
* https://refactoring.guru/design-patterns/factory-method
* https://realpython.com/factory-method-python/
* https://realpython.com/instance-class-and-static-methods-demystified/

### Confused comments on Factories

Here's what I don't understand. Is it necessary to create 2 concrete subclasses for an abstracted Factory interface? Can't there be just one class that decides which subclass to init? Or is considered a shameful half-measure that is not worthy of a real programmer?

I imagine something like that, with 4 classes, instead of 6:
1. Abstract product that defines the interface for products. Like `Npc`, with `Npc.move()` etc.
2. Concrete product 1, say, `Human` that inherits to `Npc`, but has its own implementation of `move()`.
3. Concrete product 2, like `Demon`, with same interface, but diff implementation of `move()`
4. A creator that is called with `Creator.new_npc()`, and then decides whether to make a `new Human()`, or `new Demon()`, and returns the result.

The client (some higher-level class) now gets an `npc`, and can do `npc.move()` on it, without really caring whether it's really a demon or a human.

Some (but a  minority) of descriptions online actually kinda seem to describe this case (where product is abstracted but factory is concrete, and is just called in different ways to create different products), instead of a full case (where there are also many concrete factories, for every concrete product). Not sure what to make of it. See for example:
* https://stackabuse.com/the-factory-method-design-pattern-in-python/

# Abstract Factory

Same as **Factory**, but this type Factory is for sure abstracted (in the sense that you create a factory object that follows a factory interface, but that can actually be an object of one of several alternative concrete classes, depending on the context). And then instead of creating  just one object, this factory can create many different objects, of different classes, united by some common property. Or  a collection, or a hierarchy. Say, not just an npc, but a matching house, and a matching outfit, and what not. The interactions between these objects (interfaces) are all standardized (by abstract classes), but the innards may be quite different.

Footnotes:
* https://javacurious.wordpress.com/2013/03/15/factory-method-vs-abstract-factory-again/
* https://refactoring.guru/design-patterns/abstract-factory

# Builder

Also similar to **Factory**, but with step-by-step creation, and with different products, including of different types. One cool example is that the same sequence of commands (calls to methods, defined by an interface) can create a car, and a manual for this car. Just if you call this commands on the car itself, you build the car. If you call same commands on a manual, you get a description of how this card is being built. But it ensures that the description of the car exactly matches the car, as they would be passed same parameters (number of this, color of that, etc.)

Footnotes: https://refactoring.guru/design-patterns/builder

# Decorator

A class that is used (called?) as a wrapper around the inner class (potentially several inner classes), and that in turn calls them as needed. Often, adds some functionality. Often, has the same outer interface as the inner, wrapped class.

> 🔥 I'm not actually sure whether it should say "often", "always", "sometimes", or something else. Does a Decorator have to be used in stead of the inner class? Like, is a decorator always a wrapper? Because if yes, then it always has some functionality. But then the example given by the RefactoringGuru, about creating a common point of entry for several types of notification, is not really a Decorator (it changes the interface), but rather an Adapter pattern, no?

A related term: **Wrapper**, which also wraps the inner method, but 1) definitely uses same interface as the core inner method (and so can be used in its stead), and 2) performs some additional manipulations on either inputs or outputs.

If you need to decorate not just one class, but an entire environment, it's called a **Facade**. If you're working with one class (_is it true?_), but you change the interface, it's called an **Adapter**.

Refs:
* https://refactoring.guru/design-patterns/decorator

# Command processor

Think of a GUI system. In principle, you can make all GUI elements that lead to the file being saved (button, menu, shortcut) direction call some `project.save()` method. But it feels wrong for several reasons, including that the inner works (project) is exposed at GUI level, and that you'll have to change this hard-link in several places if something changes about the implemention of this action. An alternative is to create a class that will process commands, and send commands to this class. Added benefits include that now you have logging, you have macroses, and you have undo functionality (as you can log commands).

> In Java situation, the command is expected to be a class itself (objects of a "Command" class), but my assumption is that in Python, you probably want commands to be actual strings?

Refs: 
* https://en.wikipedia.org/wiki/Command_pattern
* https://www.dre.vanderbilt.edu/~schmidt/cs282/PDFs/CommandProcessor.pdf

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