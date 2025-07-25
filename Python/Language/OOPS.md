- [Object Oriented Programming](#object-oriented-programming)
- [Classes](#classes)
    - [Names and Objects](#names-and-objects)
    - [Python scopes and namespaces](#python-scopes-and-namespaces)
    - [Class definition](#class-definition)
  - [Class Objects](#class-objects)
    - [1. Attribute reference](#1-attribute-reference)
    - [2. Instantiation](#2-instantiation)
  - [Instance Objects](#instance-objects)

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
