## EAFP vs. LBYL

### EAFP: Easier to Ask for Forgiveness than Permission

**Definition**: In Python, **EAFP** encourages writing code that assumes the operation will succeed, and if it doesn’t, handles the exception that arises. It focuses on trying to perform the operation first, and catching the exception later if something goes wrong.

Explanation: This encourages to write the code with try-except so that if the code will either run or fail, it runs the code first and catch the error later.

**Philosophy**: The code assumes the operation will be valid, and if it isn’t, it catches and handles the exception

```python
try:
    value = my_dict[key]  # Try to access the key
except KeyError:
    value = None  # Handle failure by assigning default value
```

- **Pros**:
    - Leads to cleaner code with fewer condition checks.
    - Handles errors only when necessary, which is more efficient in certain cases (e.g., if exceptions are rare).
- **Cons**:
    - Exception handling can be more costly than simply checking a condition upfront (depending on the case).
    - If overused, it can lead to inefficient or unreadable code.

### LYBL: Look Before You Leap

- **Definition**: **LBYL** encourages the programmer to check if an operation will succeed before performing it. It’s about checking the conditions upfront, making sure the operation is safe to execute.
- **Philosophy**: Rather than handling errors after they occur, LBYL focuses on preventing errors by checking conditions beforehand (e.g., checking if a key exists in a dictionary before accessing it).

```python
if key in my_dict:  # Check if key exists before accessing
    value = my_dict[key]
else:
    value = None
```

- **Pros**:
    - Can make code more predictable because you're actively checking conditions before taking action.
    - Reduces the cost of handling exceptions (since exceptions themselves are costly).
- **Cons**:
    - Leads to more code and more conditional checks, which can make code less clean and harder to maintain.
    - Can lead to incorrect assumptions (e.g., the condition check may not cover all edge cases).

#### **When to Use EAFP vs LBYL**:

- **EAFP** is generally preferred in Python because it aligns with Python's dynamic and flexible nature. It reduces boilerplate code and focuses on trying things and handling exceptions when they happen.
- **LBYL** is useful when you have a specific, predictable condition that can be checked upfront without adding unnecessary complexity. It’s best used when failure is common, and exceptions are expensive to handle.

## Unpacking

unpacking refer to process of extracting elements from an iterable into variables. python provide several syntaxes for unpacking

```python
a,b,c = (1,2,3) # Tuple unpacking
a, b, c = [4, 5, 6] # list unpacking
a, b = b, a  # Swapping values of a and b
```

### Using `*` for Extended Unpacking

`*` allows extended unpacking, where we can capture multiple elements using the `*` operator. This is useful when the number of elements is not known or when we want to grab subset of elements

```python
head, *tail = [1, 2, 3, 4, 5]
print(head)  # 1
print(tail)  # [2, 3, 4, 5]
```

You can also use `*` for unpacking dictionaries:

```python
dict1 = {'a': 1, 'b': 2, 'c': 3}
dict2 = {**dict1, 'd': 4}  # Merging dictionaries
print(dict2)  # {'a': 1, 'b': 2, 'c': 3, 'd': 4}
```

## `*args` and `**kwargs`

The `*args` and `**kwargs` are used for passing a variable number of arguments to a function. They provide the flexibility when defining functions taht can accept any number of positional and keyword argumnets

#### **`*args` (Positional Arguments)**

- **Definition**: `*args` allows you to pass a variable number of positional arguments to a function. It collects all the extra positional arguments into a tuple.

```python
def greet(*names):
    for name in names:
        print(f"Hello, {name}!")

greet("Alice", "Bob", "Charlie")  # Can pass any number of arguments
```

#### **`**kwargs` (Keyword Arguments)**

- **Definition**: `**kwargs` allows you to pass a variable number of keyword arguments (i.e., named arguments) to a function. It collects the arguments into a dictionary.

```python
def print_info(**info):
    for key, value in info.items():
        print(f"{key}: {value}")

print_info(name="Alice", age=30, city="New York")
```