# Language Reference

Python is an interpreted, interactive, object-oriented programming language, providing remarkable power with simple, clear syntax. It has interface to many system calls and libraries, and is extensible in C or C++.

## stdtypes

**numeric: int, float, complex**

* immutable, setting new value to existing variable means creating different object and bind to the existing var, with original value intact
* object pool as cache of small integers (as well as some strings), unlimited prcision in Python3 (restrain to memory size)
* float is implemented using `double` in C
* booleans are subtype of integers

**sequence: list, tuple, range(>3.2)**

* collection.abc.Sequence
* tuple is immutable, but `t = [],; t[0] += [1]` acts confusingly
* list is like stack, even though we can `insert(0)` and `pop(0)` to make it act like queue, but time complexity is `O(n)`
* range takes less amount of memory comparing to list or tuple

**sequence/text: str**

* immutable sequence of unicode code points
* format: `printf`-style, `str.format()` and f-string, or `string.Template('$page: $book')`
* four kinds of methods: search(`find`), split-join(`splitlines`), format(`lower`), content-inspect(`isdigit`)

**sequence/binary: bytes, bytearray, memoryview**

* bytearary is a mutable counterpart to bytes
* memoryview can access internal data that supports the buffer protocol without copying

**dict**

* mutable, collections.abc.Mapping
* maps hashable values to arbitrary objects
* dict.keys(), dict.values() and dict.items() are view objects, they reflect the changes when dict changes
* `__missing__`

**set: set, frozeset**

* collections.abc.Set
* unordered collection of distinct hashable objects
* frozeset is immutable and hashable

## basic special methods

**`__new__`**

* implicit static method
* intended to allow subclasses of immutable types ot customize instance creation, or to be overridden in  metaclasses to customize class creation
* abuse: implementation of Factory Method, validation
* used to implement singleton, but since `__init__` is invoked as long as `__new__` returns an instance of `cls`, it's not appropriate anymore

**`__del__`**

* `del x` doesn't call `x.__del__()`, but decrease reference count only

**`__repr__` and `__str__`**

* `__repr__` is expected to return a valid Python expression that could be used to recreation the same object
* `__str__` returns nicely printable string
* `str()` search `__str__` first and fallback `__repr__`

## widget

**decorator**

**yield[ from]**

## descriptor

**intention**

**`__get__`**

**`__set__`**

**`__del__`**

**`__set_name__`**

## metaclass

**intention**

**`__prepare__`**

**`__init_subclass__`**

## memory && gc

## new-style class

## Python2 vs Python3
