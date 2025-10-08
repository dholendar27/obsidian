The BIG-O notation is defined as a strandardized way to assess algorithm effciency, help to find the runtime of an algorithm with increasing input size. This is the important to find the scalability of the code. 

![[images/Big-0 Notation.png]]

## Rules

There are four rules that we need to follow when we are calculating the Time complexites

1. Worst Case
2. Remove Constants
3. Different terms of inputs
4. Drop Non Dominants

### 1. **Worst Case**

**What it means**: When calculating time complexity, we usually consider the worst-case scenario—i.e., the maximum number of operations that could happen based on the input size. This helps in understanding how an algorithm will perform even with the largest inputs.

#### Example:

Let’s say you have a list of 5 numbers and you’re searching for a specific number:

```python
numbers = [1, 3, 5, 7, 9]
target = 7

for num in numbers:  # Loop through each element
    if num == target:
        print("Found!")
```

- **Best Case**: If the number you're searching for is the first item, the loop only runs once.
- **Worst Case**: If the number is the last item (or not in the list at all), the loop will run through all 5 items.

So, **Worst Case Time Complexity** = **O(n)**, where `n` is the size of the list (5 in this case).

### 2. **Remove Constants**

**What it means**: When calculating time complexity, **ignore constant factors**. For example, if an algorithm takes 2n steps, it still counts as **O(n)** because constant factors don't affect the overall growth rate.

#### Example:

Consider the following code:

```python
n = 10
for i in range(n):    # Loop 10 times
    print(i)

for i in range(n):    # Loop again 10 times
    print(i)
```

Here, there are **2 loops** each running **n** times. In total, we have **2n** operations. However, when calculating time complexity, **we ignore the constant** (the number 2), so it becomes **O(n)**.

### 3. **Different Terms of Inputs**

**What it means**: If an algorithm has multiple inputs, we combine their complexities. When both inputs grow, the time complexity should reflect how both affect the running time.

#### Example:

Consider this scenario where we have two lists:

```python
list1 = [1, 2, 3]
list2 = [4, 5, 6, 7, 8]

for item in list1:
    for item2 in list2:
        print(item, item2)
```

- **List1** has 3 elements (`n = 3`).
- **List2** has 5 elements (`m = 5`).

The algorithm checks **every combination** of `n` and `m`. So, the total time complexity is **O(n * m)**, which accounts for both the sizes of the two lists.

### 4. **Drop Non-Dominants**

**What it means**: When you have multiple terms in the time complexity expression (like O(n + n²)), you only keep the **largest term** because it dominates the growth rate as the input size increases. Smaller terms become negligible for large inputs.

#### Example:

Let’s say you have the following time complexity for an algorithm:

```python
O(n + n²)
```

- As `n` grows, **n²** becomes much larger than `n`. For example, if `n = 1000`, `n² = 1,000,000`, but `n = 1000` is much smaller in comparison.

So, the time complexity is dominated by **n²**, and the smaller `n` term is ignored.

**Final Complexity** = **O(n²)**

## O(n)

```python
keyword = ["nemo"]
keywords = ["Marlin", "Nemo", "Dory", "Bruce", "Gill", "Bloat", "Peach", "Deb", "Jacques", "Nigel", "Crush", "Squirt", "Darwin", "Gurgle", "Bubbles", "Jacques", "Anchor", "Chum"]

for i in keywords:
  if i == "Nemo":
    print("Found Nemo!")
```

#### Explanation

In the given code, we loop through each item in the list `keywords` to check if it equals `"Nemo"`.

    - The number of times the loop runs depends on how many items are in `keywords`. If the list has `n` items, the loop runs `n` times.
    - This is why the time complexity is **O(n)**, meaning it grows linearly as the list size increases.

    #### Key Points:

    - If the list has 1 item, it runs once.
    - If the list has 18 items, it runs 18 times.
    - The time taken increases directly with the size of the list.

    That's why the time complexity is **O(n)**.

![[images/O(n)-graph.png]]

    If you plot the number of iterations (y-axis) versus the size of the list (x-axis), you'll see a linear relationship between the two. As the list grows larger, the number of iterations increases proportionally, forming a straight line on the graph.

## O(n+m)

```python
list1 = [1,2,3,4,5]
list = [6,7,8,9]

for i in list:
  print(i)

for j in list1:
  print(j)
```

#### Explanation:

- The first loop runs `n` times (because `list` has `n` elements).
- The second loop runs `m` times (because `list1` has `m` elements).
- Since these loops are sequential (not nested), the total time complexity is **O(n + m)**.

#### In General:

