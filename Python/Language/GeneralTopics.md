- [Mutable Objects vs Immutable Objects](#mutable-objects-vs-immutable-objects)
  - [Key Note:](#key-note)
- [Object References](#object-references)
  - [Q. **How objects are passed to functions?**](#q-how-objects-are-passed-to-functions)
  - [Q. **Is Python call by value (or) call by reference?**](#q-is-python-call-by-value-or-call-by-reference)
  - [Copy Mechanism in python](#copy-mechanism-in-python)
    - [Q. **What is the difference between shallow copy vs deep copy?**](#q-what-is-the-difference-between-shallow-copy-vs-deep-copy)
    - [1. Object Reference](#1-object-reference)
    - [2. copy/shallocopy](#2-copyshallocopy)
    - [3. deepcopy](#3-deepcopy)
- [Iterating and copying collections](#iterating-and-copying-collections)
  - [Iterators and Iterable operations](#iterators-and-iterable-operations)
    - [Key Points](#key-points)

# Mutable Objects vs Immutable Objects

Everything in python is an object. All the objects can be either mutable or immutable.
-> Python handles mutable and immutable objects differently.

| Mutable Objects                                                                                        | Immutable Objects                                                                                                      |
| ------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------- |
| An object which can change when there is some update operation happened. In Python mutable objects are | An object which can't be changed with any operation over the lifecycle of that object. In Python immutable objects are |
| Changing mutable objects is cheap                                                                      | Expensive to change the objects, because doing so involves creating a copy                                             |
| Slow to access compared to Immutable objects                                                           | Quicker to access than mutable objects                                                                                 |
| Mutable objects are great to use when you need to change the size of the object.                       | Immutable objects are used when you need to ensure that the objects created will always the same.                      |
| list, dict, set, bytearray                                                                             | int, float, complex, str, bytes, tuple, frozenset                                                                      |

### Key Note:

> The bindings of immutable objects are unchangeable, not the objects they are bound to.

Example:

```python
>>> t = ('x', [1,2])
>>> t[1] = [1,2,3]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
>>> t
('x', [1, 2])
>>> t[1].append(3)
>>> t
('x', [1, 2, 3])
>>>
```

-> tuple t, is immutable but not the objects bound to the tuples. Since list is mutable we are able to change list object bound to the tuple t.

<br/>

# Object References

Python doesn't have variables as such but instead has object references.

- when it comes to immutable objects like int, tuple there is such difference. As for mutable objects there is a difference.

### Q. **How objects are passed to functions?**

If the arguments created while function declaration are mutable then calling the function acts like call-by-reference.

If the arguments created while function declaration are immutable then calling the function acts like call-by-value

| Mutable Object as arguments                                                                                                                                                                                            | Immutable Object as arguments                                                                                                                                                                      |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| When a mutable object is passed to a function, object can change inside the function as the object is mutable hence mutable parameters act like call-by-reference                                                      | When a mutable object is passed to a function, object can't change hence immutable parameters act like call-by-value                                                                               |
| To avoid modifying the original object we need to copy the object to another variable                                                                                                                                  | No need to copy the object as they can't change anyway                                                                                                                                             |
| <pre>&gt;&gt;&gt; def updateList(list1):<br/>... list1+=[10]<br/>... <br/>&gt;&gt;&gt; n = [5,6]<br/>&gt;&gt;&gt; n<br/>[5, 6]<br/>&gt;&gt;&gt; updateList(n)<br/>&gt;&gt;&gt; n<br/>[5, 6, 10]<br/>&gt;&gt;&gt;</pre> | <pre>&gt;&gt;&gt; def updateNumber(num):<br/>... num+=10<br/>... <br/>&gt;&gt;&gt; n = 5<br/>&gt;&gt;&gt; n<br/>5<br/>&gt;&gt;&gt; updateNumber(n)<br/>&gt;&gt;&gt; n<br/>5<br/>&gt;&gt;&gt;</pre> |
| here passing call-by-reference                                                                                                                                                                                         | here passing is call-by-value                                                                                                                                                                      |

<br/>

### Q. **Is Python call by value (or) call by reference?**

A. Python uses a mechanism which is known as _call-by-object_ sometimes also called _call by object reference_ or _call by sharing_

- If we pass immutable objects arguments like integers, strings, tuples to a function then such passing will act like _call by value_

  - The objects passed to the function as parameters can't be changed because they can't be changed at all.

- If we pass mutable objects as arguments like list, set to a function then such passing acts like _call by reference_

<br/>

## Copy Mechanism in python

In general when we assign a object to another temp object, python will just create a reference to the main object to temp object.

-> If the object mutable then any change on temp object will reflect on the main object.

```python
>>> listObject = [1,2,3]
>>> tempObject = listObject
>>> listObject
[1, 2, 3]
>>> tempObject
[1, 2, 3]
>>> tempObject[0] = 9
>>> tempObject
[9, 2, 3]
>>> listObject
[9, 2, 3]
>>>
```

One Exception here is, If we assign brand new list to tempObject and modify tempObject then there won't be any affect on listObject

```python
>>> l = [1,2,3]
>>> t = l
>>> l
[1, 2, 3]
>>> t
[1, 2, 3]
>>> t = [4,5,6]
>>> l
[1, 2, 3]
>>> t
[4, 5, 6]
>>> t[0] = 9
>>> l
[1, 2, 3]
>>> t
[9, 5, 6]
>>>
```

### Q. **What is the difference between shallow copy vs deep copy?**

> copy or shallocopy will copy the objects of the reference object not the objects bound to the object

> deepcopy will copy the object bound to the referenced objects as well

| Shallow Copy                                                                                  | Deep Copy                                                                                    |
| --------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| Shallow Copy stores the references of objects to the original memory address.                 | Deep copy stores copies of the object’s value.                                               |
| Shallow Copy reflects changes made to the new/copied object in the original object            | Deep copy doesn’t reflect changes made to the new/copied object in the original object.      |
| Shallow Copy stores the copy of the original object and points the references to the objects. | Deep copy stores the copy of the original object and recursively copies the objects as well. |
| Shallow copy is faster.                                                                       | Deep copy is comparatively slower.                                                           |

### 1. Object Reference

Object Reference will just create an reference to the original object, any modifications from the reference object will update the original object.

```python
>>> obj = [1,2,3,[4,5]]
>>> temp = obj # Object reference not copy/shallocopy
>>> temp
[1, 2, 3, [4, 5]]
>>> temp[0] = 9
>>> obj
[9, 2, 3, [4, 5]]
>>>
```

### 2. copy/shallocopy

While copy we copy objects of the original list then references created for each object from original object to temp object. When some object updated in temp object then

- if the updated object is immutable then there won't be any affect on the original object

```python
>>> obj = [1,2,3,[4,5]]
>>> temp = obj[:] #Creates a copy
>>> temp[0] = 9
>>> temp
[9, 2, 3, [4, 5]]
>>> obj #Note obj is not getting updated since objects are copied not just list reference
[1, 2, 3, [4, 5]]
>>>
>>> from copy import copy
>>> temp = copy(obj)
>>> temp
[1, 2, 3, [4, 5]]
>>> temp[1] = 9
>>> temp
[1, 9, 3, [4, 5]]
>>> obj
[1, 2, 3, [4, 5]]
>>>
```

- if the updated object is mutable then the original object will be updated

```python
>>> obj = [1,2,3,[4,5]]
>>> temp = obj[:]
>>> temp[3][0] = 9
>>> temp
[1, 2, 3, [9, 5]]
>>> obj
[1, 2, 3, [9, 5]]
>>>
```

### 3. deepcopy

To avoid the updating mutable objects as well from the copied object we can use deepcopy

```python
>>> from copy import deepcopy
>>> obj = [1,2,3,[4,5]]
>>> temp = deepcopy(obj)
>>> temp[3][0]=9
>>> temp
[1, 2, 3, [9, 5]]
>>> obj
[1, 2, 3, [4, 5]]
>>>
```

# Iterating and copying collections

## Iterators and Iterable operations

- An iterable data type is one that can return each of its items one by one at a time.
- Any object
  - that has `__iter__()` method, or
  - any sequence (an object that has `__getitem__()` method taking integer arguments starting from 0)
    <br/>
    is an iterable and can provide an `iterator`.

**Iterable object**: Provides `__next__()` and raises `StopIteration`

**iter()**:<br/>

```python
>>> type(iter)
<class 'builtin_function_or_method'>
>>>
```

- Iter will take at-least one parameter, raises a **TypeError** if the passed parameters are not acceptable.

1. When called with one argument of a collection data type, then `iter` will return an iterator object for the given type
2. when passed with a function as callable and sentinal value then `iter` will return the functions return value each time and raises `StopIteration` if the return value is equal to sentinal.

```python
>>> global val
>>> def func():
...     global val
...     val+=1
...     return val
...
>>> val = 0
>>> i = iter(func, 3)
>>> i.__next__()
1
>>> i.__next__()
2
>>> i.__next__()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
>>>
```

> for each loop will use `iter` inside. Loop will get terminated when `StopIteration` exception occurred.

> Big point to remember while using iterables are calling next methods.

We can use following methods on an iterable. <br/>
let `i` be the iterable.
| Method| Description|
| -- | -- |
| all(i) | Returns `True` if all if `i` are evaluated to `True` |
| any(i) | Returns `True` if any if `i` is evaluated to `True` |
| enumerate(i) | Returns index, value |
| max(i) | Returns biggest one |
| min(i) | Returns smallest one |
| zip(i1, i2, ---, im) | zip of all iterables |

### Key Points

**KeyPoints** 1:

- all(i) -> if any value in iterable is evaluated to `False` then it will return `False` and raises `StopIteration` at that point itself
- So, `__next__()` will fetch next element.

```python
>>> i = iter([1,2,0,3,4])
>>> all(i)
False
>>> i.__next__()
3
>>> next(i)
4
>>>
```

```python
>>> i = iter([1,2,3,4])
>>> all(i)
True
>>> i.__next__()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
>>>
```

**Key Point** 2:<br/>

- If any value in iterable evaluates to `True` then iteration will return `True`
- and iteration will stop with that element itself

```python
>>> i = iter([1,2,3,4])
>>> any(i)
True
>>> next(i)
2
>>>
```

**Key Point** 3: Note below that iteration is updated the accessing value when we hit `i.__next__()`. So next time when we called next on enumeration we got index=1 and value=3 instead of 2.

```python
>>> i = iter([1,2,3,4])
>>> ei = enumerate(i)
>>> ei.__next__()
(0, 1)
>>> i.__next__()
2
>>> ei.__next__()
(1, 3)
>>>
```

**Key Point** 4:

- `min`, `max`, `sum` methods will exhaust the iterable and next method will raise `StopIteration`.

**Key Point** 5:

- zip(i1, i2, -----, im)
- Returns a zip object which will return tuples in calling the next method.
- But it will not start the iterable.

```python
>>> a1 = iter([1,2,3,4,5])
>>> a2 = iter(['a','b','c','d','e'])
>>> z = zip(a1, a2)
>>> z.__next__()
(1, 'a')
>>> a1.__next__()
2
>>> next(z)
(3, 'b')
>>>
```

When `iter` creates another iterable with `a1`, new iterable is pointing to a1 but the zip `z` is still holding the old iterable

```python
>>> a1 = iter([9,8,7,6,5])
>>> next(z)
(4, 'c')
>>> a1.__next__()
9
>>> z.__next__()
(5, 'd')
>>>
```

**Key Point** 6:

- `sorted()` supports iterable as a parameter but `reversed()` doesn't

```python
>>> a1 = iter([5,4,3,2,1])
>>> a1
<list_iterator object at 0x7f27f5378eb0>
>>> sorted(a1)
[1, 2, 3, 4, 5]
>>>
```

```python
>>> a2 = iter([5,4,3,2,1])
>>> reversed(a1)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'list_iterator' object is not reversible
>>>
```
