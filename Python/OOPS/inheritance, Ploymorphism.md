## Inheritance

inheritance is a process of inheriting the attributes and methods from the another class (parent class)

```python
class Animal:
    def speak(self):
        print("Animal speaks")

class Dog(Animal):
    def bark(self):
        print("Dog barks")

d = Dog()
d.speak()  # Inherited
d.bark()   # Defined in Dog
```

We can also inherit multiple classes

```python
class Walk:
    def move(self):
        print("Walking")

class Swim:
    def move(self):
        print("Swimming")

class Amphibian(Walk, Swim):
    pass

a = Amphibian()
a.move()  # Output: "Walking" (because Walk comes first in inheritance list)
```

By using the `super()` we can  access the parent class attributes

```python
# Parent class
class Animal:
    def __init__(self, name, age):
        self.name = name  # Public attribute
        self._age = age   # Protected attribute (conventionally used, not enforced)
        self.__species = "Unknown"  # Private attribute (name-mangled)

    def get_species(self):
        return self.__species  # Public method to access private data

# Child class inherits from Animal
class Dog(Animal):
    def __init__(self, name, age, breed):
        super().__init__(name, age)  # Call to the parent class constructor
        self.breed = breed
    
    def show_info(self):
        print(f"Name: {self.name}, Age: {self._age}, Breed: {self.breed}")
        # Accessing parent's public attribute
        print(f"Species: {self.get_species()}")  # Accessing private data through a method

# Create an instance of Dog
dog = Dog("Buddy", 3, "Golden Retriever")
dog.show_info()

```

Python uses the **Method Resolution Order (MRO)** to decide which method to call when there's a conflict.

## Polymorphism

**Polymorphism** means **"many forms"** — the ability for a single interface or method to work differently depending on the object calling it.

>polymorphism means having the same method in the different objects. but when they are called they call the same method in different objects and each object will respond in its own way.

**Example:** The objects are like donuts with similar shapes and methods are the fillings. each donut will have the filling. but the filling inside can be different every time.

There are two main types of polymorphism
### 1. Complie-time polymorphism

This is also known as method overloading or operator overloading. It occurs when you have multiple methods with the same name but different parameters (number, type, or both). The compiler decides which method to invoke based on the method signature at compile time.

> Python doesn't support method overloading directly. Instead, the last method definition overrides previous ones, but we can achieve polymorphism in other ways

```python
class Printer:
    def print(self, text: str):
        print(text)

    def print(self, text: str, count: int):
        for _ in range(count):
            print(text)

# Calling print with different parameters
printer = Printer()
printer.print("Hello")  # This would call the method without the count argument
printer.print("Hello", 3)  # This would call the method with the count argument
```

### 2. Runtime polymorphism

This occurs when the method to be invoked is determined at runtime. It is usually achieved through **method overriding** where a subclass provides a specific implementation of a method already defined in its parent class.
> Runtime polymorphism is all about deciding _which method_ to call for _which object_ while the program is running.

```python
class Animal:
    def speak(self):
        print("Animal makes a sound")

class Dog(Animal):
    def speak(self):
        print("Dog barks")

class Cat(Animal):
    def speak(self):
        print("Cat meows")

# Runtime polymorphism: This can be determined at runtime
def animal_speak(animal: Animal):
    animal.speak()

a = Animal()
d = Dog()
c = Cat()

animal_speak(a)  # Outputs: Animal makes a sound
animal_speak(d)  # Outputs: Dog barks
animal_speak(c)  # Outputs: Cat meows
```

---
When we are using the polymorphism. we use the inheritance, where the child class inherit the parent class. so the child class can override the method from the parent class. when we call the method using the object create using the child class. the method in the child class is called. if not called from the parent class. 

we can use the polymorphism without the inheritance that is called ==duck typing==. 
- Duck typing enables polymorphism without requiring objects to explicitly inherit from a common superclass or implement a specific interface.
- Instead, any object that implements the required methods or behaviors can be used interchangeably.