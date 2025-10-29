# YAML

Parents:
See also: [[json]], [[markdown]]

#formats


A config / metadata -sharing format, alternative to [[json]] and somewhat related to [[markdown]] (although apparently `YAML` itself is a self-referential abbreviation insisting that it's not related to Markdown). File extensions of `.yml` and `.yaml` are both popular.

The official YAML documentation is absolutely horrendous: it is written in some meta-programming language, does not provide any examples, and is formatted like a personal website from late 1990s on cocaine. Because of that, YAML knowledge has to be collected from random pages and stackoverflow questions.

# Basic structure

`key: val` - equivalent to [[json]] `{key: val}`. Strings can be given without quotation marks. To describe a whole dictionary, just write these, one line at a time. The sequence don't matter, as keys within a dictionary aren't ordered. The type of a value is inferred from the value.

If a value is a dict itself, just increase an indent (traditionally, 2 spaces. Tabs are not allowed). For example:
```yaml
key:
  other_key: whatever
```

For a small sequence, do this: `key: [v1, v2, v3]`. For a longer sequence, do this:
```yaml
key:
- v1
- v2
```
A sequence of sequences uses either `- [ ]` or offsets. You don't put two `-` in the same line however, but rather place a `-` at where it should be, and start a new line, offsetting it properly.

The most confusing notation happens if you have a sequence of dictionaries. Because keys within a dictionary are unordered, in an automatically-generated yaml, the command of starting a new element in a sequence (`- `) would typically get positioned around some random unimportant key, and it may be confusing. For example:
```yaml
parent_key:
- absolute_nonence: 1
  title: example
- child:
    value: 2
  title: another_example
```
In the example above, we have a sequence of 2 dictionaries, with what appears to be titles "example" and "another example", but each dictionary also has other keys, and at least alphabetically they are sorted before the "titles". So [[pydantic]] for example, while creating a yaml automatically, would put them before the "titles". And it may be confusing, especially if some of them invite you to go "deeper", like this `child` key above.

The good news is that one can reorder stuff manually. And another good news is that, unlike for json, yaml support comments (using a `#` sign).

For longer string values there are special syntax contructions. If you want to preserve new lines (`\n`) within your string, use a `|` symbol:
```yaml
key: |
  some text
  it may be multiline
```
If you don't want to have `\n` between the lines, instead of `|` use `>`, in exactly same way.

Unordered sets (as opposed to ordered sequences) and ordered maps (as opposed to standard dicts) are also possible, using `!!set` and `!!omap` directives, but I'm not a fan.

# Anchors and aliases

In general, alias (anchor) is set with `&` symbol, and then inserted elsewhere with `*` symbol. However the exact behavior is a bit annoying. First let's consider dictionaries:
```yaml
a: &anchor
  x: 1
  y: 2

b: 
  z: 3
  <<: *anchor
```
Here `&anchor` is given before a dictionary (with 2 keys) is defined, so now it's set to represent this dictionary. If we just had `b: *anchor` that would have been a valid yaml as well, and `b` would have just "contained" the same dict as `a`. But if we want to add keys that were earlier defined in `a` and caught by an anchor to `b`, we need to follow the syntax shown here. `<<` is a special key that kinda "disappears" and merges the key-value pairs into a different dictionary.

Notes to this syntax:
* Above, `a` is needed, even if the only goal of it is to define an anchor; just because a dictionary cannot just "fly in space"; so you have to put it inside _some_ key, to keep the definition part legal.
* If you merge several aliases, they will be combined.
* If you merge an alias inside a different node with a merge operator `<<`, and then reset one of the keys once again (like `x: 0` in the example above), you won't get an error that you normally get when rows are duplicated. Instead, you will overwrite the value of this key. The sequence in which the alias and the explicit key is given does not matter; the explicit `x: 0` will always overwrite the `x: 1` that is stored inside the alias.
* If you merge several aliases, and there's some overlap between keys in these aliases, the first alias takes precedence.

When anchoring a list you can then use it as an alias of a value downstream. Unlike for collections of keys, you cannot unfortuntely mix in a list into another list, or concatenate two list. This seems to be a known limitation. So you can do this:
```yaml
a: &anchor
- 1
- 2

b: *anchor
```
And then `&anchor` will represent the entire sequence block, and then you can put the same sequence into `b` key. But unfortunately you cannot union or concatenate it with another sequence.

# Python libraries

To work with the yaml-supporting package one just does `import yaml` (although apprently it's officially called `pyyaml`, and that's the name youuse with `pip` to install it).

* `yaml.safe_load(file)` - to load to a dictionary. Here `file` comes from a usual `with open('a.yaml', 'r') as file:`
* `yaml.dump(dict, file)` - writes a yaml file (as above, `file` should be opened; this time for `w`)
* `yaml.safe_load(string)` - parses a yaml-style string into a dict
* `yaml.dump(dict)` (without file specified) - transforms a dict into a yamly string (may be helpful for logging whlie debugging)

`yaml.load()` is apparently a deprecated method that was unsafe.

# Refs

* https://onlineyamltools.com/convert-yaml-to-json - converts yaml to json. The best way to test syntax and compare outputs.
* https://www.yamllint.com	 - interactive yaml troubleshooter. It shows you the errors (which is helpful), but also eagerly optimizes and optimizes aliases, in some simple cases outright eliminating them, which is not always helpful.
* https://www.educative.io/blog/advanced-yaml-syntax-cheatsheet - some nice examples of advanced yaml functions
* https://yaml.org/spec/1.2.2/ - Official YAML documentation (horrible)