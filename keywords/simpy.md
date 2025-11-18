# Simpy

Parents: [[python]], [[tools]]
See also: 

#python #simulation


A perfect python package for discrete simulations. Simpy 1 and 2 were similar, very confusing-looking (with inheritance to base classes), and ugly. Simpy 3 (described here) may look a bit naive at first: there's an einvironment and you throw functions into it, to be executed, but is actually a much more sound design system.

* Actors (things that act or move) become objects that carry **process functions** describing their life: travel, process etc.
* Objects that are passively passed around by actors - Python objects
* Congestion points (bottlenecks) - **Resources**, **Stores**, **Containers**
* Key status changes during the simulation - **Simulation events**
* Global environment to track all of that: `env = simpy.Environment()`

# Processes

Each process function = one coherent tasks. Functions always take time, e.g. using `env.timeout(t)`. They can wait for other processes to finish using `yield env.process(process_function)`, or start processes running "in parallel" without waiting for them to finish, calling them without yield. Do it mid-function, you get async branching; do it at the very end - you get chaining of processes.

Processes can be interrupted with `process.interrupt(cause)`. Interruptions use Pythonic interruptions, and need to be explicitly handled using a `try` wrapped around a `timeout`:
```python
remaining_time = full_duration
tic = env.now
try:
    yield env.timeout(remaining_time)
    remaining_time = 0
except simpy.Interrupt as interrupt:
    remaining_time -= (env.now - tic)
    print(inteerpt.cause)
```

Processes can request resources:
 ```python
with resource.request() as req:
    yield req  # Wait till it becomes free and can be acquired
    yield env.timeout(some_time)  # Imitate using it
# at this point `with` calls resource.release() automatically
```

Process can wait for whatever happens first (here's an extreme example with two competing processes and a timeout):
```python
with resource.request() as req:
    p2_done = env.process(process2())
    timeout = env.timeout()
    result = yield req | p2_done | timeout
    if req in result:
        # we got the resource
    elif p2_done in result:
    	# process2 was done first
    else:
    	# we timed out
```
Or it can wait for _both_ events using `yield event1 & event2`

# Events

Events are the main away for agents to communicate with each other, as events are something they can be _waiting for_. Timeouts are the simplest example of an event, but here are two more basic patterns:

## Hibernate until an event

A scenario in which a central process decides to put workers to sleep until something happens:

```python
def worker(env, global_bell):
    yield global_bell  # hibernate until the bell rings
    # resume work

global_bell = env.event()
for worker in workers:  # If necessary
	env.process(worker(env, global_bell))

# Then separately, in a different part of the code, awaken all hibernating workers at once:
global_bell.succeed()

```

An alternative, where each worker goes to sleep on its own, but al of them are still awaken at once:

```python
class Worker():
    def sleep(self):
        yield self.environment.global_bell

# Somewhere in the environment:
self.global_bell.succeed()
# The bell needs to be recreated immediately, or workers falling asleep won't have anything to grab
self.global_bell = simpy.event()  
```

## Change event interrupting normal work

```python
def agent(env, world_change_event):
    while True:  # Do current task BUT also listen for world changes        
        current_task = env.timeout(WORK_DURATION)  # IRL this would be something useful of course        
        result = yield current_task | world_change_event  # AnyOf pattern
        
        if world_change_event in result:
            # World changed - update plans immediately
            update_plans()
            world_change_event = env.event()  # reset for next change
            # Note: current_task might be partially done, and it should be handled here
        
        if current_task in result:
            # Task completed normally
            continue
```

# Resources

Types of resources:
* **Resource** - basic resource, has parameter `capacity` (1 by default); processes would wait until a resource is available. All requests against a resource go into a FIFO queue. See [[heap]]. `.count` gives current queue length. `.users` seems to give a list of users in the queue
    * Subtypes:
        * **PriorityResource** - its `.request()` supports a `priority` parameter, and the request goes into a priority queue.
        * **PreemptiveResource** - its `.request()` also gets a `priority` parameter, but if the priority of a request is higher of the task currently running, this task is interrupted (the code should support an `interrupt` tho, or nothing will happen). In practice, interrupting priority requests can be sent to a standard resource as well, using `.PriorityRequest()` method, but that's probably not a good pattern.
* **Container** - a tank or a battery with an `.level` property: the amount of stuff "inside". Gets an `init=0` float parameter at init, to set the original level. If `.capacity` is not set, the container is unlimited.
    * `.get(amount)` gets stuff from a container, reducing its `level`. It's a process that needs to be `yield`ed: if there's not enough stuff in the container, it won't resolve, but will wait until there's enough stuff.
    * `.put(amount)` adds stuff to a container. It's also a process: if the `amount` doesn't fit in the container, given the original `level` and the `capacity`, it will wait until there's enough space inside. Running them as "normal command" runs a risk that they won't be triggered until later.
* **Store** - a storage of objects, accessible via `.items()`.
    * `.capacity` - just the max number of objects that fit in the store. For anything fancier (objects of different size competing for total capacity) we'll need to write our custom class.
    * `.put()` - put an item
    * `.get()` - get an item in FIFO order
    * Subtypes:
        * `PriorityStore` - instead of FIFO, returns objects in reverse priority order, either because you put them there with priority, or because you make objects comparable with `__lt__()`.
        * `FilterStore` - you have to define a `filter` function, and then the store will return an object that matches the filter, using a special `get()` that's actually an alias for `FilterStoreGet`. We can implement getting objects by name, for example.

# Simulation

`env.run(until=duration)`. But by that time some processes need to be started, or nothing will happen.

# Other useful patterns

## Chained processes

```python
class Dude:
    def sleep(self):
        yield env.timeout(12)
        env.process(self.walk())

    def walk(self):
        yield env.timeout(12)
        env.process(self.sleep())
```

## Generator (emitter?) of async processes

Generator that uses process functions directly, as temporary objects. They can all run in parallel, or they can compete for a resource and thus get in a queue, or anything else.
```python
def item(env, params):
    # things that happen in the beginning
    yield env.timeout()
    # things that happen in the end

for i in range(n_items):
    yield env.timeout(correctly_distributed_time)  # wait a bit    
    x = item(env, params)  # beginning things happened here
    env.process(x)  # It's not being ran asynchronously    
```

