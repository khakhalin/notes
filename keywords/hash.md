# HashMaps and HashTables

#algo

**Hashmaps** and **Hashtables** are basically the same thing; it's just that Java used to have a type named Hashtable, but now it's kinda deprecated (apparently), as it's _synchronized_ (operations are thread-safe, as in SQL), which is an ovehead liability ([ref](https://stackoverflow.com/questions/47838841/hashtable-hashmap-hashset-hash-table-concept-in-java-collection-framework)). So Java people would probably use the word "Hashmap" as default. Sedgewick uses the word "Hashtable" as default though.

**HashMaps** are essentially Python dicts (**dictionaries**): they map arbitrary keys to values. **HashSets** implement a set interface: you can add and remove with them, and do set operations, and the data structure itself will make sure that you don't have duplicate values.

# Implementations

Two main implementations: **Chaining** and **Probing**.

**Separate chaining**. A fixed, pre-defined hashmap that maps to an array, then linked list growing from each cell of this array to resolve collisions. Pros: cannot run out of space. Cons: have to guess the size upfront; wastes space if under-utilized (even when some lists are long already, some will still be empty); increasingly slow if over-uitilized.

**Linear probing**: Each cell contains a tuple (key,value). When writing, if a cell is occupied, go along the list towards next empty cell. During reading, go down comparing the keys. Pros: effectively uses space. Cons: tends to become increasingly slow as clusters develop, can run out of space. Possible solution: resize as necessary (but note that resizing also includes rehashing and rewriting all elements).

Just pain deleting from a linear probing hash would be a problem, as it would leave an empty space, that would break search. Solutions: either mark deleted cells with a special "deleted" property (indicating that we can write to this cell, but shouldn't stop at it while searching), or shift all elements after it backwards by one, upon deletion.

**Quadratic probing**: An improvement upon linear probing, in which to resolve collision you don't go linearly by adding k to the base address, but jump by adding k² to it (k takes N starting at 0, until the collision is resolved). By having it scattered, it reduces clustering, and associated performance issues. ([Wiki ref](https://en.wikipedia.org/wiki/Quadratic_probing))

**Double hashing**: use another hashing function to resolve collisions: use it to calculate the increment, then loop through the array with this increment. This second hash function should never output zero, and all of its outputs should be mutually prime with m, but if m is prime itself, it should be fine (_right?_). ([ref](https://www.geeksforgeeks.org/double-hashing/)).

# Hash functions

Hashmaps are sensitive to the quality of hash functions, as collision resolution is always slower. (Linear probing approach is apparently more sensitive; [wiki ref](https://en.wikipedia.org/wiki/Linear_probing)).

**For integers**, one simple approach is to use **modular arithmetic** `(a*x % p) % m` where p is prime, m is the size of the hash table, and a is some decent integer < p. The simplest approach ever would be to just do `a % m`, as long as m is prime.

A better approach is to use **multiply-shift** (much faster, easier, and more randomish ?). Mathematically, it's $(ax \mod 2^w) / 2^{w-M}$, but in practice it's just `(a*x) >> (w-M)` (right shift), where w is the number of bits in a machine word, M is the desired address length (in binary), and a is some "random-looking positive integer" a<2^w, such as 2654435769 (a good magic number). Refs: [wiki](https://en.wikipedia.org/wiki/Universal_hashing), [example](https://stackoverflow.com/questions/28463794/knuths-multiplication-hash-function-via-bit-shifting), [this magic number](https://cstheory.stackexchange.com/questions/6193/what-is-special-about-232-phi-in-cryptography).

**Multiplication method**: `floor(m*frac(k*c))` where c ∈ (0,1), frac returns the fractional part. A known good choice for c, apparently, is `(sqrt(5)-1)/2` ([ref](https://www.geeksforgeeks.org/what-are-hash-functions-and-how-to-choose-a-good-hash-function/)).

**Mid-square method**: square the number, then pick middle digits ([ref](https://opendsa-server.cs.vt.edu/ODSA/Books/Everything/html/HashFuncExamp.html)).

It may be possible to make hash functions above better by replaxing ax with ax+b, making a and b randomized, etc.

**For strings**, in a loop, `h = (h*a + ord(s)) % p` where s runs through chars in a string, p is a prime, and a is some reasonable number. In the beginning, h is set to some initial value, which can itself be optimized (?). 

The simplest approach that doesn't work that well: `h += ord(s)`, followed by eventual `h % p`.

Another alternative, **string folding**: in a loop by i, do `h += ord(s[i])*k`, where k repeatedly goes through selected powers of 2: `[2**j for j in [1,8,16,24]]`. ([ref](https://opendsa-server.cs.vt.edu/ODSA/Books/Everything/html/HashFuncExamp.html))

# Refs

* https://www.geeksforgeeks.org/difference-between-hashmap-and-hashset/
* Other types of hashing: https://en.wikipedia.org/wiki/Category:Hashing
* https://en.wikipedia.org/wiki/Universal_hashing

Keywords for searching: hash map, hash table, hash set, hashmap, hashtable, hashset, hash-map, hash-table, hash-set