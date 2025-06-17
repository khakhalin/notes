# GIL = General Interpreter Lock

Parents: [[python]]
See also: [[concurrency]], garbage collection

#python #concurrency #realtime #memory


Python doesn't have garbage collection for memory management; instead, it uses a **reference counter**: for any object, there's a counter that counts how many pointers are there that point at this object. For an object `a` one can see this reference count with `sys.getrefcount(a)`. But this means that, to be sure that objects are deleted only once the counter reaches 0, Python has to ultimately de-parallelize all processes, even if they pretend to be running in parallel, to make sure this counting is consistent. At the moment when an object is created, copied, or deleted, Python locks all other processes. Which is called a General Interpreter Lock.

There are variants of Python that don't have it, but they are non-standard. Also there's some chance that it will disappear in the future. But for now all attempts to remove it "nicely" failed, as removing it either breaks backwards compatibility subtle ways, or lowers performance. And there's a pledge of sorts that GIL removal won't be introduced until there's a way to do that without lowering the performance.

# Refs

https://realpython.com/python-gil/