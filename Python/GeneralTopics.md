- [Iterating and copying collections](#iterating-and-copying-collections)
  - [Iterators and Iterable operations](#iterators-and-iterable-operations)
    - [Key Points](#key-points)


# Iterating and copying collections
## Iterators and Iterable operations
* An iterable data type is one that can return each of its items one by one at a time. 
* Any object 
  * that has `__iter__()` method, or
  * any sequence (an object that has `__getitem__()` method taking integer arguments starting from 0)
  <br/>
  is an iterable and can provide an `iterator`.

**Iterable object**: Provides `__next__()` and raises `StopIteration` 

**iter()**:<br/>
```python
>>> type(iter)
<class 'builtin_function_or_method'>
>>> 
```
* Iter will take at-least one parameter, raises a **TypeError** if the passed parameters are not acceptable.

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
* all(i) -> if any value in iterable is evaluated to `False` then it will return `False` and raises `StopIteration` at that point itself
* So, `__next__()` will fetch next element.
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
* If any value in iterable evaluates to `True` then iteration will return `True`
* and iteration will stop with that element itself

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
* `min`, `max`, `sum` methods will exhaust the iterable and next method will raise `StopIteration`.

**Key Point** 5:
* zip(i1, i2, -----, im)
* Returns a zip object which will return tuples in calling the next method.
* But it will not start the iterable.
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
* `sorted()` supports iterable as a parameter but `reversed()` doesn't
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