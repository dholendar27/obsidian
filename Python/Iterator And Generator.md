## Iterator

Iterator is an object which can iterated. It implements iterator protocal which provides

- `iter()` → Which return the iterator object
- `next()` → returns the next item in sequence and raise an stopIteration Error which all the items are completed

We can create an custom iterator using the `__iter__()` and `__next__()` 

```python
class iterator():
    def __init__(self, data):
        self.data = data
        self.index = 0
        
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.index < len(self.data):
            val =  self.data[self.index]
            self.index += 1
            return val
        else:
            raise StopIteration
```

## Generator

Generator is a simpler way to write a iterator using the function and the `yield` keyword.

Everytime when the the generator function is called it pauses the function and remembers the data. when resume execution continues from where it left off.

```python
def iterator(data):
    for i in data:
        yield i
        
it = iterator([10,20,30])
print(next(it))
print(next(it))
print(next(it))
```

## Difference between iterator and Generator

| **Feature**       | **Iterator**                                                     | **Generator**                                                               |
| ----------------- | ---------------------------------------------------------------- | --------------------------------------------------------------------------- |
| **Definition**    | Class-based, manually maintains state                            | Function-based, uses `yield` to maintain state                              |
| **Code size**     | More verbose (need class, `__iter__`, `__next__`)                | Very concise (just a function with `yield`)                                 |
| **Use Case**      | When you need full control over iteration logic or complex state | When you need simple, efficient iteration (esp. for large or infinite data) |
| **Memory**        | Can be memory-efficient if implemented well                      | Automatically memory-efficient (lazy evaluation)                            |
| **Reusability**   | Often reusable with reset logic                                  | Generally single-use (unless re-invoked)                                    |
| **State Control** | You manage the state manually                                    | Python handles it automatically with `yield`                                |