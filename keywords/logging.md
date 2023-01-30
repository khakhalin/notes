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

To suppress warnings from some of the most verbose packages you use:
```python
logging.getLogger("package_name").setLevel(logging.INFO)
```

To filter one specific warning - 🔥 for now I don't know how to do it!! I tried this, as it seems to be an extension of what people suggest on StockOverflow:
`logging.getLogger("package").addFilter(lambda record: 'annoying_string' in record.msg)`
But for some reason it doesn't work (no error, just no filtering)

Footnotes that seem to claim that it should work, but it doesn't work:
* https://www.programcreek.com/python/example/3364/logging.Filter
* https://stackoverflow.com/questions/879732/logging-with-filters 

# Warnings

To suppress all warnings (not recommended): `import warnings; warnings.simplefilter('ignore')`

To only suppress deprecation warnings (as these seem to be the most annoying and the least relevant):
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