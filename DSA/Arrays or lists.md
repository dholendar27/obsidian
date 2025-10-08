- The Arrays is data strucutre which allow us to store the multiple elements in a single variables.
- The Arrays store the data in a contiguous block of memory
- The Arrays has number associated to the each value named as index

There are two types of Arrays. They are:

1. Static Array
2. Dynamic Array

## Static Arrays

static type Arrays the size of the array is determined at the time of the declaration and cannot be altered during the runtime. This is type of arrays are used in the languages like `C` and `C++`. The Static arrays are generally faster as they the have fixed size and contigious memory and direct indexing.

```python
int StaticArray[10];
```

![[images/dynamic array.png]]
## Dynamic Arrays

The size of the array can be changed at runtime as need allowing it to grow and shrink. memory of the dynamic array is typically allocated on heap. 

The performance the dynamic array are slightly slower than the static array.

```python
numbers = [1,2,3,4,5,6]
```