**Decorators** are powerful and flexible way to midfy or enchange the behaviour of the functions, methods and classes with changing the source code.

There are three improtant types of decorators. They are:

1. Function Decorators
2. Class Decorators
3. functools

### Function Decorators

The function decorators are useful for modifying the function without changing the acutal function.

```python
def decorator(func):
    def wrapper(*args, **kwargs):
        print("Before the function call")
        result = func(*args, **kwargs)
        print("After the function call")
        return result
    return wrapper

@decorator
def say_hello(name):
    print(f"Hello, {name}!")

say_hello("Alice")
```

### Class Decorators

Class decorators are a way to modify or enhnace classes in a resuable way. class decorators are function taht take the class as input and return a new class after modifying it.

```python
def add_method(cls):
  def new_method(self):
    return "This is a new method"
  cls.new_method = new_method
  return cls

@add_method
class MyClass:
    def __init__(self, name):
        self.name = name

# Usage
obj = MyClass("Test")
print(obj.new_method())  # Output: This is a new method!
```

### functools

The functools are the one of the most useful python standard libraries which contains a collection of higher order functions. 