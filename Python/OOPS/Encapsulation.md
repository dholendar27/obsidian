Encapsulation is way to restrict object's components from direct access. which means that internal representation of an object is hidden from the outside. This is done to protect the interanal state and behavior

We can implement the Encapsulation using

- Private and Protected members
- Getters and setters

## Access Modifiers in python

python doesn't have access modifiers like other languages, instead it uses the naming conventions to denote the level of access control.

1. Public Members

The public class can be accessed from anywhere

```python
class Car:
    def __init__(self):
        self.color = "Red"  # Public attribute

car = Car()
print(car.color)  # Accessible

```

2. Protected Members (`_`)
    - Meant to be used for the iternal use only
    - But we can still access from the outside of the class (Python doesn't enfore it)

```python
class Car:
    def __init__(self):
        self.color = "Red"  # Public attribute
    def _change_color(self):
      print(f"Chaning the color to: {self.color}" )

car = Car()
print(car.color)  # Accessible
car._change_color() # Can access, but shouldn't (by convention)

```

3. Private Members(`__`)
    - Name mangled to avoid accidental access.
    - Cannot be accessed directly using the attribute name

```python
class Car:
    def __init__(self):
        self.color = "Red"  # Public attribute
    def __change_color(self):
      print(f"Chaning the color to: {self.color}" )

car = Car()
print(car.color)  # Accessible
car.__change_color() # Attribute Error
car._Car__change_color()# Can access, but shouldn't (by mangling)
```

> **Why name mangling?**

> Python changes the name of the variable internally to `_ClassName__variable`, to make it harder to access accidentally.

## Getters and Setters

### Getter

Method that returns the value of a private

### Setter

Method that sets/updates the value of a private/protected attribute, often with **validation**.

```python
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance  # private

    # Getter method
    def get_balance(self):
        return self.__balance

    # Setter method
    def set_balance(self, amount):
        if amount < 0:
            print("Balance can't be negative!")
        else:
            self.__balance = amount

account = BankAccount(1000)

print(account.get_balance())  # 1000
account.set_balance(2000)
print(account.get_balance())  # 2000

account.set_balance(-500)     # Won't set
```

### Pythonic way: Using `@property` Decorator

```python
class Temperature:
    def __init__(self, celsius):
        self.__celsius = celsius

    @property
    def celsius(self):
        return self.__celsius

    @celsius.setter
    def celsius(self, value):
        if value < -273.15:
            raise ValueError("Temperature can't be below absolute zero!")
        self.__celsius = value

temp = Temperature(25)
print(temp.celsius)  # 25

temp.celsius = 100    # Calls setter
print(temp.celsius)   # 100

# temp.celsius = -300  # Raises ValueError

```