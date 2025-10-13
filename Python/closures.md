A **closure** in Python is just a **function inside another function** that **remembers** the values from its "parent" function, even after the parent function is done running.

Here’s a simple way to think about it:
1. You have an **outer function** (a big function).
2. Inside that, you create an **inner function** (a small function).
3. The inner function can use things (like variables) from the outer function, even after the outer function has finished.

```python
def outer_function(outer_value):
    def inner_function(inner_value):
        return outer_value + inner_value  # Uses outer_value from the outer function
    return inner_function  # We return the inner function

# Get the inner function (closure)
closure = outer_function(5)

# Call the inner function (closure) with 10
print(closure(10))  # It remembers 'outer_value' is 5, so it adds 5 + 10 = 15

```

### What happens here?

- `outer_function` is the big function, and it takes `outer_value` (5).
- Inside it, we define `inner_function`, which uses both `outer_value` (from the outer function) and `inner_value` (its own input).
- We return `inner_function`, and even after `outer_function` finishes, `inner_function` still **remembers** `outer_value`.