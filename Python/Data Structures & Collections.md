There are Four Important data structures in the python which are most oftenly used. These datastructures are used to store multiple values in a single variable. They are 

1. Lists
2. Tuples
3. set
4. dictionary

## Lists

List are used to store multiple values of different types in a single variables. we can also store the nested lists and other types of data structures. The list are mutable (We can change the values in particular index), ordered (they maintain the order in the same way we have added them) and allow duplicate (we can have the same values)

### Operations

We have three different ways to add the data into the list

1. Adding the list: we can add the data into list using the `append()` method.

```python
new_list = []
new_list.append(1)
new_list.append("b")
```

1. Combining two list: we can combine list1 and list2 using the `extend()` method. 

```python
list_1 = [1,2,3,4,5]
list_2 = [6,7,8,9,0]
list_1.extend(list_2) # the list_1 will be updated with the extended data
```

1. Inserting data at sepecific position: We can insert the data at the specific index using the `insert(index,value)` method.

```python
fruits = ["apple",'mango','pineapple']
fruits.insert(1,"kiwi")
```

list have three ways to delete the data

1. Remove value/item from the list: we can use the `remove(item)` method to remove the data

```python
list_1 = [1,2,3,4,5]
list_1.remove(2) # it remove the value 2 in the list
```

1. Remove last values: We can use the `pop()` method to remove the last value of the list

```python
fruits = ["apple",'mango','pineapple']
fruits.pop() # "pineapple" will be removed from the list
fruits.pop() # "mango" will be removed from the list
```

    1. clear all the values in the list: we can use the `clear()` method to remove all the values from the list

```python
fruits = ["apple",'mango','pineapple']
fruits.clear()
```

someother mostly used methods

1. Retrieve the index of the value: we can use the `index()` method to retrieve the index of the first occuring calue of the specified items

```python
fruits = ["apple",'mango','pineapple',"apple"]
fruits.index("apple") # provides the index "0" as is the appeared first in the search
```

1. Sort the elements in the list: we can use the `sort()`  method to arrange the value in ascending order.

```python
list_1 = [5,6,1,4,7,8,2,0]
list_1.sort()
```

1. reverse the list: we can use the `reverse()` method to reverse all the values in the list.

```python
fruits = ["apple",'mango','pineapple']
fruits.reverse()
```

## Tuple

Tuples are also used to save the multiple values in the same variable. Tuple is immutable (once the tuple is created we cannot add or change the data), ordered (they maintain the position) and allow duplicates.

### Operations

1. Creating a tuple: There are different ways to create tuples. 

```python
tuple_1 = (1,2,3) # with values
tuple_2 = () # Empty tuple
tuple_3 = tuple()
```

common operations of tuple:

    1. Accessing Elements: we can access tha value of an tuple using the index

```python
tuple_1 = (1,2,3)
tuple_1[1]
```

    2. Concatenation: We can combine two list using `+` operator

```python
tuple_1 = (1,2,3)
tuple_2 = (4,5,6)
print(tuple_1 + tuple_2)
```

    3. Delete Tuple: we can delete the tuple using the `del` keyword

```python
del tuple_1
```

## Sets

Sets are unordered (they won't maintain the order), immutable (we can add the new values but we cannot update the old values) and set doesn't allow duplicated

### Operations

    1. Creating set: 

```python
set_1 = {1,2,3}
set_2 = set()
```

1. Adding data: we can add the data into set using two different methods

```python
set_1.add(4) # we can use the `add()` method to single value
set_1.update([5,6]) # we can use the `update()` method to multiple value
```

    3. Removing values from sets: we can remove the elemts using remove() and discard

```python
set_1.remove(2) # we need to pass the value that need to removed. if the value is not present it raises an error
set_1.discard(3) # if the value is not present no error is raised
set_1.pop() # pop/remove the last element from the set
set_1.clear() # clear all the elements in the set
```

    4. Union, intersection, Difference:

```python
set_1 = {1,2,3,4}
set_2 = {2,5,6,7}
print(set_1.intersection(set_2)) # intersect provides the common values in the both sets
print(set_1.difference(set_2)) # provides the distinct values in the set_1 by removing the common values which are in set_1 and set_2
print(set_1.union(set_2)) # combines the both values sets and provide the distinct values

```

## Dictionary

In python we use the dictionary to store the data in the key-value pair. this means we need to have an key and assing the keys with values. there values can be of different types. The dictionay are mutable (only the values are mutable, we cannot edit the keys). we cannot have the duplicate keys. and unordered.

### Operations

        1. Creating a dictionary

```python
dict_1 = {'key1':"value1","key2":"value2"}
empty_dict = {}
empty_dict_with_object = dict()
```

        2. Inserting data and merging:

```python
dict_1["key1"] = "value3" # we can add or update the data in the same way
dict_1.update(dict_2)
```

        3. remove the data: we can remove the data in dictionary in different ways

```python
dict_1.pop("key1") # in the dict for the pop we need to pass the key
dict_1.clear() # Remove all the values from the dict
del dict1["key2"] # we can also remove the value using the del keyword
```

        4. common operation:

```python
dict_1.keys() # list all the keys in the dict
dict_1.values() # list all the values in dict
dict_1.items() # list both keys and values.
```

## Collections

### DefaultDict

The defaultdict is an subclass of the default `dict` class. The main difference is that if the value is not present in the dict it used the default factory function and provide an value without throwing an error

```python
from collections import defaultdict

d = defaultdict(int)
print(d[a]) -> 0 # the value will 0, the defaultdict added the default value
```

### Counter

The counter is an subclass of `dict` designed for counting hashable objects. The counter counts the fequerency of the each element in the iterable (string, list,tuple)

```python
from collections import Counter

c  = Counter('banana')
print(c)  # Output: Counter({'a': 3, 'n': 2, 'b': 1})
```

### deque

The deque is an double-ended queue which can add and remove elements from the both ends of the collection.

```python
append(item): # Adds an item to the right end.
appendleft(item): # Adds an item to the left end.
pop(): # Removes and returns an item from the right end.
popleft(): # Removes and returns an item from the left end.
extend(iterable): # Extends the deque by appending elements from an iterable to the right.
extendleft(iterable): # Extends the deque by prepending elements from an iterable to the left.
rotate(n): #Shifts elements by n positions. A positive n rotates to the right, a negative n rotates to the left. 
```