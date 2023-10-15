# SOLID

Parent: [[coding]], [[oop]]
See also: [[design_pattern]]

#oop


SOLID is an acronym for 5 core principles of OOP:
1. SRP: Single-Responsibility - when a single task changes, you need to only update one class
2. OCP: Open (for extension) Closed (to modification) - about abstracting interfaces
3. LSP: Liskov Substitution - a derived class cannot refuse to support behaviors of a parent class
4. ISP: Interface Segregation
5. DIP: Dependency Inversion

There's also an alternative set, called GRASP :) https://en.wikipedia.org/wiki/GRASP_(object-oriented_design)

# S. Single Responsibility (SRP)

Formally, it's about what every class is responsible for, but ultimately, it's about when the code for a class is needed to be changed, as all of these principles are ultimately about coding and refactoring optimally. Aka **how to draw borders**.

Ideally, for a programmer, **there needs to be only one legitimate reason to change each class**. Say, if you load something, process it, and output it, you need to build your code in such a way that when loading changes, you only update one class. When saving changes, you only update another class. It's like non-repetition of code, but abstracted one layer up. If every time you save data you need to do some prep work, and this prepping is scattered across the code, this code will be a nightmare to update. On the other hand, if saving is managed by only one class, and all other code talks to it through a good interface, then a change from csv to SQL or whatever will only require your changing this one class, not the entire codebase.

Related patterns: **Facade pattern**, **Chain of responsibility pattern**

# O. Open-Closed (OCP)

Class should be **open to extension** without a need for modification (aka **closed to modification**). You need to be able to add new behaviors without rewriting earlier code. Leading to an important decision of **which parts to abstract early on**. Essentially, if, when adding a new behavior, you need to rewrite something in the class, this principle was violated.

A classic example is a method with a long "switch case" inside, doing different things depending on what type of an object it receives. Which means that every time you add a behavior, you have to make this class larger. Instead, consider this top-level class calling some method which is shared across all types of objects (inherited from an abstract class), and then in each class describe how this abstract methods needs to be implemented. Now if you're adding a new class, you're only adding new stuff, you don't have to change anything in the old stuff, which is the goal.

Typical solution: **interface pattern**. Or a method that receives objects, and invokes a certain method of them, but it is the object itself that knows how to "perform" this method, once it is invoked.

# L. Liskov Substitution

**Subtypes should be substitutable for their base types**, without altering the correctness of the program.

A classic example: while squares are a subset of rectangles, it's not a good idea to have a class `square` inherit to a class `rectangle`, as some practical behaviors that are normal with rectangles would fail for a square. For example, you can change the length of one side of a rectangle, and expect the other side stay unchanged, but in a square it's either a non-valid operation, or the other side should be changed as well. One legit way to fix it is to have 1 class with a property of being either 'just' a rectangle, or a square, and writing all behaviors accordingly. Alternatively, create a `shape` object, and have square and rectangle both inherit to it, then use **factory pattern** ([[design_patterns]]) to abstract object creation.

As another example, in [[python]], a list, a tuple, or a series need to support all methods defined for an iterable. You cannot do an "aktchually" and decide that your new class is too specialized to support a more general interface.

# I. Interface Segregation

**Clients** (in this case, children classes of a base class, where the base class describes which methods they will expose) **should not need worry about interfaces they don't use**. This is, basically, how you achieve the "L" goal described above: because subclasses need to support all behaviors of the parent class, don't force them to support behaviors they can't meaningfully use. Which in practice means, don't define too many interfaces in the parent class. Only describe those behaviors that are really universal, and useful.

Directly related to the **interface pattern**. The trick is in creating small interfaces instead of one large one, and only expose clients to those interfaces that they need. Then let interfaces call appropriate methods of the base class.

Here's an example. Say, you're writing a game, and you make a class "NPC", hoping that all NPC types will inherit to it. In this base class, you describe interfaces, such as "attack" and "walk". But then some of your derived classes never attack, yet it is possible that at some point some piece of code may ask every NPC in the neighborhood to attack. So now for every subtype NPC you have to write a method `.attack()` that does nothing. This is bad. When creating new derived methods, we want to only describe those methods that matter to them. Ther eare several possible "good options" here:
1. Make the base class implement the default "attack" behavior (maybe even just "do nothing"?), and then override it in derived classes if needed
2. Conceptually abstract "attack" behavior into something like "freak_out", and treat it as either "attack" or "panic" depending on the NPC class (define it different in different classes)
3. Create an intermediate class ("enemy NPC"?) and when looking through NPCs, check their type, and only invoke "attack" on som eof them

The main thing, you should not be writing dud methods when defining your derived classes.

What is usually NOT a good solution is to create a single service class "NPC" that impelements the `attack` method by checking its own properties, and calling either `_panic`, or `_attack`, depending on the property. If followed consistently, this strategy would lead to a creation of a **fat class** - a giant compendium of all possible methods this class can do. If a class has too many methods (dozens to hundreds), it needs to split in sub-classes; at least by functionality (separate methods dealing with different types of functionality).

Footnotes:
* https://en.wikipedia.org/wiki/Interface_segregation_principle
* https://learning-notes.mistermicheels.com/architecture-design/oo-design/avoiding-fat-service-classes/

# D. Dependency Inversion

**High-level modules shouldn't import anything from low-level modules. Instead, use a middle-level abstraction, to decouple them**. Basically, we want to have two different thinking models for high-level thinking, and low level implementations. We don't want to rewrite the top-level logic when implementation changes, and vice versa. To achieve this goal, we introduce an intermediate abstraction, resulting in a 3-layer structure:
* Policy layer - overall logic
* Interface - imported by the policy layer. No implementation here, only a definition of supported interfaces
* Mechanism layer - actual implementations of whatever interfaces that the mechanism layer exposes
(And then the same triad is supposed to replace the Mechanism layer - Utility layer sequence)

> 🔥 I have to admit that I'm not quite getting how it would help me in practice, especially in Python.

Apparently, if you follow this path, if forces you to start development from low-level modules, bottom-up, rather than top-down, from policy modules, which is seen is healthier (and which is called "inversion"). The practice of introducing all these abstract levels is essentially what is called "design patterns" ([[design_patterns]]).

A typical situation: instead of module A (higher) directly referencing module B (lower), we have A referencing an abstract level, and then have B inherit to this abstract level. Which is why it is called an "inverstion". **Before**, you have a direct reference:
```python
# A
include B
b = B()
b.work()

# B
class B():
    def work(self):
        ...
```

**After**, you have:
```python
# A
include C
c = C()
c.work()

# C
class C():
    def work(self):
        pass

# B
include C # Now B includes something, which is kinda backwards, and feels like an inversion
class B(C): # Even though it includes it only to extend it, not to call it
    def work(self):
        ...
```

Related patterns [[design_patterns]]: **Adapter pattern**, **Service locator pattern**

Footnotes:
* https://en.wikipedia.org/wiki/Dependency_inversion_principle
* https://stackoverflow.com/questions/61358683/dependency-inversion-in-python

# Refs

Some examples in Python (only semi-helpful, but still better than several top results on Google that are just random Medium-noise):
https://github.com/heykarimoff/solid.python