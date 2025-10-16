---
date created: 2025-10-16 10:17
---

Python is a object oriented programming language that allow developers to structure their code using classes and objects.

## Classes

Class is an blueprint for creating a objects. Class contains the strcture and code data and methods. The classes helps to organize code, promoting reusability and implementting real-world entites in programming.

```python
class Person:   # class
  def __init__(self,name):
    self.name = name # data
    
  def greet(self): # method
    print("Hello")
```

## Objects

The object is an instance of class. The objects act an final product that has been created using the class.  For example: We have an blueprint to build an car. we can use the blueprint to build different car for different drivers for their specifications. one driver need high performance and other one need durability.

```python
class Car:
    def __init__(self, brand, model, year):
        self.speed = brand
        self.model = model
        self.year = year

def display_info(self):
        print(f"Car: {self.brand} {self.model}, Year: {self.year}")


# Creating objects
car1 = Car("Toyota", "Corolla", 2020)
car2 = Car("Honda", "Civic", 2018)
```

## `__init__`

The `__init__` method is a special method in python classes. It is called a constuctor because it get called when the object is create. It intialized the attributes of the object, ensure the each object instance have the unique data

## `self`

The `self` parameter represents the instance of the class. It is used to access attributes and methods of the class within the class itself. Every method in a class must have `self` as its first parameter to access instance attributes.

```python
class Car:
  def __init__(self,name, speed, durability):
    self.name = name
    self.speed = speed
    self.durability = durability

  def build(self):
    print(f"Adding Name to car: {self.name}")
    print(f"Adjusting the speed to: {self.speed}")
    print(f"Enforcing the durability to: {self.durability}")

tesla = Car("Tesla",100,"high")
tesla.build()
```
