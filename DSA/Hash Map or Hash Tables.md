A hash table is data structure that maps key to values. It uses a hash function to compute an index into a array of bucket of slots, from which the desired value can be found.
In python the hash tables are implement using the built in dict type
``` python
user = {
	"name":"Venkatasai dholendar reddy",
	"age":26,
	"DOB":"27-03-1999"
}
```
## Hash function
A hash function is a function that takes an input (key) and return a fixed-size integer (hash code). This hash code is then used as an index to store the value in a hash table.
In the hash table we convert the key into the hash using the hash function and point it to an value.
## Hash collisions
A hash function happens when two different keys produce the same index after applying the hash function.
### Why Do Collisions Happen?
Because:
- A **hash table has limited size** (e.g., 10 slots),
- But there are **many possible keys** (possibly infinite),
- So some keys will inevitably hash to the **same index**.
This is known as the **Pigeonhole Principle**.
![[hash collisions.png]]
