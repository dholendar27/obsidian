## What is `@property`?

- `@property` is a **decorator** that lets you define a method as a **getter** for a property.
- It allows you to access methods like **attributes** (without parentheses).
- You can also define a setter method to **set** the property value using `@property_name.setter`

---

## Why Use `@property`?

- To **control access** to instance variables without changing how you access them.
- To **hide internal implementation** but keep a clean interface.
- It avoids the need to call explicit getter and setter methods (`get_x()`, `set_x()`), making the code more readable.

---

## Example Without `@property`

```python
class Person:
    def __init__(self, name):
        self._name = name  # convention: _name means "private"

    def get_name(self):
        return self._name

    def set_name(self, new_name):
        if isinstance(new_name, str):
            self._name = new_name
        else:
            print("Name must be a string.")

person = Person("Alice")
print(person.get_name())  # Alice
person.set_name("Bob")
print(person.get_name())  # Bob
```

---

## Same Example Using `@property`

```python
class Person:
    def __init__(self, name):
        self._name = name

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, new_name):
        if isinstance(new_name, str):
            self._name = new_name
        else:
            print("Name must be a string.")

person = Person("Alice")
print(person.name)    # Access like attribute, prints: Alice

person.name = "Bob"   # Set like attribute
print(person.name)    # Bob

person.name = 123     # Invalid, prints: Name must be a string.
```

---

## How It Works:

- `@property` makes the method `name()` behave like a **getter**.
- `@name.setter` defines the setter for the `name` property.
- Now you access and set `name` like a **normal attribute**, but internally you control what happens.

---

## Benefits:

- Cleaner, more intuitive syntax.
- Can add validation logic inside setters.
- Keeps encapsulation intact without complicated getter/setter calls.

|Feature|Using `@property`|Using Normal Getter/Setter Methods|
|---|---|---|
|Accessing the value|`obj.attribute`|`obj.get_attribute()`|
|Setting the value|`obj.attribute = value`|`obj.set_attribute(value)`|
|Syntax style|Looks like **normal attribute access** (cleaner)|Explicit method calls (more verbose)|
