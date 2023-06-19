# Dates and times in Python

Parents: [[python]]
Related: [[pandas]]

#tools


The main package for dates and times in Python is called **datetime**. There is another one, called "time", with many methods looking deceptively similar, but using a different syntax. Yet now datetime seems to be winning, despite its really weird namespace (see below).

At least as of 2022, the best way to deal with these namespaces and the lack of comfortable functions is to completely rely on [[pandas]] for everything datetime related, as it provided a much more confortable interface for creating and manipulating core datetime types. At the same time, `from datetime import datetime as dt` may still have to be used if you need to get access to core classes of datetime, as whiel Pandas provides a better interface for these libraries, it doesn't fully expose libraries themselves (Refs: [1](https://gitlab.tudelft.nl/rhenning/ANTS-model/-/issues/28), [2](https://stackoverflow.com/questions/60856866/why-was-datetime-removed-from-pandas-1-0#:~:text=%22FutureWarning%3A%20The%20pandas.,Import%20from%20datetime%20module%20instead.%22)).

# Namespace

One of the most annoying things about `datetime` as a package is that the main class that actually contains all functions is also named `datetime`. But it does not contain everything: for example, it doesn't contain the definition of data types used in this package (say, the definition of `date`), and one needs access to these types if they want to do `isinstance()` checks. Which means that one is faced with several almost equally ugly solutions to this problem.

1. `from datetime import datetime, date` - the most popular one. Now all useful stuff is accessible via `datetime.blabla()`, and `date` is available as a separate variable. I don't like it because I feel that `date` is not a good name for something that is essentially a constant.
2. `import datetime` - cursed, as most useful stuff is now hiding in `datetime.datetime.blabla()` and that's just painful.
3. `from datetime import datetime, date as DATE_TYPE` - this one may be ok

# Methods

* `strptime` - from string to datetime. Some examples:
    *  `time(s, "%H:%M:%S %p")` would match a string of `05:12:41 pm`
    *  `datetime(d, "%d %m %Y")` for `22 08 2022'
* `strftime` - in the opposite direction, from datetime to string. 

Both use this list of format commands: https://docs.python.org/3/library/time.html#time.strftime
Among less obvious codes:
* `%b%` - 3-letter month name

Unlike dates in [[pandas]], datetime doesn't have a good smart function that would try to guess the format for you.

**Gotchas:**
* Both packages have `strftime` to parse date-time expressions, but they use a different sequence of arguments.
* Most useful functions sit in `datetime.datetime`, but not all of them (`timedelta` doesn't, for example), so we cannot easily import only the inner part.

# Refs

Full manual:
https://docs.python.org/3/library/datetime.html