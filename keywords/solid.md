# SOLID

#oop

Parent: [[oop]]
See also: [[design_patterns]]

An acronym for 5 core principles of OOP:
1. SRP: Single-Responsibility
2. OCP: Open-Closed
3. LSP: Liskov Substitution
4. ISP: Interface Segregation
5. DIP: Dependency Inversion

Has an alternative set, called GRASP :) https://en.wikipedia.org/wiki/GRASP_(object-oriented_design)

# 1. Single Responsibility (SRP)

Formally, it's about what every class is responsible for, but ultimately, it's about when the code for a class is needed to be changed. As all of these principles are ultimately about coding and refactoring optimally. Aka **how to draw borders**.

Ideally, for a programmer, **there needs to be only one legitimate reason to change each class**. Say, if you load something, process it, and output it, you need to build your code in such a way that when loading changes, you only update one class. When saving changes, you only update another class. It's like non-repetition of code, but abstracted one layer up. If you have prepping for saving scattered across the code, this code will be a nightmare to update. On the other hand, if saving is managed by only one class, and other code talks to it through a good interface, then a change from csv to SQL or whatever will only require your changing this one class, not the entire pipeline.

Related patterns: **Facade pattern**, **Chain of responsibility pattern**

# 2. Open-Closed (OCP)

Class should be **open to extension** without a need for modification (aka **closed to modification**). You need to be able to add new behaviors without rewriting earlier code. Leading to an important decision of **which parts to abstract early on**. Essentially, if, when adding a new behavior, you need to rewrite something in the class, this principle was violated.

A classic example is a method that has a long "switch case" of sorts, doing different things depending on what type of an object it receives. Which means that every time you add a behavior, you have to make this class larger. Instead, consider this top-level class call some method that is shared across all types of objects (inherited from an abstract class), and then in each class describe how this abstract methods needs to be implemented. Now if you're adding a new class, you're only adding new stuff, you don't have to change anything in the old stuff, which is the goal.

Typical solution: **interface pattern**. Or a method that receives objects, and invokes a certain method of them,, but it now the object itself that knows how to "run" this method.

# 3. Liskov Substitution

**Subtypes should be substitutable for their base types**, without altering the correctness of the program.

A classic example: while squares are a subset of rectangles, it's not necessarily a good idea to have a class `square` inherit to a class `rectangle`, as some practical behaviors that are normal with rectangles would fail for a square. For example, if you set both sides of a rectangle, you can expect it to hold shape, but actually in a square, you have to resize both sides every time either of them is set. So the 2nd command will override the 1st.	

One legit way to fix it is to have 1 class with a property of being either 'just' a rectangle, or a square, and writing all behaviors accordingly. Alternatively, create a `shape` object, and then have square and rectangle both inherit to it. Then use **factory pattern** ([[design_patterns]]) to abstract object creation.

# 4. Interface Segregation

**Clients** (in this case, children classes of a base class that describes what methods they will expose) **shouldn't worry about interfaces they don't use** (where "interfaces" are these outward exposed methods, I think).

Here's an example. Say, you're writing a game, and you make a class "NPC", hoping that all types of NPC will inherit to it. In this base class, you describe interfaces, such as "attack" and "walk". But then some of your derived classes never attack, yet it is possible that at some point some piece of code may ask every NPC in the neighborhood to attack. So now for every subtype NPC you have to write a method `.attack()` that does nothing. This is bad. When creating new derived methods, we want to only describe those methods that matter to them.

Directly related to the **interface pattern**. The trick is in creating small interfaces instead of one large one, and only exposing clients to those interfaces that they need. Then let interfaces call appropriate methods of the base class.

A problem with following this principle results in a so-called 'fat class' with a collection of assorted methods that is too large to manage (_and that may also lead to performance issues apparently?_)

Footnotes:
* https://en.wikipedia.org/wiki/Interface_segregation_principle

# 5. Dependency Inversion

**High-level modules shouldn't depend on low-level modules; both should depend on abstractions**. Basically, when coding, try to introduce an abstract object on which both particular modules (low-level, utility level) and more general (high-level, mechanism and policy) levels rely. Apparently, if you do it, if forces you to start development from low-level modules, bottom-up, rather than top-down, from policy modules, which is seen is healthier (and which is called "inversion"). The practice of introducing all these abstract levels is essentially what is called "design patterns" ([[design_patterns]]).

A typical situation: instead of module A (higher) directly referencing module B (lower), we have A referencing an abstract level, and then have B inherit to this abstract level.

> It's a bit like working with iterators in Python, right? Top level modules don't know how the iterator works, but they call some basic standardized methods on it. And then as you write your utility class, you can make it into an iterator by supporting some bare minimum of methods.

Related patterns [[design_patterns]]: **Adapter pattern**, **Service locator pattern**

# Refs

Some examples in Python (only semi-helpful, but still better than several top results on Google that are just random Medium-noise):
https://github.com/heykarimoff/solid.python