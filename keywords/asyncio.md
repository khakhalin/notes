# Asyncio

Parents: [[parallelism]], [[microservice]], [[python]]
See also: [[flask]]

#realtime #python


A typical design fork when processing external requests (see [[flask]]) is between `asyncio.Lock` (for asyncronous, parallelized code) or `threading.Lock` (for synchronous threaded code where requests get to a queue and are processed one by one). If we want to selectively lock individual resources, we need to create a dictionary of locks with useful names, and lock them selectively:
```python
lock = await get_lock(resource)  # This is not a standard method, it needs to be written
async with lock:
    # some sensitive stuff
    pass
```