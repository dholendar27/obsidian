
Both **class methods** and **static methods** are ways to define functions inside a class, but they serve different purposes and behave differently when called.

---

### 1. Class Methods

A **Class Method**:

- Belongs to the **class itself**, not to a specific object.
- Can **access or modify class variables**, but not instance variables.
- Is defined using the `@classmethod` decorator.
- The first parameter is always `cls` (refers to the class, not the object)

#### Example:

```python
class Student:
    school_name = "ABC High School"   # Class variable

    def __init__(self, name):
        self.name = name

    @classmethod
    def change_school(cls, new_name):
        cls.school_name = new_name    # changing class variable

# Using class method
print(Student.school_name)  # ABC High School
Student.change_school("XYZ International")
print(Student.school_name)  # XYZ International
```

Why use it:

- To work with data that is shared among all objects of the class.
- Useful for factory methods (methods that create objects in different ways).

---

### 2. Static Methods

A **Static Method**:

- Belongs to the class too, but it doesn’t need `self` or `cls`.
- It can’t access class variables or instance variables directly.
- Is defined using the `@staticmethod` decorator.
- It behaves just like a normal function but is placed inside a class for better organization.

#### Example:

```python
class MathUtils:
    @staticmethod
    def add(a, b):
        return a + b

# Using static method
result = MathUtils.add(5, 3)
print(result)  # 8
```

Why use it:

- When the method does not depend on the class or its objects.
- Useful for utility/helper functions that are logically related to the class.