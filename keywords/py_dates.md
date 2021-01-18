# Dates and times in Python

#tools

Parents: [[python]]
Related: [[pandas]]

Two main libraries: **time** and **datetime** with different syntax.

Main functions:
* `strftime` - from datetime to string. The list of format command: https://docs.python.org/3/library/time.html#time.strftime
* `strptime` - from string to datetime.

Gotchas:
* Both packages have `strftime` to parse date-time expressions, but they use a different sequence of arguments.
* Most useful functions sit in: `datetime.datetime`, but not all of them (`timedelta` doesn't, for example), so we cannot easily import only the inner part.