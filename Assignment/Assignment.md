3. Group the anagrams from an array of strings.
	Example: ["eat", "tea", "tan", "ate", "nat", "bat"] -> `[["eat", "tea", "ate"], ["tan", "nat"], ["bat"]]`

```python
	from collections import defaultdict
	strs = ["eat","tea","tan","ate","nat","bat"]
	d = defaultdict(list)
	
	for word in strs:
	    key = ''.join(sorted(word))
	    d[key].append(word)
	
	print(list(d.values()))
```

5. Given an array of strings, group words that have the same set of characters (ignoring order and frequency). For example, "may" and "yam" are grouped together because they contain the same characters.

	Example: Input: ["may", "yam", "act", "cat", "army"] → Output: `[["may", "yam"], ["act", "cat"], ["army"]]`

```python
	from collections import defaultdict
	strs = ["may", "yam", "act", "cat", "army"]
	d = defaultdict(list)
	
	for word in strs:
	    key = ''.join(sorted(set(word)))
	    d[key].append(word)
	
	print(list(d.values()))
```

4. Given two strings s and goal, determine if s can be transformed into goal by rotating s (i.e., moving some characters from the start to the end). For example, if s="abcde", a rotation could yield "cdeab".
Example:
	a. Input: s="abcde",goal="cdeab"→ Output: True 
	b. Input: s="abcde",goal="abced"→ Output: False

```python
	def is_rotated(s, goal):
	    if len(s) != len(goal):
	        return False
	    if not s:
	        return False
	        
	    ss = s + s
	    return goal in ss
```