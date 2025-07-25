- [Functions](#functions)
  - [1. Global functions](#1-global-functions)
  - [2. Local functions](#2-local-functions)
  - [3. Lambda functions](#3-lambda-functions)
  - [4. Methods](#4-methods)
    - [Q. **Parameters vs Arguments** ?](#q-parameters-vs-arguments-)
      - [**Scope of Variable and need of global statement:**](#scope-of-variable-and-need-of-global-statement)
- [Arguments of Functions](#arguments-of-functions)
  - [1. Default Argument Values:](#1-default-argument-values)
  - [2. Keyword arguments:](#2-keyword-arguments)
  - [3. Arbitrary Arguments lists](#3-arbitrary-arguments-lists)
    - [*arguments, **keywords](#arguments-keywords)
  - [4. Unpacking argument list](#4-unpacking-argument-list)
- [lambda expressions](#lambda-expressions)
- [Nested functions (Inner functions)](#nested-functions-inner-functions)

# Functions

* Functions are a means by which we can package up and parameterize functionally
* Four kinds of functions can be created in python
    1. Global functions
    2. Local functions
    3. lambda functions
    4. Methods

## 1. Global functions
* Every normal function will be global function
* Global objects/functions can be accessible in same or other python files
```python
>>> def add(x, y):
...     return x+y
... 
>>> add(5,6)
11
>>>
```
## 2. Local functions
* Local functions are defined in other functions (nested).
* These are visible where there are created

## 3. Lambda functions
* `lambda` functions are expressions. So these can be created at their point of use.
* These functions are much more limited than normal functions
```python
>>> lf = lambda x, y: x+y
>>> lf(5,6)
11
>>> 
```
## 4. Methods
* Methods are functions that are associated with a particular data type and can only be used in conjunctions with the data type.

### Q. **Parameters vs Arguments** ?
`A parameter is a variable in a method definition`. When a method is called the `arguments` are the the data you pass into the methods parameter.
```python
def func(parameters):
    # Function body

args = 'arguments'
func(args)
```
```python
>>> def add(x, y): # x, y are formal parameters
...     return x+y
... 
>>> a = 10
>>> b = 20
>>> add(a, b) # a, b are actual parameters or arguments
30
>>> 
```
* If we pass incorrect number of arguments then TypeError will be raised
* The first statement of the function body can optionally be a string literal which is also called as `docstring`

#### **Scope of Variable and need of global statement:**
* The execution of a function introduces a new symbol table used fr that local variable of the function (or) All the variable assignments in a function store the value in a local symbol table.
* Variable references first look in the local symbol table then n the local symbol tables enclosing functions then in the global symbol table and finally in the table of built-in names
* So, global variables cannot be directly assigned a value within a function (unless named in a global statement) although they may be referenced.
* A function definition introduces the function name in the current symbol table.
* The value of the function name has a type that is recognized by the interpreter as a user-defined functions
```python
>>> def func():
...     return 5
... 
>>> func
<function func at 0x7f90947923a0>
>>> func()
5
>>> 
```
* This value can be assigned to another name which can also be used as a function. This servers as a general renaming mechanism.
```python
>>> f = func
>>> f
<function func at 0x7f90947923a0>
>>> f()
5
>>> 
```
* If we observe both functions func and f are reffering to same address
* Using the value `None` is normally suppressed by the interpreter if it would be the only value returned
```python
>>> def func(n):
...     if n<5:
...             return n
... 
>>> func(5)
>>> print(func(5))
None
>>> 
```

# Arguments of Functions
* It is possible to define functions with different no.of parameters
## 1. Default Argument Values:
* The most useful form is to specify a default value for one or more arguments
* This creates a function that can be called with fewer arguments than it is defined to allow
```python
>>> def func(a, b=1, c=2):
...     return a+b+c
... 
>>> func(1)
4
>>> func(1,2)
5
>>> func(1,2,3)
6
>>> 
```
**Note 1:** The default values are evaluated at the point of function definition in the defining scope.
```python
>>> i = 5
>>> def func(arg=i):
...     print(arg)
... 
>>> i = 6
>>> func()
5
>>> func(10)
10
>>> 
```
**Note 2:** 
* The default value is evaluated only once
* This makes a difference when the default is a mutable object such as list, dict, set.
```python
>>> def func(a, L=[]):
...     L.append(a)
...     return L
... 
>>> func(1)
[1]
>>> func(2)
[1, 2]
>>> func(3)
[1, 2, 3]
>>> 
```
## 2. Keyword arguments:
Functions can also be called using keyword arguments of the form `kwarg=value`
```python
>>> def f(a, b=1, c=2, d=3):
...     print(a, b, c, d)
... 
>>> 
```
* Above function accepts one required argument and 3 optional arguments (b, c, d). 
* Above function can be called in following ways
```python
>>> f(10)               # 1 positional, 3 default arguments
10 1 2 3
>>> f(a=10)             # 1 Keyword, 3 default arguments
10 1 2 3
>>> f(a=10, c=10)       # 2Keyword, 2 default 
10 1 10 3
>>> f(c=10, a=10)       # 2 Keyword, 2 default
10 1 10 3
>>> f(10, 10, 10)       # 3 positional, 1 default
10 10 10 3
>>> f(10, b=10)         # 1 positional, 1 Keyword, 2 default
10 10 2 3
>>> 
```
* But the following calls would be invalid
```python
>>> f()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: f() missing 1 required positional argument: 'a'
>>> f(a=10, 10)
  File "<stdin>", line 1
SyntaxError: positional argument follows keyword argument
>>> f(10, a=10)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: f() got multiple values for argument 'a'
>>> f(x=10)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: f() got an unexpected keyword argument 'x'
>>> 
```


## 3. Arbitrary Arguments lists
* A function can be called with an arbitrary number of arguments. These arguments will be wrapped up in a tuple.
* Before the variable number of arguments, zero or more arguments may occur.
* Normally these variable arguments will be last in the list of formal parameters because the scoop up all remaining input arguments that are passes to the function.
* Any formal parameters occur after the *arg parameter are key-words only
```python
>>> def concat(*args, sep="/"):
...     return sep.join(args)
... 
>>> concat('a', 'b', 'c', 'd')
'a/b/c/d'
>>> concat('a', 'b', 'c', 'd', sep="-")
'a-b-c-d'
>>> 
```

### *arguments, **keywords
* when a final formal parameters of the form **name is present it receives a dictionary containing all keyword arguments except for those corresponding to formal parameters.
```python
>>> d = {'a':1, 'b':2}
>>> func(**d)
1 {'b': 2}
>>> func(100,**d)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: func() got multiple values for argument 'a'
>>> func(a=100, **d)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: func() got multiple values for keyword argument 'a'
>>> 
```
* Keywords may be combined with a formal parameter of the form \*name which receives a tuple containing list (\*name must occur before \*\*name).
```python
>>> def func(a, *arg, **kw):
...     print(a, end=", ")
...     for ar in arg:
...             print(ar, end=", ")
...     for k in kw:
...             print(k," = ", kw[k], end=", ")
... 
>>> func(1,2,3,4,b=5,c=6,d=7)
1, 2, 3, 4, b  =  5, c  =  6, d  =  7, >>>
>>> 
```

## 4. Unpacking argument list
* \* operator to unpack arguments out of list or tuple
```python
>>> list(range(3,6))
[3, 4, 5]
>>> args = [3,6]
>>> list(range(*args)) # call with arguments unpacked from a list
[3, 4, 5]
>>> 
```
Unpacking arguments cannot be used like below
```python 
>>> a, b = *args
  File "<stdin>", line 1
SyntaxError: can't use starred expression here
>>> 
```
* `**` operator to unpack dictionaries and deliver keyword arguments.

# lambda expressions
* small anonymous functions can be created with the `lambda` keyword. Syntax is
```python
lambda parameters: expression
```
* parameters are optional, if they are supplied then they are just comma separated variable names that is positional arguments.
* The expression cannot contain branches or loops(although conditional expressions are allowed) and cannot have a return or yield statement.
* When a `lambda` function called it returns the value of computed expression
* If the expression is a tuple it should be enclosed in parenthesis
* ```lambda a, b: a+b``` will give sum of a, b.
```python
>>> def func(n):
...     return lambda x: x+n
... 
>>> func
<function func at 0x7f9094792670>
>>> func()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: func() missing 1 required positional argument: 'n'
>>> func(10)
<function func.<locals>.<lambda> at 0x7f90947925e0>
>>> l = func(10)
>>> l()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: <lambda>() missing 1 required positional argument: 'x'
>>> l(11)
21
>>> func(10)(11)
21
>>> 
```
* The above example uses a lambda expression to return a function. Another use is to pass a small function as an argument
```python
>>> pairs = [(1,'one'), (2, 'two'), (3, 'three'), (4, 'four')]
>>> pairs.sort(key=lambda k:k[1])
>>> pairs
[(4, 'four'), (1, 'one'), (3, 'three'), (2, 'two')]
>>> 
```

**Documentation string**
* The python parser does not strip indentation from multi-line string literals
```python
>>> def func():
...     """First line is not having any indentation
...     But second line has
...             and Third line has double indentation"""
... 
>>> print(func.__doc__)
First line is not having any indentation
	But second line has
		and Third line has double indentation
>>> 
```
# Nested functions (Inner functions)
* A function inside another function
* A simple example for a nested function will be
```python
>>> def func(n, x):
...     def add(x):
...             return n+x
...     f = add(x)
...     return  f
... 
>>> func(10, 20)
30
>>> 
```
* The above code wil add x to n. But these are static
* We can return the inner function from the outer function
```python
>>> def func(n):
...     def add(x):
...             return x+n
...     return add
... 
>>> func
<function func at 0x7f9094792670>
>>> func(10)
<function func.<locals>.add at 0x7f90947925e0>
>>> func(10)(20)
30
>>> f = func(10)
>>> f
<function func.<locals>.add at 0x7f90947925e0>
>>> f(20)
30
>>> f(10)
20
```
```python
>>> f()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: add() missing 1 required positional argument: 'x'
>>> 
```
* Normally inner functions are used to make closures.
* Inner functions can access variables from the enclosing scope(here, n)
* Inner functions can sometimes make it harder to unit test.



