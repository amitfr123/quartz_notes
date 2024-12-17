---
title: Fluent Python
draft: false
description: Summary of Practical Fluent Python by Luciano Ramalho.
---
> [!info] Incomplete article head
# The Python Data Model
## Special methods (Magic/Dunder)
Double underscore functions like `__getitem__` are called Magic or Dunder (double under) methods because they are special methods called by the python interpreter to perform specific operations on objects.

Here are some examples:
```
__len__(self) => len(obj)
__getitem__(self, pos) => obj[pos]
```

By implementing special methods we gain the following benefits:
* Standardizing the api for our clases.
* Allow support for STD.
* Gain advanced features by implementing the basic building blocks for them. For example implementing `__getitem__` gives us support for __slicing and iterating__ on our object.

### Explicit `__init__`
Unlike most magic methods `__init__` is used directly and the use-case is when initializing base class from derived in inheritance.

### `__repr__` vs `__str__`
Both `__repr__` and `__str__` methods are used to show the object properties as a string. Here are the main differences:
* `__repr__` is meant to be used by developers and doesn't have to be user friendly.  
* `__repr__` matches %r in classic formatting and !r in f-strings.
It should be noted that the implemented of object calls the method `__repr__` as a fallback in case `__str__` isn't implemented. So if only 1 method is implemented it should be `__repr__`.
### Ordering for Magic methods
For Magic methods that accept 2 operands like a + b the python interpreter will attempt to call the first operand implementation of `__add__` and only if it isn't implemented it will try to call the second operand reversed operator in this case `__radd__`.

# Data Structures
## Abstract collection classes (ABC)
The collection ABC unifies the 3 essential interfaces for every type of collection:
* Iterable
* Sized
* Container
There are 3 important specializations of collections are:
* Sequence
* Mapping
* Set
In order to support the different interfaces in your classes you must implement the Magic methods that comprise that interface functionality.

## Sequences
### Container vs Flat sequences
The main difference is that Container sequences may hold items of different types and Flat sequences may only hold items of 1 simple types.
This is important to understand because it means that containers sequences only hold references to their items while flat sequences hold the value in their own memory and not as separate python objects. This implementation detail is important because it means that Flat sequences do not need to allocate memory for the python object meta data.
Here are some examples:
* Container - list, tuple, deque
* Flat - str, bytes, array
### Mutable and Immutable sequences
Here are some examples:
* Mutable - list, bytearray, array
* Immutable - str, tuple, bytes
### List comprehensions (Listcomps)
This are easy and clean syntactic forms of list initialization that are more readable and oftentimes event faster. 
Here is an example:
```python
# Basic
symbols = '$¢£¥€¤'
codes = []
for symbol in symbols:
	if symbol > 127
		codes.append(ord(symbol))
```
vs
```python
#Listcomp
symbols = '$¢£¥€¤'
codes = [ord(symbol) for symbol in symbols if ord(symbol) > 127]
```
vs
```python
#Filter
symbols = '$¢£¥€¤'
codes = list(filter(lambda c: c > 127, map(ord, symbols)))
```
#### Cartesian Products
Listcomps can build lists from the Cartesian product of two or more iterables. 
Here is an example:
```python
colors = ['black', 'white']
sizes = ['S', 'M', 'L']
tshirts = [(color, size) for color in colors for size in sizes]
```

### Generator Expressions (genexp)
To initialize tuples, arrays, and other types of sequences generator expressions are used because they allow us to save on memory that would be wasted if we used a Listcomp just to push it to another constructor. This feature is very important for Cartesian products.

Genexps use the same syntax as Listcomps, but are enclosed in parentheses rather
than brackets.
Here is an example:
```python
symbols = '$¢£¥€¤'
tuple(ord(symbol) for symbol in symbols)
```

