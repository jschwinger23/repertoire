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

gulf between classes and types:

* cannot directly subclass the dictionary type
* introspection interface for finding out methods and instance variables an object has is different for types and classes
* classes use `__dict__`, `__class__` and `__bases__` to search an attribute of an object
* types use `__members__` and `__methods__` to list method names and data attribute names supported, but have never been carefully defined, like Fulton's ExtensionClasses ignore the type API, and instead emulate the class API, which is more powerful.

so we are to make types look more like classes, or the opposite option: make classes look more like types. Due to classes introspection API is more powerful, like we can extract SocketType's docstrings without creating a socket, we hereby provide PEP0252 and bring descriptor.

**`__get__` && `__set__`**

`__get__` and `__set__` are designed to well define attribute searching precedence rule, cause we are mixing type `__methods__` / `__members__` into classes `__dict__`, but we can't have a simple rule like 'static overrides dynamic' or 'dynamic overrides static', because sometimes static indeed overrides dynamic, such as `'__class__'` in an instance's `__dict__` is ignored if favor of the statically defined `__class__`, and most keys in instance's `__dict__` override instance's `__class__.__dict__`.

here's the rule:

* static attribute with `__get__` as well as `__set__` override dynamic, call it data descriptor
* static attribute with `__get__` without `__set__` not override dynamic, call it non-data descriptor
* `__get__(self, instance, owner)`
* `__set__(self, instance, value)`

Here we can implement bound method, unbound method, staticmethod, and classmethod easily. Cool!

**mro**

since now all classes are inherited from `object`, diamond inheritance emerges as a problem. So we use MRO!

* DSF
* remove all occurences except for the last

## metaclass

**intention**

**`__prepare__`**

**`__init_subclass__`**

## memory && gc

## new-style class

## Python2 vs Python3
