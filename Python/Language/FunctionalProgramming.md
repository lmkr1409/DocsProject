- [Functional Programming](#functional-programming)
  - [Characteristics of Functional Programming](#characteristics-of-functional-programming)
    - [1. Function as the first class objects](#1-function-as-the-first-class-objects)
    - [2. Pure Functions](#2-pure-functions)
    - [3. Reducing the usage of Loops](#3-reducing-the-usage-of-loops)
    - [4. Recursion](#4-recursion)
- [Advanced Python](#advanced-python)
  - [Higher Order Functions](#higher-order-functions)
  - [Closures](#closures)
  - [Decorators](#decorators)

# Functional Programming

- Functional programming is a coding style that focuses on defining what to do, instead of performing some actions.
- In functional code, the output of the function depends only on the arguments that are passed.
- Calling the function f, for the same values x should return the same result f(x) no matter how many times you pass it.
- Instead of moving through steps, you think of data as undergoing transformations with the desired result as the end state.

## Characteristics of Functional Programming

A functionally pure language should support the following characteristics

1. **Functions as first class objects**, which means that you should be able to apply all the constructs of using data to functions as well.
2. **Pure Functions**, there should not be any side effects.
3. **Limit the Use of loops**, Ways and constructs to limit the use of for loops.
4. **Recursion**, There should be a good support for recursion.

### 1. Function as the first class objects

In python functions are first class objects. Using functions as the first class objects means to use them in the same manner that you use data. So, you can pass them as parameters like passing a function to another function as argument.

1. passing to another function
   ```python
   >>> list(map(str, [1,2,3,4]))
   ['1', '2', '3', '4']
   ```
   - Here `str()` is passes to `map()` as an argument.
2. assigning functions to variables
   - We can assign them to variables and return them
   ```python
   >>> def hello_world(h):
   ...     def world(w):
   ...         print(h,w)
   ...     return world
   ...
   >>> h = hello_world
   >>> x = h('hello')
   >>> h
   <function hello_world at 0x000002128CCD6020>
   >>> x
   <function hello_world.<locals>.world at 0x000002128CCD6660>
   >>> x('world')
   hello world
   ```
3. we can store functions in various data structures.
   ```python
   >>> function_list = [h,x]
   >>> function_list
   [<function hello_world at 0x000002128CCD6020>, <function hello_world.<locals>.world at 0x000002128CCD6660>]
   ```

### 2. Pure Functions

There are various built-in functions in python that can help to avoid procedural code in functions. Something like below

```python
>>> def native_sum(my_list):
...     s = 0
...     for l in my_list:
...         s+=l
...     return s
...
>>> sample_list = [1,2,3,4,5]
>>> native_sum(sample_list)
15
```

can be replaced with

```python
>>> sum(sample_list)
15
```

Similarly, built-in functions such as map, reduce and the itertools module in python can be utilized to avoid side effects in your code.

### 3. Reducing the usage of Loops

- Loops come into picture when you want to loop over a collection of objects and apply some kind of logic or function.

```python
>>> for x in l:
        func(x)
```

- The above construct seems from the traditional thinking of visualizing the whole program as a series of steps where you define how things needs to be done.
- Making this more functional will need a change in thinking pattern.
- Above for loop can be replaced as `map(func, l)`. This is read as 'map the function to the list',
- If we take this idea and apply it to the sequential execution of functions

```python
>>> def func1():
...     return "func1"
...
>>> def func2():
...     return "func2"
...
>>> def func3():
...     return "func3"
...
>>> map(lambda f: f(), [func1, func2, func3])
<map object at 0x000002128CCD2B90>
```

`map` does not actually run the functions, but returns a lazy map object. You need to pass this object to a list or any other eager functions to have the code executed.

```python
>>> list(map(lambda f: f(), [func1, func2, func3]))
['func1', 'func2', 'func3']
```

### 4. Recursion

Recursion is a method of breaking a problem into subproblem's which are essentially of the same type as the original problem. usually this involves function calling itself. An example with binary search

```python
>>> def binary_search(my_list, element, low, high):
...     if low > high:
...         return -1
...     mid  = int((low+high)/2)
...     if my_list[mid] == element:
...         return mid
...     elif my_list[mid] < element:
...         return binary_search(my_list, element, mid+1, high)
...     else:
...         return binary_search(my_list, element, low, mid-1)
...
>>>
>>> sample_list = [1,2,3,4,5,6,7,8]
>>> binary_search(sample_list, 5, 0, len(sample_list))
4
>>> binary_search(sample_list, 8, 0, len(sample_list))
7
```

An example for converting procedural code into functional code

```python
>>> num = 96
>>> sq = num**2
>>> inc = sq + 1
>>> cube = inc ** 3
>>> dec = cube - 1
>>> dec
783012621312
```

```python
>>> def call(x, f):
...     return f(x)
...
>>> sq = lambda x: x**2
>>> inc = lambda x: x+1
>>> cube = lambda x: x ** 3
>>> dec = lambda x: x-1
>>> funcs = [sq, inc, cube, dec]
>>> from functools import reduce
>>> reduce(call, funcs, 96)
783012621312
```

# Advanced Python

## Higher Order Functions

In python, functions are treated as first class objects, allowing us to perform the following operations as functions.

- A function can take one or more functions as arguments
- A function can be returned as a result of another function

**Functions as arguments**:

- we can pass functions as one of the arguments to another function

```python
>>> def summation(nums): # Normal function
...     return sum(nums)
...
>>> def main(f, *args): # functions as an argument
...     result = f(*args)
...     print(result)
...
>>> main(summation, [1,2,3])
6
```

- The main function took in the function summation as an argument
- The main function is a normal function which executes the supplied function with the arguments.
- This opens up possibilities where ypu can pass different functions to a function and the passed function only will be considered.

**Function as a return value**

```python
>>> def add_2_nums(x, y):
...     return x + y
...
>>> def add_3_nums(x, y, z):
...     return x + y + z
...
>>> def get_appropriate_function(num_len):
...     if num_len == 3:
...         return add_3_nums
...     else:
...         return add_2_nums
...
>>> args = [1,2,3]
>>> num_len = len(args)
>>> res_func = get_appropriate_function(num_len)
>>> res_func
<function add_3_nums at 0x000002128CCD7060>
>>> res_func(*args)
6
>>> args = [1,2]
>>> num_len = len(args)
>>> res_func = get_appropriate_function(num_len)
>>> res_func
<function add_2_nums at 0x000002128CCD6FC0>
>>> res_func(*args)
3
```

## Closures

A closure is a way of keeping alive a variable even when the function has returned. In a closure a function is defined along with environment. In Python, this is done by nesting a function inside the encapsulating function and then returning the underlying function.

Another definition:

In simplest terms, a closure is a function returned by a higher order function whose return value depends on the data associated with the higher order function.

```python
>>> def multiple_of(x):
...     def multiple(y):
...         return x*y
...     return multiple
...
>>> c1 = multiple_of(5) # C1 is a closure
>>> c2 = multiple_of(6) # C2 is a closure
>>>
>>> c1(4)
20
>>> c2(4)
24
>>> c1
<function multiple_of.<locals>.multiple at 0x000002128CCD72E0>
>>> c2
<function multiple_of.<locals>.multiple at 0x000002128CCD7380>
```

- You can observe from the example that the closure functions C1 and C2 holds the data passed to enclosing function, multiple_of during their execution.
- The first closure function, c1 binds the value 5 to the argument '5' and when called with an argument 4, it executes the body of multiple function and returns the product of 5 and 4.

## Decorators

- The ability to build higher order functions allows a programmer to create closures, which in tern are used to create decorators.
- Python decorators are convenient ways to make changes to the functionality of code without making changes to the code.
- A decorator is written as a function closure and implemented by giving `@` operator n top of the function.

Another definition

- A decorator function is a higher order function that takes a function as an argument and returns the inner function.
- The skeleton of a python decorator is show below

```python
>>> def my_decorator(func):
...     # Code for wrapping function
...     # return the wrapping function
...     pass
...
>>>
>>> @my_decorator
... def my_normal_function(args):
...     # Original functionality
...     pass
...
>>>
>>> my_normal_function(123)
```

- Using `@` operator on tp of the function is same as writing `my_decorator(my_normal_function(123))`
- Ex: Following function returns a dictionary

```python
>>> def my_code(args):
...     return {"lang": args}
...
>>> my_code('python')
{'lang': 'python'}
```

- say, you want to build the additional functionality if the return value is a dict.

```python
>>> def check(func):
...     def wrapper(*args, **kwargs):
...         res = func(*args, **kwargs)
...         if isinstance(res, dict):
...             print("checked, correct")
...             return res
...     return wrapper
...
>>> @check
... def my_code(args):
...     return {"lang": args}
...
>>> my_code('python')
checked, correct
{'lang': 'python'}
```

Here, function check is used as decorator and it enforces the additional functionality of checking if the return of the function `my_code` is a dict or not.