### Unpacking
Unpacking is the act of taking a sequence and deconstructing it to its components.
Here is a example:
```python
t = tuple(1,2)
(x,y) = t
```
#### Nested unpacking
This is a more advanced feature that allows you to unpack the internal structures of your data type.
Here is a example:
```python
t = tuple(1,2, tuple(3,4))
(x,y,(a,b)) = t
```
#### Evil *
By using * before a variable while unpacking we say that any values that weren't explicitly used should enter that value.
Here is a example:
```python
t = tuple(1,2, 3, 4)
x, y, *l = t
#l will contain a list including 3,4
```
The usage of * doesn't have to be the last values and can allow for unpacking logic like pull the first and last values from a list or tuple.
Here is a example:
```python
t = tuple(1,2, 3, 4)
x, *l, y = t
#l will contain a list including 2,3
```
### Pattern matching with sequences
This is a new feature in python that allows you to implement complex pattern matching in python for sequences.
Match improvement over switch is destructuring a more advanced form of unpacking. We can make pattern specific by adding type information, expected structure(including nesting) and expected values.

Here is a example:
```python
match record:
	case [name, _, _, (lat, lon)] if lon <= 0:
		print(f'{name:15} | {lat:9.4f} | {lon:9.4f}')
```

#### Matching _
We can use _ to match any value (allow any) without binding it to a variable.

#### Evil * comeback
We can use the * to perform the same logic as we did while unpacking.

### Slicing
Slices are objects that define the perspective to look into a sequences.
* Slicing in python excludes the last item.
* You can specify a step by using the syntax `s[start:stop:step]`
#### Multidimensional slices
The operator `[]` can take multiple indexes/slices separated by commas. An example for a 2 dimensional slice is `a[m:n, k:l]`.

### Building list of lists
The proper way is:
```python
board = [['_'] * 3 for i in range(3)]
```
The wrong way is:
```python
weird_board = [['_'] * 3] * 3
```
The weird board example is equivalent to:
```python
row = ['_'] * 3
board = []
for i in range(3):
	board.append(row)
```

## Dictionaries and sets
## Dictcomp
This feature allows you to build dictionaries in a similar fashion to Listcomps.
Here is an example:
```python
dial_codes = [
	(880, 'Bangladesh'),
	(55, 'Brazil'),
	(86, 'China'),
	(91, 'India'),
	(62, 'Indonesia'),
	(81, 'Japan'),
	(234, 'Nigeria'),
	(92, 'Pakistan'),
	(7, 'Russia'),
	(1, 'United States'),
]
country_dial = {country: code for code, country in dial_codes}
low_dial = {code: country.upper() 
	 for country, code in sorted(country_dial.items()) 
	 if code < 70}
```

## Mapping merging
You can use the `|` or `|=` operators to update the mapping in a dictionary.
Here is an example:
```python
d1 = {'a': 1, 'b': 3}
d2 = {'a': 2, 'c': 6}
d1 | d2
# {'a': 2, 'b': 3, 'c': 6}
d1 |= d2
# d1 = {'a': 2, 'b': 3, 'c': 6}
```

## Pattern matching
You can perform pattern matching like the ones done with sequences. It should be noted that pattern matching with dictionaries work with partial matches. So there in no use for `**extra` if you do not need them and `**_` is forbidden. 
Here is an example:
```python
def get_creators(record: dict) -> list:
	match record:
		case {'type': 'book', 'api': 2, 'authors': [*names]}:
			return names
		case {'type': 'book', 'api': 1, 'author': name}:
			return [name]
		case {'type': 'book'}:
			raise ValueError(f"Invalid 'book' record: {record!r}")
		case {'type': 'movie', 'director': name}:
			return [name]
		case _:
			raise ValueError(f'Invalid record: {record!r}')
```

## Default values and `__missing__`
The dictionary in python usually raises an exception when trying to access an uninitialized key. This behavior can be modified to prevent an exception in several ways:
* dict.setdefault - initializing using the given default value if the key is missing.
* defaultdict - set a default behavior for accessing with foreign keys.
* `__missing__` - this method is triggered with get or `__getitem__` on a given key fail.

### `__missing__` inconsistency
* dict subclass - only triggers on `__getitem__`.
* collections.UserDict subclass - triggers on get or `__getitem__`.
* abc.Mapping subclass with the simplest possible `__getitem__` - never triggers.
* abc.Mapping subclass with `__getitem__` calling `__missing__` - triggers on get or `__getitem__`.
# Random tips and tricks
* Line breaks are ignored inside of `[],{},()`.
* According to python API convention methods that  change the object in place should return None.
* When defining functions you can use `*` to define a parameter that accepts multiple values as a tuple.
* When defining functions you can use `**` to define a parameter that accepts multiple values as a dictionary.