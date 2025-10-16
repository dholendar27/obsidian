
## 1. What are Dunder Methods?

- **Dunder** stands for **“Double UNDerscore”**.
- These are special methods in Python that start and end with **two underscores**, like:
    - `__init__`
    - `__str__`
    - `__len__`
    - `__add__`

Example of a dunder method name:

```python
__methodname__
```

Python **automatically calls** these methods in certain situations. You don’t call them directly most of the time.

---

## 2. Why We Use Dunder Methods

Dunder methods let you **customize how objects of your class behave**.  
They allow your class to work **like built-in Python objects**.

For example:

- `__init__` lets you **initialize** an object when it’s created
- `__str__` controls how the object looks when printed.
- `__add__` lets you define what happens when you use `+` between two objects.
- `__len__` lets your object work with the built-in `len()` function.

They make your classes more **intuitive** and **Pythonic**.

---

## 3. Common Dunder Methods and Their Use

| Dunder Method      | When it’s called                          | What it does                                       |
| ------------------ | ----------------------------------------- | -------------------------------------------------- |
| `__init__`         | When an object is created                 | Initializes the object                             |
| `__str__`          | When `print()` or `str()` is used         | Returns a readable string representation           |
| `__repr__`         | When `repr()` or in interactive shell     | Returns a developer-friendly string representation |
| `__len__`          | When `len(obj)` is called                 | Defines length of the object                       |
| `__add__`          | When `obj1 + obj2` is used                | Defines behavior of the `+` operator               |
| `__eq__`           | When `obj1 == obj2` is checked            | Defines equality comparison                        |
| `__lt__`, `__gt__` | When `<` or `>` is used                   | Defines comparison behavior                        |
| `__getitem__`      | When using indexing like `obj[index]`     | Makes the object behave like a sequence or list    |
| `__call__`         | When the object is called like a function | Makes the object callable                          |

---

## 4. Example

```python
class Book:
    def __init__(self, title, pages):
        self.title = title
        self.pages = pages

    def __str__(self):
        return f"Book: {self.title}"

    def __len__(self):
        return self.pages

    def __add__(self, other):
        return self.pages + other.pages

book1 = Book("Python Basics", 300)
book2 = Book("OOP in Python", 200)

print(book1)            # __str__ → Book: Python Basics
print(len(book1))       # __len__ → 300
print(book1 + book2)    # __add__ → 500
```

Here:

- `__str__` makes `print(book1)` show a nice string instead of something like `<Book object at 0x...>`.
- `__len__` makes `len(book1)` work like a list or string.
- `__add__` lets you use `+` between two book objects.
---

## 5. Why Dunder Methods Are Useful

- They make your classes **act like built-in types** (strings, lists, numbers).
- They make your code **cleaner and more readable**.
- They allow **operator overloading** (customizing what `+`, `-`, `==` mean for your objects).
- They are heavily used in **frameworks** and **libraries** to give objects special behavior.
---

## 6. A Few More Useful Dunder Methods

- `__iter__` and `__next__` → for iteration (`for` loops)
- `__enter__` and `__exit__` → for context managers (`with` statement)
- `__bool__` → for `if` conditions
- `__contains__` → for `in` operator