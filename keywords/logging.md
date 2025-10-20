# Logging

Parents:[[tools]] / [[python]]
See also: [[kibana]]

#python #debugging


The hierarchy of logging levels: `DEBUG < INFO < WARN < ERROR < FATAL`

Most logging traditionally happens at `DEBUG` level, while warnings and errors are to report bad news. `INFO` level is a bit of a mystery: supposedly, it is needed to offer a very basic markup of code behavior, to position Warnings and Errors within this markup. Some say, you should get no more 1 info-level message for every 20 debug-level messages. But it also means that one shouldn't really use `INFO` level within packages, as every function in a package may be called thousands of times during a normal script execution.

Some people also position a `TRACE` level either somewhere between `DEBUG` and `INFO`, or the other way around - even lower than `DEBUG`. A classic python logger doesn't support this level though.

# Logging in Python

Just to send logging events from a package:
```python
import logging
_log = logging.getLogger(__name__)
_log.debug("Hey there!")
```

To see events in a Jupyter notebook:
```python
import logging
import sys
_log = logging.getLogger(__name__)
logging.basicConfig(level=logging.INFO, stream=sys.stdout,
                   format='%(asctime)s | %(levelname)s : %(message)s'))
```

To send 2 different logs from a script, with two different levels:
```python
import logging

# This part goes into __main__() of the script
formatter = logging.Formatter(fmt="%(asctime)s: %(levelname)s: %(funcName)s: %(message)s",
                              datefmt='%I:%M:%S')
hnd = logging.StreamHandler(stream=sys.stdout)

logger = logging.getLogger()
fh = logging.FileHandler('detailed_log.log') # Will be written to the file
fh.setLevel(logging.DEBUG)
ch = logging.StreamHandler() # Will go to the screen
ch.setLevel(logging.INFO)
ch.setFormatter(formatter)
fh.setFormatter(formatter)
# add the handlers to logger
logger.addHandler(ch)
logger.addHandler(fh)
```

And then everywhere elsewhere you get `logger = logging.getLogger`, and supposedly it will return this nicely pre-configured logger (initialized in `_main_()`), so depending on the levels set, `logger.info("My message")` would go either in both streams (handlers), or only one of them.

## Filtering messages

To suppress warnings from some of the most verbose packages you use:
```python
logger = logging.getLogger("package_name")
logger.setLevel(logging.INFO)
```

**To filter one specific warning**: You need to create a filter (either a class or a function) and attach it to something. The filter is supposed to return a Boolean, and messages that trigger a `True` will pass, while messages that trigger a `False` will be filtered out.

Stackoverflow recommend a solution with a class:
```python
import logging

class SnowflakeFilter(logging.Filter):
    def filter(self, record): 
        return not record.getMessage().startswith("snowflake.connector.cursor: query execution done") 
    
logger = logging.getLogger("snowflake") 
logger.addFilter(SnowflakeFilter())
```

but also mentions that you don't need a class, technically; that it's enough to pass a function:
```python
def snowflake_filter(record): 
    return not record.getMessage().startswith("snowflake.connector.cursor: query execution done") 

logger = logging.getLogger("snowflake")
logger.addFilter(logging.Filter(name="snowflake_filter", filterfunc=snowflake_filter))
```

Which implies that technically one could even have a one-liner:
`logging.getLogger("package").addFilter(lambda record: 'annoying_string' not in record.msg)`

The problem is that none of these solutions work for me. What did work, is attaching the filter to a handler, not a logger. Even though that's not what people write online. Don't know the reasons of this difference:

```python
if not len(root_log.handlers):  
    root_log.addHandler(logging.StreamHandler())  # Standard process of handler creation
def my_filter(record):
    return 'lala' not in record.getMessage()
root_log.handlers[0].addFilter(my_filter)
```
Footnotes:
* https://www.programcreek.com/python/example/3364/logging.Filter
* https://stackoverflow.com/questions/879732/logging-with-filters 

# Formatting messages

```python
if not len(root_log.handlers):  
    root_log.addHandler(logging.StreamHandler())  # Standard process of handler creation
log_format = logging.Formatter(fmt="%(asctime)s: %(levelname)s: %(name)s: %(message)s")
root_log.handlers[0].setFormatter(log_format)     
```

# Warnings

What's better, `warnings` or `logging`? The official policy seems to be that if you talk to the user, `logging` is better, but if you want to communicate with the debugger of some client code, `warnings` may be preferable. For me, in practice, it means defaulting to `logging`. Also apparently it's possible to redirect `warnings` messages into `logging` interface with a few more lines at root logger init, but I haven't looked deeper into it.

To suppress **all warnings** (not recommended):
`import warnings; warnings.simplefilter('ignore')`

To only suppress **deprecation warnings** (as these seem to be the most annoying ones):
```python
import warnings
warnings.filterwarnings('ignore', category=DeprecationWarning)
warnings.filterwarnings('ignore', category=FutureWarning)
```

# Refs

A healthy discussion on log levels:
https://stackoverflow.com/questions/2031163/when-to-use-the-different-log-levels

Python logger documentation:
https://docs.python.org/3/howto/logging.html
The cookbook contains some good common ready-to use solutions:
https://docs.python.org/3/howto/logging-cookbook.html

How to log properly:
https://www.linkedin.com/pulse/five-philosophies-designing-logs-veronica-schmitt/