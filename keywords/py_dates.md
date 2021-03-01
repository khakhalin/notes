# Dates and times in Python

Parents: [[python]]
Related: [[pandas]]

#tools

Two main libraries: **time** and **datetime** with different syntax.

### Main functions

* `strptime` - from string to datetime. For example, `time(s, "%H:%M:%S %p")` would match a string of`05:12:41 pm`.
* `strftime` - in the opposite direction, from datetime to string. 

Both use this list of format commands: https://docs.python.org/3/library/time.html#time.strftime

**Gotchas:**
* Both packages have `strftime` to parse date-time expressions, but they use a different sequence of arguments.
* Most useful functions sit in `datetime.datetime`, but not all of them (`timedelta` doesn't, for example), so we cannot easily import only the inner part.