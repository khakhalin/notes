# Regular Expressions

Parents: [[tools]] / [[python]]
Related: [[pandas]]

#tools


In Python, package `re`, but is also supported by Pandas (see [[pandas]]).

* `a` - letter a, once
    * `a?` - same, but either 0 or 1 times
    * `a+` - same, but 1 time or more
    * `a*` - same, but either 0 times, or more
    * `a{2}`- exactly 2 repetitions
    * `a{2,4}` - anything from 2 to 4 repetitions (inclusive)
    * `a{2,}` - 2 or more
* `(a)` - letter "a" found, and captured to the output
* `(?:a)` - letter "a" detected
* `(ba)+` - a sequence "ba", repeated any number of times, and captured to the output
* `(one|two)` - either of these words
* `.` - any one symbol (except the newline symbol)
* `[abc]` - any symbol from this set, once. Combine with `?+*` to regulate how many times these symbols need to be matched. For example `[abc]+` would match `cccb`
* `[^abc]` - any symbol except these
* `[a-zA-Z]` - any letter
* `\w` - any alphanumeric, including `_`
* `\d` - same as `[0-9]` (but does not include `.+-`)
* `\s` - any whitespace
    * Uppercasing these symbols above with complements the set. Like, `\S` is any NOT whitespace.
* `^` in the very beginning of a regex means string start. `$` at the end means string end.
* `|` - for an OR operation between two sub-expressions

More notes:
* To escape characters (like, search for `[`) use `\` before the character (including `\\`)
* As normally `\` is used in strings to write special characters, it makes sense to use a so-called Raw string format: `r"lala"`
* There's some other 3d party variant of re that supports unicode characters better.

Methods:
* `re.match()` - checks if there's a match
* `re.search()` - scans through the string looking for matches. If can find a match, returns a special match object. Check `.group(0)` to find the first match. If no match, returns None. Because of that, writing `re.search(...).group(0)` only works if there's a match, as one cannot take this property on a None.
* `re.findall()` - finds all matches, as a list

Some useful regex:
* `#\w+` - hashtags
* `[\w.]+@[\w.]+\.[\w]+`  - email (not sure it's correct, that's my attempt :)
* `[-+]?\d+` - a signed integer
* `[-+]?(\d+(\.\d*)?|\.\d+)([eE][-+]?\d+)?` - a scientific format number

> I'm still not sure what is the best way to match strings of a fixed format, like `From 40 to 20`. I can match this string as `From \d+ to \d+` but the result has both "from" and "to", so what do I do next? Do I manually index it to cut out the middle, or do I do `.split(' ')` on the result, and then pick `[1]`, or what? Especially considering that if matching fails, I'd get a None, which means that I need to wrap it all into `if not None`, and then also it means that I cannot use `|` inside the regex, as it would mess with all this hard-coded formatting, no? Hardly a good replacement for scanf...

Ideally REs need to be  **compiled** (for speed?). For example:
```python
p = re.compile("[a-z]+")
p.match('lala: lovely string')
```

# Refs

Intro: https://docs.python.org/3/howto/regex.html

A really cool sandbox to play with, that both explains and traces the logic of the match: https://regex101.com/
* Including this "learning quiz" that gets progressively harder: https://regex101.com/quiz

Full syntax: https://docs.python.org/3.4/library/re.html

* https://medium.com/better-programming/introduction-to-regex-8c18abdd4f70
* https://medium.com/factory-mind/regex-tutorial-a-simple-cheatsheet-by-examples-649dc1c3f285