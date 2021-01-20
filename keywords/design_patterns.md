# Design Patterns

#oop

Parents: [[oop]]
See also: [[solid]]

A famous programming paradigm, and a name of a famous book (1994), about optimized, standardized ways to design interactions between classes and objects in object-oriented programming. About 23 or so (some sites say, 26) archetypical solutions and interfaces that software engineers learn by heart. Also aids with refactoring (minimizes, structures, and ideally - eliminates it). Also facilitates interactions between developers (a common vocab).

Are split into 3 categories: **Creational** (about how to init classes), **Structural** (about how to organize class composition), and **Behavioral** (about communication between classes).

Of these patters, some are obscure, but some are really important. The short list:
* Factory
* Builder
* Prototype
* Singleton
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

Instead of a sophisticated object capable of all possible alternative behaviors, create a bunch of sister classes (probably all inheriting to an abstract class?), and a separate "Factory" class to be called instead of a constructor, when a new object is needed. And then let this "Factory" (object creator) create whatever subtype of an object that you need. Like, instead of `npc = new Npc`, do `npc = Demon()` (where `class Demon(npc):`etc.) Or maybe even`npc = new_npc('demon')`, having`new_npc` check what the request was, and create a matching `Demon` object. And then you can totally do `Npc.walk()`, and instead of having one method that can walk and knows whether it's a Demon or not, you'll either actually call for `Demon.walk()` method, or some other `.walk` for another tpe of npc, inheriting to `Npc`.

Footnotes:
* https://en.wikipedia.org/wiki/Factory_method_pattern
* https://refactoring.guru/design-patterns/factory-method
* https://realpython.com/factory-method-python/

# Decorator



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