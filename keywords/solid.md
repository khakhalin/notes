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

Has an alternative set, called GRASP :) https://en.wikipedia.org/wiki/GRASP_(object-oriented_design)

# S. Single Responsibility (SRP)

Formally, it's about what every class is responsible for, but ultimately, it's about when the code for a class is needed to be changed. As all of these principles are ultimately about coding and refactoring optimally. Aka **how to draw borders**.

Ideally, for a programmer, **there needs to be only one legitimate reason to change each class**. Say, if you load something, process it, and output it, you need to build your code in such a way that when loading changes, you only update one class. When saving changes, you only update another class. It's like non-repetition of code, but abstracted one layer up. If you have prepping for saving scattered across the code, this code will be a nightmare to update. On the other hand, if saving is managed by only one class, and other code talks to it through a good interface, then a change from csv to SQL or whatever will only require your changing this one class, not the entire pipeline.

Related patterns: **Facade pattern**, **Chain of responsibility pattern**

# O. Open-Closed (OCP)

Class should be **open to extension** without a need for modification (aka **closed to modification**). You need to be able to add new behaviors without rewriting earlier code. Leading to an important decision of **which parts to abstract early on**. Essentially, if, when adding a new behavior, you need to rewrite something in the class, this principle was violated.

A classic example is a method that has a long "switch case" of sorts, doing different things depending on what type of an object it receives. Which means that every time you add a behavior, you have to make this class larger. Instead, consider this top-level class calling some method which is shared across all types of objects (inherited from an abstract class), and then in each class describe how this abstract methods needs to be implemented. Now if you're adding a new class, you're only adding new stuff, you don't have to change anything in the old stuff, which is the goal.

Typical solution: **interface pattern**. Or a method that receives objects, and invokes a certain method of them,, but it now the object itself that knows how to "run" this method.

# L. Liskov Substitution

**Subtypes should be substitutable for their base types**, without altering the correctness of the program.

A classic example: while squares are a subset of rectangles, it's not necessarily a good idea to have a class `square` inherit to a class `rectangle`, as some practical behaviors that are normal with rectangles would fail for a square. For example, if you set both sides of a rectangle, you can expect it to hold shape, but actually in a square, you have to resize both sides every time either of them is set. So the 2nd command will override the 1st.	 One legit way to fix it is to have 1 class with a property of being either 'just' a rectangle, or a square, and writing all behaviors accordingly. Alternatively, create a `shape` object, and then have square and rectangle both inherit to it. Then use **factory pattern** ([[design_patterns]]) to abstract object creation.

As another example, in [[python]], a list, a tuple, or a series need to support all methods defined for an iterable. You cannot do a "aktchually" and decide that your new class is too specialized to support a more general interface.

# I. Interface Segregation

**Clients** (in this case, children classes of a base class, where the base class describes which methods they will expose) **shouldn't need worry about interfaces they don't use**. This is, basically, how you achieve the "L" goal described above: because subclasses need to support all behaviors of the parent class, don't force them to support behaviors they can't meaningfully use. Which in practice means, don't define these interfaces in the parent class. Only describe those behaviors that are really universal, and useful.

Directly related to the **interface pattern**. The trick is in creating small interfaces instead of one large one, and only expose clients to those interfaces that they need. Then let interfaces call appropriate methods of the base class.

Here's an example. Say, you're writing a game, and you make a class "NPC", hoping that all types of NPC will inherit to it. In this base class, you describe interfaces, such as "attack" and "walk". But then some of your derived classes never attack, yet it is possible that at some point some piece of code may ask every NPC in the neighborhood to attack. So now for every subtype NPC you have to write a method `.attack()` that does nothing. This is bad. When creating new derived methods, we want to only describe those methods that matter to them. Possible solution here (afaik) would be to either create an intermediate class ("enemy NPC"?) and only call `attack` on them, or to conceptually abstract the "attack" behavior into something like "activate", and treat it as either "attack" or "panic" depending on the NPC. But there should be no need to write dud classes.

A problem with following this principle too literally may result in a so-called 'fat class' with a collection of assorted methods that is too large to manage (🔥 what?..)

Footnotes:
* https://en.wikipedia.org/wiki/Interface_segregation_principle

# D. Dependency Inversion

**High-level modules shouldn't import anything from low-level modules. Instead, use a middle-level abstraction, to decouple them**. Basically, we want to have two different thinking models for high-level thinking, and low level implementations. We don't want to rewrite the top-level logic when implementation changes, and vice versa. To achieve this goal, we introduce an intermediate abstraction, resulting in a 3-layer structure:
* Policy layer - overall logic
* Interface - imported by the policy layer. No implementation here, only a definition of supported interfaces
* Mechanism layer - actual implementations of whatever interfaces that the mechanism layer exposes
(And then the same triad is supposed to replace the Mechanism layer - Utility layer sequence)

> 🔥 I have to admit that I'm not quite getting it, so while I'm trying to convince myself otherwise in these paragraphs, I don't "feel it" for now. Don't see how it woudl help me in practice, espeically in Python.

Apparently, if you follow this path, if forces you to start development from low-level modules, bottom-up, rather than top-down, from policy modules, which is seen is healthier (and which is called "inversion"). The practice of introducing all these abstract levels is essentially what is called "design patterns" ([[design_patterns]]).

A typical situation: instead of module A (higher) directly referencing module B (lower), we have A referencing an abstract level, and then have B inherit to this abstract level. Which is why it is called an "investion". Before, you have a direct reference (🔥 this example isn't finished, and I'm not sure it is correct right now. Is it even Pythonic?)
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

After, you have:
```python
# A
include C
c = C()
c.work()

# B
include C # Now B includes something, which is kinda backwards, and feels like an inversion
class B(C): # Even though it includes it only to extend it, not to call it
    def work(self):
        ...
        
# C
class C():
    def work(self):
        pass
```

Related patterns [[design_patterns]]: **Adapter pattern**, **Service locator pattern**

Footnotes:
* https://en.wikipedia.org/wiki/Dependency_inversion_principle
* https://stackoverflow.com/questions/61358683/dependency-inversion-in-python

# Refs

Some examples in Python (only semi-helpful, but still better than several top results on Google that are just random Medium-noise):
https://github.com/heykarimoff/solid.python