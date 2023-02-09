# Logging

Parents:[[01_Tools]] / [[python]]
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
logging.getLogger("package_name").setLevel(logging.INFO)
```

**To filter one specific warning**: 🔥 for now I don't know how to do it! People on Stackoverflow seem to claim that you don't need a class for that, and that passing a function is enough. But it didn't work fo rme for some reason.

`logging.getLogger("package").addFilter(lambda record: 'annoying_string' in record.msg)`
But for some reason it doesn't work (no error, just no filtering)

ChatGPT claims that this is a solution: (🔥 _still needs to be tested_)
```python
import logging

class SnowflakeFilter(logging.Filter):
    def filter(self, record): 
        return not record.getMessage().startswith("snowflake.connector.cursor: query execution done") 
    
logger = logging.getLogger("snowflake") 
logger.addFilter(SnowflakeFilter())
```

An explicit method is not necessary, as we can create it from a function on the fly:
```python
def snowflake_filter(record): 
    return not record.getMessage().startswith("snowflake.connector.cursor: query execution done") 

logger = logging.getLogger("snowflake")
logger.addFilter(logging.Filter(name="snowflake_filter", filterfunc=snowflake_filter))
```

And the function can be created on the fly too:
```python
logger = logging.getLogger("snowflake") 
logger.addFilter(logging.Filter(name="snowflake_filter", 
                   filterfunc=lambda record: not record.getMessage().startswith(
                       "snowflake.connector.cursor: query execution done")))
```

> 🔥 So, even if the function works, the first difference is between `record.msg` and `record.getMessage()`, and the second is with my attempts returning True for bad messages, while chatgpt solution returns True for good messages. Need to try it again; perhaps my solution didn't work at all because of the `msg` part, and that is why I haven't noticed that the filter is inversed.

Footnotes with various attempts to solve this:
* https://www.programcreek.com/python/example/3364/logging.Filter
* https://stackoverflow.com/questions/879732/logging-with-filters 

# Warnings

To suppress **all warnings** (not recommended): `import warnings; warnings.simplefilter('ignore')`

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