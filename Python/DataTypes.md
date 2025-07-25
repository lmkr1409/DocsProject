- [Sequence Types](#sequence-types)
  - [sequence types - common topics](#sequence-types---common-topics)
  - [Slicing](#slicing)
    - [Syntax](#syntax)
- [Lists - Mutable](#lists---mutable)
    - [Unpacking and starred unpacking of a list:](#unpacking-and-starred-unpacking-of-a-list)
    - [Methods on lists:](#methods-on-lists)
    - [List Comprehension](#list-comprehension)
- [Tuples - Immutable](#tuples---immutable)
    - [Methods on tuples](#methods-on-tuples)
- [Named Tuples](#named-tuples)
    - [String formatting for namedtuple](#string-formatting-for-namedtuple)
- [Set Types](#set-types)
- [set](#set)
  - [Set Operations](#set-operations)
  - [set comprehension](#set-comprehension)
  - [set methods](#set-methods)
- [frozenset](#frozenset)
  - [frozenset methods](#frozenset-methods)
- [Mapping Types](#mapping-types)
- [Dictionaries](#dictionaries)
    - [dict declaration](#dict-declaration)
  - [dict methods](#dict-methods)
    - [Q. **What is the difference between view and iterable**?](#q-what-is-the-difference-between-view-and-iterable)
    - [Useful code](#useful-code)
  - [dict comprehensions:](#dict-comprehensions)
- [Default Dictionaries](#default-dictionaries)
  - [Factory Function](#factory-function)
- [Ordered Dictionaries](#ordered-dictionaries)

# Sequence Types

A sequence data type is the data type that supports the membership operator *in*, the size function *len()*, slices *[]* and iterable.

Python provides 5 built-n sequence types
* bytearray
* bytes
* str
* list
* tuple

## sequence types - common topics

## Slicing
Examples are using list, but applicable for all sequence types
* Slicing is a technique to extract part of the sequence using indexes. Big brackets [] are used for slicing the sequence
* Sequence starts with 0
* when using slicing endIdx is excluded. 
    * Ex: li[start: end] -> start: included, end: excluded

Consider below list 

        #x[-7]  x[-6]   x[-5]   x[-4]   x[-3]   x[-2]   x[-1]
    x = [1,     2,      3,      4,      5,      6,      7]
        #x[0]  x[1]    x[2]    x[3]    x[4]    x[5]    x[6]

### Syntax
```python
>>> x[start:end:step]
```
* start -> start index(inclusive)
* end -> end_index (excludes)
* step -> what is next index to look at

Examples:
```python
>>> x = [1,2,3,4,5,6,7]
>>> # Single idx slice
>>> x[0]
1
>>> x[-7]
1
```
When end index is not provided, slicing will run till last values of the list. And when start is provided then slice function assumes 0 as start
```python
>>> x[1:]
[2, 3, 4, 5, 6, 7]
>>> x[-2:]
[6, 7]
>>> x[:4]
[1, 2, 3, 4]
>>> x[:-3]
[1, 2, 3, 4]
>>> 
```
When both indexes provided, start is inclusive and end is exclusive
```python
>>> x[2:5] # idx 5 is excluded, so there won't be 6 in output
[3, 4, 5]
>>> x[-3:6]
[5, 6]
>>> x[-5:6]
[3, 4, 5, 6]
>>> x[-3:-5]
[]
>>> x[-5:-3]
[3, 4]
>>> x[3:-3]
[4]
```
With Step 
```python 
>>> x[1:6:1] # step =1, is default
[2, 3, 4, 5, 6]
>>> x[1:6:2] # step =2, skips every second element
[2, 4, 6]
>>> x[1:6:3] # step = 3, skips 2 elements every time 
[2, 5]
>>> x[1:6:4]
[2, 6]
>>> x[1:6:5]
[2]
>>> x[1:6:6]
[2]
>>> x[1:6:100]
[2]
```
Negative step - will iterate the list in reverse order. So, if the start is less than the end then output will be empty.
```python
>>> x[1:6:-1]
[]
>>> x[1::-1]
[2, 1]
>>> x[1:0:-1]
[2]
>>> x[7::-1]
[7, 6, 5, 4, 3, 2, 1]
>>> x[7:0:-1]
[7, 6, 5, 4, 3, 2]
>>> x[7:-7:-1]
[7, 6, 5, 4, 3, 2]
>>> x[7:-8:-1]
[7, 6, 5, 4, 3, 2, 1]
>>> x[7:-8:-2]
[7, 5, 3, 1]
>>>
```

# Lists - Mutable

* In Python a list is an ***ordered sequence*** of zero or more object references.
* Lists are **Mutable** objects.

### Unpacking and starred unpacking of a list:
```python
>>> a,b = [1,2]
>>> a
1
>>> b
2
>>> a, *b, c = [1,2,3,4,5,6]
>>> a
1
>>> b
[2, 3, 4, 5]
>>> c
6
>>> *a, b = [1,2,3,4,5,6]
>>> a
[1, 2, 3, 4, 5]
>>> b
6
>>> 
```

### Methods on lists:
Let **li** be the list object

| method | description|
| -------------------- | -- |
| li.append(x) | append item x to the list li|
| li.count(x) | count number of occurrences of item x in the list li|
| extend(m) <br/> or <br/> li +=li_b | append all items of iterable li_b to the list li|
| li.insert(i,x) | insert item x at position i |
| li.index(x, start, end) | Returns the first occurance of x, if x not found in the list raises **ValueError** |
| li.pop() | returns and removes last (or) right most element |
| li.pop(i) | returns and removes the item at index position int i |
| li.remove(x) | removes the first occurance of item x. If x not found in the list li then **ValueError** will be raised. | 
| li.reverse() | reverses the list | 
| li.sort() | sorts the list |
|

### List Comprehension
A list comprehension is an expression and a loop with an optional condition enclosed in brackets where the loop is used to generate items for the list and where the condition can filter out unwanted items.
```python
>>> [item for item in iterable]
>>> [expression for item in iterable]
>>> [expression for item in iterable if condition]
```

# Tuples - Immutable
A tuple is an ordered sequence of zero or more objects references.
* Same unpacking technique can be used to tuples which we used in lists

### Methods on tuples
Since tuples are immutable there are no methods which does the modification to the sequence.
| Method | Description |
| -- | -- |
| t.count(x) | returns the number of times object x appeared in the tuple t | 
| t.index(x) | index of first occurance of item x in the tuple <br/> **ValueError**: if x not found in tuple t |
|

> Tuples can be used with operators
* \+ concatenation
* \* replication
* [] slice

Note: 
* The += and *= assignment operators can be used with tuple overthought tuples are immutable. 
* Python will create new tuple and change the reference of the object to new tuple
* Tuples can be compared using the standard comparison operators
    * <, <=, ==, !=, >=, >
    * comparison will be applied item by item (and recursively for the nested items)

# Named Tuples
A named tuple behaves just like a plain tuple and has the same performance characteristics. What it adds is the ability to refer to items in the tuple by name as well by index position.
<br/> And this allows us to create aggregates of data items.

```python
>>> from collections import namedtuple
```
named tuple takes in 2 arguments.
1. Custome tuple datatype we want to create
2. A string of space separated names.One for each item that our custome tuple will take.

```python
>>> saleObj = namedtuple("sale", "pid cid quantity")
>>> s = saleObj(1,2,3)
>>> s
sale(pid=1, cid=2, quantity=3)
>>> s[0]
1
>>> s.pid
1
>>> s.cid
2
>>> 
```
For nested namedtuple
```python
>>> afObj = namedtuple("Aircraft", "manufacturer model seating")
>>> stObj = namedtuple("seating", "minimum maximum")
>>> a1 = afObj("Airbus", "A320", stObj(100,200))
>>> a1.seating.minimum
100
>>>
```
### String formatting for namedtuple 
In general string formatting is like below
```python
>>> "{} {}".format(a1.manufacturer, a1.model)
'Airbus A320'
>>>
```
In namedtuple we can use **_asdict()** private method for string formatting.
```python
>>> "{manufacturer} {model}".format(**a1._asdict())
'Airbus A320'
>>> 
```


# Set Types
* A set type is a collection of data items/types that supports membership operator(in), the size function len() and is iterable.
* Supports for bitwise operators
* Python support two built-in set types
    1. set - Mutable set type
    2. frozenset - immutable set type

Note: 
> only hashable objects can be added to a set.

* Hashable objects are objects which have a \_\_hash\_\_() function whose return values is always same throughout the objects lifecycle <br/> and which can be compared for equality using the \_\_eq()\_\_ special method.

> special methods in python are methods whose name begins and ends with 2 underscores <br/>
> Ex: \_\_init()\_\_, \_\_len()\_\_, \_\_eq()\_\_, \_\_hash()\_\_,etc

> All built-in immutable data types such as float, int, frozenset, str, tuple are hashable and can be added to sets.

> The built-in mutable data types such as dict, list and set are not hashable since their hash value changes depends on the items they contain.


# set

* A set is an **unordered collection** of zero or more object references that refer to hashable objects.
  
-> Mutable<br/>
-> since sets are unordered they have no notion of index position and so cannot be sliced or strided.

```python
>>> s = set()
>>> s
set()
>>> s.add(1)
>>> s.add(2)
>>> s
{1, 2}
>>> s1 = set(s) # Shallow copy of set s
>>> s1
{1, 2}
>>> s2 = set([1,2,3, 2, 1]) # set from list
>>> s2
{1, 2, 3}
>>> 
```

Note:
> Empty set should be created with set() method as {} is used for dict creation

> set always contains unique item

-> ```set('apple')```, ```set('aple')```, ```set(['e', 'p', 'a', 'l'])``` all same<br/>
-> To access elements either loop through set or convert the set to list by using ```list(set) ```

## Set Operations

| operation | operator | example |
| -- | -- | -- |
| union | \| | ```set("pecan") \| set("pie") => {'p','e','c','n','i'}``` |
| intersection | & | ```set("pecan") & set("pie") => {'p','e'}``` |
| difference | - | ```set("pecan") - set("pie") => {'c','a','n'}``` |
| symmetric difference | ^ | ```set("pecan") ^ set("pie") => {'c','n','i'}``` |
|

## set comprehension 
A set comprehension is an expression and a loop with an optional conditon enclosed in braces.
```python
>>> {expression for item in iterable}
>>> {expression for item in iterable if condition}
>>>
```
An Example
```python
>>> all_html_files = {x for x in file_names if x.lower().endswith('.html')}
>>> 
```

## set methods

let <br/>
-> s - set <br/>
-> t - another set <br/>
-> x - variable
| method | description |
| -- | -- |
| s.add(x) | Add item x to set s if item x is not already present in set s |
| s.clear() | Removes all the items | 
| s.copy() | Returns a shallow copy | 
| s.pop() | Removes and returns random item from set, raises **KeyError** if set is empty |
| s.discard(x) | removes x |
| s.remove(x) | Removes x or raises **KeyError** |
| s.difference(t) <br/> s-t | Returns new set with s-t |
| s.difference_update(t) <br/> s-=t | Removes every item from s which is in t |
| s.intersection(t) <br/> s&t | Returns a new set which has items in both s and t |
| s.intersection_update(t) <br/> s&=t | updates s with s&t |
| s.isdisjoint(t) | Returns True if s and t are disjoints | 
| s.issubset(t) <br/> s<=t | Returns True if s is subset of t |
| s.issuperset(t) <br/> s>t | Returns True if s is superset of t|
| s.symmetric_difference(t) <br/> s^t| Returns new set with s^t |
| s.symmetric_difference_update(t) <br/> s^=t | Updates s with s^t |
| s.union(t) <br/> s\|t | Returns new set with union of s and t |
| s.update(t) <br/> s=\|t | Update as union |
|


# frozenset

-> A frozenset is immutable<br/>
-> can only be created with ```frozenset()```
```python
>>> frozenset()
frozenset()
>>> fs = frozenset([1,2,3])
>>> fs
frozenset({1, 2, 3})
>>> fs2 = frozenset(fs) # shallo copy
>>> 
```
> since frozenset s are immutable they support only those methods and operations that produce a result without affecting the frozenset

> If a binary operator is used with a set and frozenset then the result will be <br/>
> ```f&s => returns frozenset <br/>``` <br/>
> ```s&f => returns set``` <br/>
> ```== or != => returns True/False```

## frozenset methods
let <br/>
-> s - frozenset <br/>
-> t - another frozenset <br/>
-> x - variable
| method | description |
| -- | -- |
| s.copy() | Returns a shallow copy | 
| s.difference(t) <br/> s-t | Returns new set with s-t |
| s.intersection(t) <br/> s&t | Returns a new set which has items in both s and t |
| s.isdisjoint(t) | Returns True if s and t are disjoints | 
| s.issubset(t) <br/> s<=t | Returns True if s is subset of t |
| s.issuperset(t) <br/> s>t | Returns True if s is superset of t|
| s.symmetric_difference(t) <br/> s^t| Returns new set with s^t |
| s.union(t) <br/> s\|t | Returns new set with union of s and t |
|

<br/>
<br/>

# Mapping Types
* A mapping type is one that supports the membership operator(in) and the size function len() and is iterable.

-> Only hashable objects may be used as dictionary key. So immutable objects like float, frozenset, int, str and tuple can be used dictionary keys but mutable objects like dict, list, set cannot. <br/>
-> Dictionaries are mutable objects <br/>
-> Dictionaries can be compared using <br/>
* == and != with comparisons being applied item by item (and recursively for nested items), <br/>
* comparison using <, <=, >=, > are not supported

-> few types 
* dictionary (default)
* collections.defaultdict
* collections.ordereddict

# Dictionaries
A dict is an unordered collection of zero or more key-value pairs whose keys are object references the refer to the hashable objects and whose values are object references to objects of any type.<br/>
-> Dictionaries are mutable<br/>
-> Since unordered, no notion of index position and cannot be sliced or strided.<br/>
-> ```d = dict()```<br/> &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(or) &nbsp;&nbsp;&nbsp;&nbsp; <br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```d={}``` ---> creates an empty dictionary<br/>
->dict(another_dict) -> creates a shallow copy

### dict declaration
```python
>>> d1 = dict({"id":1948, "name":"washer", "size":3})
>>> d2 = dict(id=1948, name="washer", size = 3)
>>> d3 = dict([("id", 1948), ("name","washer"), ("size", 3)])
>>> d4 = dict(zip(("id","name","size"),(1948, "washer", 3)))
>>> d5 = {"id":1948, "name":"washer", "size":3}
>>>
```
All above 5 dictionaries will be same
<br/>
-> Dictionaries support the build in len() function<br/>
-> Accessing dictionaries items can be
```python
>>> for key, value in d1.items():
...     print(key, value)
... 
id 1948
name washer
size 3
>>> 
```

## dict methods
|Method | description |
| -- | -- |
| clear() | Removes all items|
| copy() | Returns shallow copy | 
| fromkeys(S, V) | Returns a dict whose keys are the items in the sequence S and whose values are None or V if V is given |
| get(K) | Returns K's values, None if K isn't in the dict.<br/>           **Note**: d[K] will raise **KeyError** if K is not in the dict|
| get(K, V) | Returns K's value if K is in dict. If K not in dict then V | 
| items() | Returns a **view** of all (Key, Values) pairs |
| keys() | Returns a view of all Keys|
| values() | Returns a view of all the values from d |
| pop(k) | Returns K's associated value and removes K from dict, raises **KeyError** if K not in dict |
| pop(K, V) | Returns K's associated value and removes K from dict, If K is not in dict then V is returned |
| popitem() | Returns and removes arbitary (Key, Value) pair from d, raise **KeyError** if d is empty |
| setdefault(K, V) | Returns K's Value. If K is not in d then a new K is created with None/V provided V|
| update(a) | updates d with all Key, value from a. Overrides of same with with value from a |
|

### Q. **What is the difference between view and iterable**?

A view helps to get a new view of array with the same data. Two things makes a view different from a normal iterable.
1. If the datatype the view refers to is changed then the view reflects the change
2. Views support set-like operations provides objects of view are not sequences. Ex: Keys and values of dict not items of dict

```python
>>> d = {1:2, 2:3, 3:4}
>>> d.keys()
dict_keys([1, 2, 3])
>>> d.values()
dict_values([2, 3, 4])
>>> d.items()
dict_items([(1, 2), (2, 3), (3, 4)])
>>> 
>>> d = {"A":3, "B":4, "C":5, "D":6}
>>> s = set("ACX")
>>> s
{'A', 'X', 'C'}
>>> d.keys() & s
{'A', 'C'}
>>> 
```

* view supported set operations

V & X => intersection <br/>
V \| X => union<br/>
V - X => difference<br/>
V ^ X => symmetric difference<br/>

### Useful code
* When implementing a counter kind of operation using dict one way of doing is 

```python
>>> d = {}
>>> for ch in [1,2,3,4,3,2]:
...     try:
...             d[ch] +=1
...     except KeyError:
...             d[ch] = 1
... 
>>> d
{1: 1, 2: 2, 3: 2, 4: 1}
>>> 
```
we can use get method 
```python
>>> d = {}
>>> for ch in ['a','b','c','a','b','a']:
...     d[ch] = d.get(ch,0)+1
... 
>>> d
{'a': 3, 'b': 2, 'c': 1}
>>> 
```
for list or set
```python
>>> d = {}
>>> for idx, ch in enumerate(['a','b','c','a','b','a']):
...     d.setdefault(ch, list()).append(idx)
... 
>>> d
{'a': [0, 3, 5], 'b': [1, 4], 'c': [2]}
>>> 
```
## dict comprehensions:

```python
>>> {keyexpression: valuesexpression for key, value in iterable}
>>> {keyexpression: valueexpression for key, values in iterable if condition}
>>> inverted_d = {V:K for K, V im d.items()}
```

<br/>

# Default Dictionaries
Default dictionaries are dictionaries, they have all the operators and methods that dictionaries provide.<br/>
-> Default dictionary is a subclass of dict <br/>
-> The difference between dictionary and default dictionary is that the way how it handles missing keys.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
d[key] will raise **KeyError** in dict<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
d[key] never raises **KeyError**, it will create a default value with the key.<br/>
-> defaultdict takes factory function as input while creation

## Factory Function
* A factory function is a function that when called returns an object of a particular type. 
* All of python's built-in data types can be used as factory functions.

Ex:
Data type str can be called as str() and it returns an empty string object.<br/>
* Name of a function is an object reference to the function 
  - so when we pass function as parameter we pass the name
  - when we use function with parenthesis, then python will call the function.

```python
>>> from collections import defaultdict
>>> d = defaultdict(int)
>>> d[5]
0
>>> d
defaultdict(<class 'int'>, {5: 0})
>>> 
>>> d = defaultdict(list)
>>> d
defaultdict(<class 'list'>, {})
>>> d[5].append(7)
>>> d
defaultdict(<class 'list'>, {5: [7]})
>>> d[6]
[]
>>> d
defaultdict(<class 'list'>, {5: [7], 6: []})
>>> 
```

# Ordered Dictionaries
* The ordered dictionaries store their items in order in which they were inserted.
* If an ordered dictionary is passed with unordered dictionary then the result may be unordered
* A similar effect occurs with update() method
* Best to avoid these 2 when using ordered dict

```python
>>> from collections import OrderedDict
>>> 
```
Note: [dict's retaining insertion is guaranteed for python3.7](https://stackoverflow.com/questions/39980323/are-dictionaries-ordered-in-python-3-6), So, no need to specifically use OrderedDict