- If you're processing two separate inputs (lists, arrays, or strings) where each has its own size, you add their complexities.
- For example, if you loop through two lists, one with size `n` and the other with size `m`, the overall time complexity is **O(n + m)**.

## O(n^2)

```python
n = 5
for i in range(n):
    for j in range(n):
        print(i, j)
```

#### Explanation:

    - The outer loop runs `n` times (where `n = 5`).
    - For each iteration of the outer loop, the inner loop also runs `n` times.
    So, if `n = 5`, the total number of iterations will be:
    >> 5×5=25
    This results in **O(n²)** because you are performing `n` operations for each of the `n` elements, leading to a quadratic relationship between the input size and the number of operations.
    #### When Does **O(n²)** Happen?

    - **Nested loops** that iterate over the same data are usually the cause.
    - This is common in algorithms that need to compare each element to every other element, like bubble sort, insertion sort, and selection sort.

## O(n*m)

```python
n = 3  # Size of list1
m = 4  # Size of list2

list1 = [1, 2, 3]
list2 = ['a', 'b', 'c', 'd']

for i in range(n):  # Looping over list1
    for j in range(m):  # Looping over list2
        print(list1[i], list2[j])

```

#### Explanation:

    - The outer loop runs `n` times (in this case, `n = 3`).
    - For each iteration of the outer loop, the inner loop runs `m` times (in this case, `m = 4`).
    This means the inner loop runs **for every element** in the second list **for each element** in the first list, resulting in a total of `n * m` iterations.

    In this example:
    - `n = 3` (for list1)
    - `m = 4` (for list2)

    So, the total number of iterations would be:

    >> 3×4=123
    Thus, the time complexity is **O(n * m)**.
    #### When Does **O(n * m)** Happen?

    - When you're working with two datasets, and you need to compare every element in the first dataset with every element in the second.
    - Common in algorithms like matrix multiplication, comparing pairs from two lists, or checking relationships between two groups of items.

## O(n!)

Imagine you want to list **all possible ways to arrange** `n` different books on a shelf.

If you have 3 books: A, B, and C, how many different ways can you arrange them?

- ABC
- ACB
- BAC
- BCA
- CAB
- CBA

There are **6** ways — and that’s **3! = 3 × 2 × 1 = 6**.

Explanation:

- The number of permutations of a list with `n` elements is **n!** (factorial of n).
- So if the list has `n` elements, this function will run approximately **n! times**.

#### In General:

- If your code needs to **try every possible ordering** of `n` elements, it usually runs in **O(n!)** time.
- This happens in problems like generating permutations, the traveling salesman brute force, or some backtracking problems.
- Since factorial grows **super fast**, **O(n!)** algorithms become very slow even for small `n`.

## **O(1) — Constant Time**

```python
my_list = [10, 20, 30, 40, 50]
print(my_list[2])
```

#### **Explanation:**

- This code accesses a specific index in a list.
- No matter how large the list is, accessing an element at index `2` always takes **the same amount of time**.
- You're not looping or doing anything that depends on the size of the input.

> So, the time it takes **does not grow** with the size of the list.

#### **Key Points:**

- Operations like:
    - Accessing an element by index (`list[i]`)
    - Setting a value at an index (`list[i] = x`)
    - Basic arithmetic (`x + y`)
    - Boolean comparisons (`x == y`)
- These all run in **O(1)** time.

## O(logn) (Logarithmic Time)

This complexity means that the runtime grows **logarithmically** as the input size n increases. An algorithm with this complexity becomes more efficient the larger the input size gets. A classic example is **binary search** on a sorted array.
#### Explanation
Think of searching for a word in a dictionary. You don't start at page one and flip through every single page. Instead, you open the dictionary to the middle. If your word is alphabetically before that page, you discard the second half. If it's after, you discard the first half. You repeat this process of cutting the search space in half until you find the word. This is much faster than checking every page.
- **Key takeaway:** The input size is repeatedly **halved** in each step. This is incredibly efficient for large datasets.

## O(nlogn)

This complexity is common in efficient sorting algorithms like **merge sort** and **quicksort**. It's slightly slower than linear time O(n) but much faster than quadratic time O(n2).

#### Easy Explanation

Imagine you have a deck of n playing cards that you want to sort.

- The logn part comes from repeatedly **dividing** the list into halves, just like in binary search or a merge sort algorithm. This process creates a tree-like structure of divisions.
- The n part comes from the work done at each level of this division. For example, in merge sort, at each level of the tree, you have to do about n comparisons or operations to merge the sorted sub-lists back together.
- **Key takeaway:** The algorithm divides the problem into smaller subproblems (logn) and then combines the results of those subproblems, with each combination step taking linear time (n).

