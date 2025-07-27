- [Object Oriented Programming](#object-oriented-programming)
- [Classes](#classes)
  - [Names and Objects](#names-and-objects)
  - [Python scopes and namespaces](#python-scopes-and-namespaces)
  - [Class definition](#class-definition)
  - [Class Objects](#class-objects)
    - [1. Attribute reference](#1-attribute-reference)
    - [2. Instantiation](#2-instantiation)
  - [Instance Objects](#instance-objects)
    - [1. Data Attributes](#1-data-attributes)
    - [2. Methods](#2-methods)
  - [Method Objects](#method-objects)
  - [Class and Instance Variables](#class-and-instance-variables)
  - [Random Remarks](#random-remarks)
  - [Odds and Ends](#odds-and-ends)
- [Inheritance](#inheritance)
  - [Multiple Inheritance](#multiple-inheritance)
  - [Object - The Base class](#object---the-base-class)
- [Polymorphism](#polymorphism)
  - [1. Method overriding](#1-method-overriding)
  - [2. Method Overloading](#2-method-overloading)
- [Encapsulation](#encapsulation)
  - [Private Name Mangling](#private-name-mangling)
- [Private Variables](#private-variables)
  - [Q. Create a private method using a decorator](#q-create-a-private-method-using-a-decorator)

# Object Oriented Programming

- Python is a multi-paradigm language, it allows us to program in Procedural, object-oriented and functional style or in any mixture of style
- Object oriented programming is achieved by using classes and Objects

# Classes

- Class provides a means of bundling and functionality together
- Creating a new class creates a new `type` of object of allowing new instances of that type to be made
- Each class instance can have attributes attached to it for maintaining it's state. Class instances can also have methods(defined by it's class) for modifying it's state.
- Python classes provide all the standard features of object oriented programming
- Class Inheritance mechanism allows multiple base classes. A derived class can override any method of it's base class (or classes) and a method can call the method of a base class with the same name
- Classes are created at runtime and can be modified further after creation
- Normally class members are public (including data members except some private variables) and all member functions are virtual.
- There are no short hand fr referencing the object's members from its methods: The method function is declared with an explicit first argument representing the object, which is provided implicitly by the call.
- Classes themselves are objects, this provides semantics for importing and renaming.
- Built-in types can be used as base classes for extensions by the users.

### Names and Objects

- Objects have individuality and multiple names (in multiple scopes) can be bound to the same object. This is known as aliasing in other languages.
- Aliasing has a possible effect involving mutable objects such as list, dictionary and most other types
- Aliases behave like pointers in some aspects (passing an object is cheap since only a pointer is passed ny the implementation).

### Python scopes and namespaces

- A namespace is a mapping from names to objects.
  - Ex: Set of built-in names, global names in a module, local names in a function.
- There is absolutely no relation between names in different namespaces.
- Different modules may define a same function 'fucn' without confusion. Users of the module must prefix it with the module name.

  **Attribute**:

  - Any name following a dot for an object.
    - Ex: Z.real -> real is an attribute of Z
  - References to names in modules are attribute references
    - `mod_name.func_name`
    - mod_name is an module object, func_name is attribute of it.
  - In this case, module's attributes and the global names defined in the module share same namespace
  - Attributes may be read-only or writable. If they are writable then
    - `mod_name.the_answer = 42`
  - writable attributes may also be deleted
    - `del mod_name.the_answer`

  **Scope**:

  - At any point in time during execution, there are at least three nested scopes whose namespaces are directly accessible
    1. The inner most scope, which is searched first, contains the local names
    2. The scope of any enclosing functions, which are searched starting with the nearest enclosing scope, contains nonlocal but also non-global names.
    3. The next-to-last scope contains current module's global names.
    4. The outer most scope which is searched last is the namespace containing built-in names.
  - The global statement can be used to indicate that particular variables live in that global scope and should be rebound there.
  - The nonlocal statement indicates that particular variable lives in an enclosing scope and should be rebound there.

  ```python
  >>> def scope_test():
    def do_local():
        spam = "local"
    def do_nonlocal():
        nonlocal spam
        spam = "nonLocal"
    def do_global():
        global spam
        spam = "global"

    spam = "test"
    do_local()
    print(spam) # -> test
    do_nonlocal()
    print(spam) # -> nonLocal
    do_global()
    print(spam) # -> nonLocal

  >>>
  >>> scope_test()
  >>> print(spam)
  global
  ```

  - nonlocal assignment changed scope-test's binding of spam and global assignment changed module level binding.

### Class definition

```python
class ClassName:
    <statement 1>
    |
    |
    <statement n>
```

- class definitions, like function definitions must be executed before they have any effect (You could conceivably place a class definition in a branch of if statement or inside a function).
- When a class definition is entered a new namespace is created and used as local scope - thus all assignments to local variables go into this new namespace.In particular function definition binds the name of new function here.
- When a class definition is left normally (via the end) a class object is created. This is basically wrapper around the contents of the namespace created by the class definition.

## Class Objects

Class objects support two kind of operations

1. Attribute reference
2. Instantiation

### 1. Attribute reference

- Attribute references use the standard syntax used for all attribute references in python.
  - Ex: obj.name
- Valid attribute names are all the names that were in the class's namespace when the class object was created

```python
class MyClass:
    """ A simple example class"""
    i = 1234
    def func(self):
        return "Hello World"
```

- then MyClass.i and MyClass.func are valid attribute references, returning an integer and a function object respectively.
- `MyClass.__doc__` is also valid attribute reference
- Class attributes can also be assigned, `MyClass.i = 5456` # is valid

### 2. Instantiation

- Instantiation uses function notation, Just pretend class object is a parameter-less function that returns new instances of the class.
- `X = MyClass()` creates a new instance of the class and assigns this object to the local variable `X`.
- The instantiation operation (calling a class object) creates an empty object. Many classes like to create objects customized to a specific initial state
- A class may define a special method named `__init__()`

```python
def __init__(self):
    self.data = []
```

- When a class defines an `__init__()` method, class instantiation automatically invokes `__init__()` for the newly create class instance `X=MyClass()`
- `__init__()` method may have arguments for greater flexibility. In that case, argument given to the class instantiation operator are passed on to `__init__()`.

```python
>>> class Complex:
        def __init__(self, real_part, img_part):
            self.r = real_part
            self.i = img_part
>>> x = Complex(3.0, -4.5)
>>> x.r, x.i
(3.0, -4.5)
>>> Complex
<class '__main__.Complex'>
>>>x
<__main__.Complex object at 0x0A>
>>> Complex(3.0, -4.5)
<__main__.Complex object at 0x0B>
>>>Complex(3.0)
TypeError: __init__() missing 1 required positional argument
```

- **Python does not support method overloading**

```python
>>> class abc:
        def __init__(self, a):
            self.a = a
        def __init(self, a, b):
            self.a = a
            self.b = b

>>> x = abc(10)
TypeError: __init__() missing 1 required positional argument 'b'
```

## Instance Objects

The only operation understood by instance objects are attribute references. There are 2 kinds of valid attribute names

1. Data Attributes
2. Methods

### 1. Data Attributes

Data attributes need not to be declared, like local variables they spring int existence when they are first assigned to

```python
>>> class MyClass:
        pass
>>> X = MyClass()
>>> X.c = 1
>>> while X.c < 5:
        X.c = X.c + X.c
>>> X.c
8
>>> type(X.c)  # Since assignment is int
<class 'int'>
>>> MyClass.c
AttributeError: type object 'MyClass' has no attribute 'c'
>>> del X.c
>>> X.c
AttributeError: 'MyClass' object has no attribute 'c'
```

### 2. Methods

- A method is a function that belong to an object (In Python, the term method is not unique for class instances, other objects types can have methods as well)
- Valid methods names of an instance object depend on its class.
- By definition, all attributes f a class that are function objects, define corresponding methods of its instances.

```python
>>> class MyClass:
      i = 1234
      def func(self):
          print("Hello World")

>>> X = MyClass()
```

1. X.func is a valid method reference, since MyClass.func is a function
2. X.i is not since MyClass.i is not a function
3. X.func is not same as MyClass.func

```python
>>> MyClass.func
<function MyClass.func at 0x0A>
>>> X.func
<bound method MyClass.func of <__main__.MyClass object at 0x0B>>
>>> X.func()
Hello World
>>> MyClass.func()
TypeError: func() missing 1 required positional argument: 'self'
>>> MyClass.func(MyClass)
Hello
```

## Method Objects

- Usually, a method is called right after it is bound `X.func()`
- It is not necessary to call a method right away.
- `X.func` is a method object, and can be stored away and called at later time.

```python
>>> xf = X.func()
>>> print(xf())
Hello
>>> xf()
Hello
```

- special thing about methods is that the instance object is passed as the first of the function.

## Class and Instance Variables

- Instance variables are unique to each instance
- Class variables are attributes and methods shared by all instances of the class

```python
>>> class Dog:
        kind = 'canine' # class variable shared by all instances
        def __init__(self, name):
            self.name = name

>>> d = Dog('Fido')
>>> e = Dog('Buddy')
>>> d.kind
'canine'
>>> e.kind
'canine'
>>> d.name
'Fido'
>>> e.name
'Buddy'
```

- Shared data can have possible surprising effects with involving mutual objects

```python
>>> class Dog:
...     num = 1
...     tricks = []
...     def __init__(self, name):
...         self.name = name
...     def add_trick(self, trick):
...         self.tricks.append(trick)
...
>>> b = Dog('buddy')
>>> e = Dog('Fido')
>>> e.num
1
>>> b.num
1
>>> b.add_trick('dumb')
>>> e.add_trick('numb')
>>> b.num = 5
>>> e.num = 10
>>> b.tricks
['dumb', 'numb']
>>> e.tricks
['dumb', 'numb']
>>> b.num
5
>>> e.num
10
>>> f = Dog('kito')
>>> f.tricks
['dumb', 'numb']
```

- To correct the design

```python
>>> class Dog:
...     num = 1
...     def __init__(self, name):
...         self.name = name
...         self.tricks = []
...     def add_trick(self, trick):
...         self.tricks.append(trick)
...
>>> b = Dog('buddy')
>>> e = Dog('Fido')
>>> e.num
1
>>> b.num
1
>>> b.add_trick('dumb')
>>> e.add_trick('numb')
>>> b.num = 5
>>> e.num = 10
>>> b.tricks
['dumb']
>>> e.tricks
['numb']
>>> b.num
5
>>> e.num
10
>>> f = Dog('kito')
>>> f.tricks
[]
```

## Random Remarks

- Data attributes override method attributes and vice versa

**Data Attributes override method attributes**

```python
>>> class SomeClass:
...     def attr(self):
...         return "function attribute"
...     attr = 'data attribute'
...
>>> x = SomeClass()
>>> x.attr
'data attribute'
>>> x.attr()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object is not callable
```

**Method attributes override data attributes**

```python
>>> class SomeClass:
...     attr = 'data attribute'
...     def attr(self):
...         return "function attribute"
...
>>> x = SomeClass()
>>> x.attr
<bound method SomeClass.attr of <__main__.SomeClass object at 0x000002128CCBB9D0>>
>>> x.attr()
'function attribute'
```

```python
>>> class SomeClass:
...     def __init__(self):
...         self.attr = 'data attribute'
...     def attr(self):
...         return "function attribute"
...
>>> x = SomeClass()
>>> x.attr
'data attribute'
>>> x.attr()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object is not callable
```

- Since data attributes and method attributes within a class share same namespace first two scenarios are overriding predecessor with later.
- There is another way of overriding/shadowing that may happen.
  - Method definitions end up in the class dict `obj.__class__.__dict__`
  - Where as instance variables end up in the instance_dict `obj.__dict__`
- Attribute lookup happens in the instance dict first, so it may override a method with the same name, even though that the method is still in the class-dict.
- Data attributes (instance variables) are first assigned in `__init__()`, which is why they get assigned later than method definitions, which are assigned when the class declarations is passed.

```python
>>> x.__class__.__dict__
mappingproxy({'__module__': '__main__', '__init__': <function SomeClass.__init__ at 0x000002128C760540>, 'attr': <function SomeClass.attr at 0x000002128CCD4040>, '__dict__': <attribute '__dict__' of 'SomeClass' objects>, '__weakref__': <attribute '__weakref__' of 'SomeClass' objects>, '__doc__': None})
>>> x.__dict__
{'attr': 'data attribute'}
```

```python
>>> del x.attr
>>> x.attr
<bound method SomeClass.attr of <__main__.SomeClass object at 0x000002128CCCC810>>
```

- To avoid conflicts use some naming conventions like capitalizing method names or prefix data attributes with a small unique string (perhaps just an underscore).
- Classes are not usable to implement pure abstract data types
- The first argument of a method is called `self`. This is nothing more than a convention. The name `self` has absolutely no special meaning to python. Just purely for readability.
- Any function object that is a class attribute defines a method for instance of that class.It is not necessary that the function definition is textually enclosed in the class definition: assigning a function object to a local variable in the class is ok. For example

```python
>>> def func1(self, x, y):
...     return x + y
...
>>> class MyClass:
...     func = func1
...     def junk(self):
...         return "Junk"
...
>>> X = MyClass()
>>> X.func(20, 20)
40
```

```python
>>> def func1(x, y):
...     return x + y
...
>>> class MyClass:
...     func = func1
...
>>> X = MyClass()
>>> func1(10, 20)
30
>>> X.func(10, 20)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: func1() takes 2 positional arguments but 3 were given
>>> X.func(10)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in func1
TypeError: unsupported operand type(s) for +: 'MyClass' and 'int'
```

- Methods may call other methods by using method attributes of the `self` argument

```python
class Bag:
    def __init__(self):
        self.data = []
    def add(self, x):
        self.data.append(x)
    def add_twice(self, x):
        self.add(x)
        self.add(x)
```

- Methods may reference global names in the same way as ordinary functions.

```python
>>> a = 123
>>> global_name = 123
>>> class MyClass:
...     def method(self):
...         print(global_name)
...
>>> X = MyClass()
>>> X.method()
123
```

- The global scope associated with a method is the module containing its definition.
  - In the above example `method()`'s scope is the terminal
- There are many legitimate uses of the global scope, for one thing functions and modules imported into the global scope can be used by methods as well as functions and classes defined in it.
- Each value is an object, and therefore has a class (also called it type). It is stored as `object.__class__`.

## Odds and Ends

Sometimes it is useful to have a data type similar to C `struct`, bundling together a few named data items. An empty class definition will do nicely.

```python
>>> class Employee:
        pass
>>> john = Employee()
>>> john.name = "John"
>>> john.dept = "Computers"
>>> john.salary = 1000
```

# Inheritance

The syntax for the derived class definition looks like this

```python
class DerivedClassName(BaseClassName):
    <statement 1>
    |
    |
    <statement n>
```

- Inheritance enables new objects to take on the properties of existing objects.
- In place of a BaseClassName, other arbitrary expression are allowed. This can be useful, for example when the BaseClass is defined in another module

```python
class DerivedClassname(mod_name.BaseClassName)
```

- Execution of a derived class definition proceeds the same as for a base class.
- When the class object is constructed, the base class is remembered. This is used for resolving the attribute references. If a requested attribute is not found in the class the search proceeds to look in the base class. This rule is applied recursively if the base class itself is derived from another class

```python
>>> class Base:
...     val = 10
...
>>> class Sub(Base):
...     pass
...
>>> class SubSub(Sub):
...     def __init__(self):
...         print(self.val)
...
>>> X = SubSub()
10
>>> X.val
10
```

- **Instantiation**: `DerivedClassName()` creates a new instance of the class.
- Method references are resolved as follows: The corresponding class attribute is searched, (descending down the chain of base methods if necessary) and the method reference is valid if this yields a function object.
- Derived class may override methods of their base class
- Python has 2 built-in functions that work with inheritance
  1. isinstance() - to check an instance type
     - isinstance(obj, int) - will be True only if `obj.__class__` is int or some class derived from int.
  2. issubclass() - to check class inheritance
     - `issubclass(bool, int) -> True`
     - `issubclass(float, int) -> False`

## Multiple Inheritance

A class definition with multiple base classes looks like

```python
class DerivedClassName(Base1, Base2, Base3):
    <Statement 1>
    |
    |
    <Statement N>
```

- In the simplest cases, the search for attributes inherited from parent class as depth first or left to right not searching twice in the same class (where there is an overlap in hierarchy).
- So, If an attribute is not found in DerivedClassName it is searched first in Base1, then recursively in the Parent classes of Base1, and if not found there it was searched in Base2 and so on.
- The method resolution order changes dynamically to support cooperative calls to `super()`.
- In Python, we use `super()` function to call the parent class methods.

```python
>>> class Shape:
...     def __init__(self, color='black'):
...         self.color = color
...
>>>
>>> class Circle(Shape):
...     def __init__(self, r):
...         self.r = r
...
>>>
>>> c = Circle(10)
>>> c.r
10
>>> c.color
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Circle' object has no attribute 'color'
```

- Since initialization is not happened for super class, we got attribute error for color attribute of Shape class.

```python
>>> class Shape:
...     def __init__(self, color='black'):
...         self.color = color
...
>>>
>>> class Circle(Shape):
...     def __init__(self, r):
...         super().__init__()
...         self.r = r
...
>>>
>>> c = Circle(10)
>>> c.r
10
>>> c.color
'black'
```

## Object - The Base class

- In python, all classes inherit from the object class implicitly. So, `class Myclass:` and ` class MyClass(object):` are same.
- `object` class provides some special methods which are inherited by all the classes few of them are
  - `__init__()`
  - `__new__()`
    - create the object. After creating the object it calls `__init__()` method to initialize the attributes of the object. Finally, it returns the newly created object to the calling program.
  - `__str__()`
    - used to return a nicely formatted string representing of the object
- The object class version of `__str__()` method returns a string containing the name of the class and its memory address in the hexadecimal format

```python
>>> class MyClass:
...     def f1(self):
...         print("A, f1")
...
>>> a = MyClass()
>>> a
<__main__.MyClass object at 0x000002128CCCF150>
>>> print(a)
<__main__.MyClass object at 0x000002128CCCF150>
```

```python
>>> class MyClass:
...     def __str__(self):
...         return "My description"
...
>>>
>>> a = MyClass()
>>> a
<__main__.MyClass object at 0x000002128CCCF290>
>>> print(a)
My description
```

# Polymorphism

Polymorphism means the ability to take various forms. Basically there are 2 types of polymorphism are there

1. Method overriding
2. Method Overloading

## 1. Method overriding

- A child class inherits all the methods from the parent class. However, we will encounter situations where the method inherited from the parent class doesn't quite fit into the child class. In such cases we will have to re-implement method in the child class. This process is know as Method Overriding

```python
>>> class A:
...     def explore(self):
...         print("Class A")
...
>>>
>>> class B(A):
...     def explore(self):
...         print("Class B")
...
>>>
>>> b = B()
>>> a = A()
>>> a.explore()
Class A
>>> b.explore()
Class B
```

```python
>>> class A:
...     def f1(self):
...         print("A, f1")
...     def f2(self):
...         print("A, f2")
...
>>> class B(A):
...     def f1(self):
...         print("B, f1")
...
>>>
>>> b = B()
>>> b.f1()
B, f1
>>> b.f2()
A, f2
```

- For some reason, if we still need to access the overriden methods of the parent class in the child class

```python
>>> class A:
...     def f1(self):
...         print("A, f1")
...     def f2(self):
...         print("A, f2")
...
>>> class B(A):
...     def f1(self):
...         super().f1()
...         print("B, f1")
...
>>>
>>> b = B()
>>> b.f1()
A, f1
B, f1
>>> b.f2()
A, f2
```

- Some notes

```python
>>> class A:
...     def f1(self):
...         print("A, f1")
...     def f2(self):
...         print("A, f2")
...
>>> class C(A):
...     super().f1()
...
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in C
RuntimeError: super(): no arguments
>>>
>>> class C(A):
...     super(self).f1()
...
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in C
NameError: name 'self' is not defined
>>>
>>> class C(A):
...     super(A).f1()
...
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in C
AttributeError: 'super' object has no attribute 'f1'
```

## 2. Method Overloading

- Method overloading is having same method with different signatures, But in Python, the later methods will override the earlier method. So, it is not possible to create a method with different signatures in regular way.
- But in python it is possible to create a method which can accept different signature.

```python
>>> class MyClass:
...     def __init__(self, x, y):
...         self.x = x
...         self.y = y
...     def get_value(self, x=None, y=None):
...         if x is not None and y is not None:
...             print(x, y)
...         elif y is not None:
...             print(x)
...         elif x is not None:
...             print(y)
...         else:
...             print("None")
...
>>>
>>> X = MyClass(10, 20)
>>> X.get_value()
None
>>> X.get_value(10)
None
>>> X.get_value(10, 20)
10 20
```

# Encapsulation

Encapsulation is an OOP technique of wrapping the data and code.

- Encapsulation is the mechanism for restricting the access to some of the objects components, this means the internal representation of an object can't be seen from outside f the object's definition.
- Access to this data is typically achieved using getters and setters
- By using solely get and set methods, we can make sure that the internal data cannot be accidentally set into an inconsistent or invalid state.
- Encapsulation is achieved through private variables.

```python
>>> class Encap:
...     def __init__(self, x):
...         self.__x = x
...
>>> X = Encap(10)
>>> X.x
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Encap' object has no attribute 'x'
>>> X.__x
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Encap' object has no attribute '__x'
```

```python
>>> class Encap:
...     def __init__(self, x):
...         self.__x = x
...     def get_x(self):
...         return self.__x
...
>>> X = Encap(10)
>>> X.get_x()
10
```

- Note: If we `get_x(self): return self.x` will give `AttributeError`.
- If an identifier doesn't start with an underscore character `-`, it can be accessed from outside. i.e., the value can be read and modified.
- Data can be protected by making members private or protected.
- Instance variables starting with 2 underscore `__` cannot be accessed from outside of the class. At least not directly, but they can be accessed through private name mangling.

### Private Name Mangling

Private data `__var` can be accessed by using name construct, `object_name.__className__var`

```python
>>> class Encap:
...     def __init__(self, x):
...         self.__x = x
...     def get_x(self):
...         return self.__x
...
>>> X = Encap(10)
>>> X._Encap__x
10
>>> X._Encap__x = 20
>>> X.get_x()
20
```

| Declaration | Notation  | Behavior                                                                    |
| ----------- | --------- | --------------------------------------------------------------------------- |
| var         | public    | Can be accessed from inside and outside                                     |
| \_var       | protected | Like a public member, but they shouldn't be directly accessed from outsider |
| \_\_var     | private   | can't be seen and accessed from outside                                     |

# Private Variables

- Private instance variables that cannot be accessed from inside an object don't exist in python.
- When you create an instance of some class there is nothing to prevent you from poking around and various internal, private methods that are necessary for that class to function, But nt intended for direct use/access.
- Nothing is really private in python, no class instance can keep you away from all what's inside(this makes introspection possible and powerful)
- However there is a convention that is followed by most python code: a name prefixed with an underscore (eg: \_var) should be treated as non-public part of the API(Whether it is a function, a method or a data member). It should be considered as implementation details and subject to change without notice.
- Since there is valid use case for class private members (to avoid name classes of names with names defined by subclasses), there is limited support for such a mechanism called name mangling.

**Name Mangling**:

Any identifier of the form **spam or **var (at least 2 leading underscore, one tailing underscore) is textually replaced with `_className__spam` where class name is the current class name with leading underscores stripped.

The mangling is dne without regard to the synthetic position of the identifier as long as it occurs within the definition of a class.

```python
>>> class Test:
...     def __private(self):
...         pass
...     def public(self):
...         pass
...
>>> dir(Test)
['_Test__private', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getstate__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'public']
```

- `__private` should be considered as private but still be accessible through `_Test__private`
- Name mangling is helpful for letting subclasses override methods without intra-class method calls.

```python
>>> class Mapping:
...     def __init__(self, iterable):
...         self.items_list = []
...         self.__update(iterable)
...     def update(self, iterable):
...         for item in iterable:
...             self.items_list.append(item)
...     __update = update # Private copy of original update() method
...
>>>
>>> class MappingSubClass(Mapping):
...     def update(self, keys, values):
...         # Provides new signature for update()
...         # But does not break __init__()
...         for item in zip(keys, values):
...             self.items_list.append(item)
...
...
>>> y = MappingSubClass([11,12,13])
>>> y.update(['a', 'b', 'c'], [1,2,3])
>>> y.items_list
[11, 12, 13, ('a', 1), ('b', 2), ('c', 3)]
>>> x = Mapping([11,12,13])
>>> x._Mapping__update([20, 24])
>>> y.items_list
[11, 12, 13, ('a', 1), ('b', 2), ('c', 3)]
>>> y.__class__.__dict__
mappingproxy({'__module__': '__main__', 'update': <function MappingSubClass.update at 0x000002128CCD5E40>, '__doc__': None})
>>> x.__class__.__dict__
mappingproxy({'__module__': '__main__', '__init__': <function Mapping.__init__ at 0x000002128CCD5D00>, 'update': <function Mapping.update at 0x000002128CCD5DA0>, '_Mapping__update': <function Mapping.update at 0x000002128CCD5DA0>, '__dict__': <attribute '__dict__' of 'Mapping' objects>, '__weakref__': <attribute '__weakref__' of 'Mapping' objects>, '__doc__': None})
>
>>> dir(y)
['_Mapping__update', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getstate__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'items_list', 'update']
>>> dir(x)
['_Mapping__update', '__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getstate__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'items_list', 'update']
>>>
>>> dir(y) == dir(x) # Just a string comparison
True
```

- Even if dir(y) = dir(x), if we check the address of x and y they are different.
- The above example would work even if MappingSubClass were to introduce **update method since it is replace with \_Mapping**update in the Mapping class and \_MappingSubclass\_\_update in the MappingSubClass

### Q. Create a private method using a decorator

```python
>>> import sys, functools
>>> def private(member):
...     @functools.wraps(member)
...     def wrapper(*function_args):
...       myself = member.__name__
...       caller = sys._getframe(1).f_code.co_name
...       if (not caller in dir(function_args[0]) and not caller is myself):
...          raise Exception("%s called by %s is private"%(myself,caller))
...       return member(*function_args)
...     return wrapper
...
>>>
>>> class Test:
...     def public_method(self):
...         print("Public")
...     @private
...     def private_method(self):
...         print("Private")
...
>>> t = Test()
>>> t.public_method()
Public
>>> t.private_method()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 7, in wrapper
Exception: private_method called by <module> is private
```
