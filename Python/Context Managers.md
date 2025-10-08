The Context managers are used for the resource management. This helps to make sure the resource has been used acquired, used properly and released or clean up even with errors and exceptions.

This is usefule when we are working with resources like files, network connections and database handles.

The context manager are objects which contains two methods which are`__enter__` and `__exit_` . The `__enter__` method is classes when enter the with block and it return the values assigned to the variables within that block. The `__exit__` methods is calles whrn the block exist

## Custom context manager

### Class-Based Approach

The class-based approach is the most structured and flexible method for writing context managers. Here, you define a class that implements the special methods `__enter__` and `__exit__`.

```python
import time

class Timer:
   def __enter__(self):
       self.start_time = time.time()
       return self

   def __exit__(self, exc_type, exc_value, traceback):
       self.end_time = time.time()
       elapsed_time = self.end_time - self.start_time
       print(f"Elapsed time: {elapsed_time} seconds")


# Example usage
if __name__ == "__main__":
   with Timer() as timer:
       # Code block to measure the execution time
       time.sleep(2)  # Simulate some time-consuming operation
Elapsed time: 2.002082347869873 seconds
```

Function-Based Approach

```python
import time
from contextlib import contextmanager


@contextmanager
def timer():
   start_time = time.time()
   yield
   end_time = time.time()
   elapsed_time = end_time - start_time
   print(f"Elapsed time: {elapsed_time} seconds")


# Example usage
if __name__ == "__main__":
   with timer():
       time.sleep(2)
Elapsed time: 2.0020740032196045 seconds
```