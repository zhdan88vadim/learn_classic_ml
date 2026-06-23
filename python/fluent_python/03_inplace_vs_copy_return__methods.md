Excellent question! You're asking about **methods that have both an in-place version and a copy-returning version** (like `.sort()` vs `sorted()`). Let me list all the major ones:

## 1. **Reversing**

| In-place (modifies) | Returns Copy |
|---------------------|--------------|
| `list.reverse()` | `reversed(list)` (returns iterator) or `list[::-1]` |

```python
# In-place
fruits = ['apple', 'banana', 'cherry']
fruits.reverse()
print(fruits)  # ['cherry', 'banana', 'apple']

# Returns copy
fruits = ['apple', 'banana', 'cherry']
new_list = list(reversed(fruits))  # ['cherry', 'banana', 'apple']
# OR
new_list = fruits[::-1]  # ['cherry', 'banana', 'apple']
print(fruits)  # Still ['apple', 'banana', 'cherry']
```

---

## 2. **Removing Elements**

| In-place (modifies) | Returns Copy |
|---------------------|--------------|
| `list.remove()` | `filter()` or list comprehension |
| `list.pop()` | `list[:-1]` (remove last) |

```python
# Remove first occurrence of an element
fruits = ['apple', 'banana', 'cherry', 'banana']
fruits.remove('banana')  # In-place, removes first 'banana'
print(fruits)  # ['apple', 'cherry', 'banana']

# Returns copy without element
fruits = ['apple', 'banana', 'cherry', 'banana']
new_fruits = [x for x in fruits if x != 'banana']
print(new_fruits)  # ['apple', 'cherry']
print(fruits)  # ['apple', 'banana', 'cherry', 'banana']

# Pop vs copy without last
fruits = ['apple', 'banana', 'cherry']
last = fruits.pop()  # In-place, returns 'cherry'
print(fruits)  # ['apple', 'banana']

fruits = ['apple', 'banana', 'cherry']
new_fruits = fruits[:-1]  # Returns copy without last
print(new_fruits)  # ['apple', 'banana']
print(fruits)  # ['apple', 'banana', 'cherry']
```

---

## 3. **Adding/Extending**

| In-place (modifies) | Returns Copy |
|---------------------|--------------|
| `list.append()` | `list + [item]` |
| `list.extend()` | `list + other_list` |

```python
# Append
fruits = ['apple', 'banana']
fruits.append('cherry')  # In-place
print(fruits)  # ['apple', 'banana', 'cherry']

fruits = ['apple', 'banana']
new_fruits = fruits + ['cherry']  # Returns copy
print(new_fruits)  # ['apple', 'banana', 'cherry']
print(fruits)  # ['apple', 'banana']

# Extend
fruits = ['apple', 'banana']
fruits.extend(['cherry', 'date'])  # In-place
print(fruits)  # ['apple', 'banana', 'cherry', 'date']

fruits = ['apple', 'banana']
new_fruits = fruits + ['cherry', 'date']  # Returns copy
print(new_fruits)  # ['apple', 'banana', 'cherry', 'date']
print(fruits)  # ['apple', 'banana']
```

---

## 4. **Inserting**

| In-place (modifies) | Returns Copy |
|---------------------|--------------|
| `list.insert()` | Slicing assignment with copy |

```python
# Insert at index
fruits = ['apple', 'cherry']
fruits.insert(1, 'banana')  # In-place
print(fruits)  # ['apple', 'banana', 'cherry']

# Returns copy
fruits = ['apple', 'cherry']
new_fruits = fruits[:1] + ['banana'] + fruits[1:]
print(new_fruits)  # ['apple', 'banana', 'cherry']
print(fruits)  # ['apple', 'cherry']
```

---

## 5. **Modifying/Updating**

| In-place (modifies) | Returns Copy |
|---------------------|--------------|
| Index assignment | `list[:]` with new value |
| `list.sort()` | `sorted()` |
| `list.reverse()` | `reversed()` |

```python
# Modify by index
fruits = ['apple', 'banana', 'cherry']
fruits[1] = 'blueberry'  # In-place
print(fruits)  # ['apple', 'blueberry', 'cherry']

# Returns copy
fruits = ['apple', 'banana', 'cherry']
new_fruits = fruits.copy()
new_fruits[1] = 'blueberry'
print(new_fruits)  # ['apple', 'blueberry', 'cherry']
print(fruits)  # ['apple', 'banana', 'cherry']
```

---

## 6. **Dictionary Methods**

| In-place (modifies) | Returns Copy |
|---------------------|--------------|
| `dict.update()` | `{**dict, key: value}` or `dict | other_dict` (Python 3.9+) |

```python
# Update
d = {'a': 1, 'b': 2}
d.update({'c': 3})  # In-place
print(d)  # {'a': 1, 'b': 2, 'c': 3}

# Returns copy
d = {'a': 1, 'b': 2}
new_d = {**d, 'c': 3}  # Returns copy
print(new_d)  # {'a': 1, 'b': 2, 'c': 3}
print(d)  # {'a': 1, 'b': 2}
```

---

## 7. **String Methods (Always Return Copy)**

Strings are **immutable**, so ALL string methods return new strings (no in-place version):

```python
text = "hello world"

# All return copies
upper_text = text.upper()
lower_text = text.lower()
replaced_text = text.replace('world', 'python')
split_text = text.split()

print(text)  # Still "hello world" - unchanged
```

---

## 8. **Set Methods**

| In-place (modifies) | Returns Copy |
|---------------------|--------------|
| `set.update()` | `set | other_set` |
| `set.intersection_update()` | `set & other_set` |
| `set.difference_update()` | `set - other_set` |
| `set.symmetric_difference_update()` | `set ^ other_set` |

```python
# Update (add elements)
s1 = {1, 2, 3}
s1.update({4, 5})  # In-place
print(s1)  # {1, 2, 3, 4, 5}

s1 = {1, 2, 3}
s2 = s1 | {4, 5}  # Returns copy
print(s2)  # {1, 2, 3, 4, 5}
print(s1)  # {1, 2, 3}
```

---

## Complete Reference Table

| Operation | In-place Method | Copy-Returning Equivalent |
|-----------|----------------|---------------------------|
| **Sort** | `list.sort()` | `sorted(list)` |
| **Reverse** | `list.reverse()` | `reversed(list)` or `list[::-1]` |
| **Add element** | `list.append()` | `list + [item]` |
| **Add multiple** | `list.extend()` | `list + other_list` |
| **Remove element** | `list.remove()` | List comprehension |
| **Pop last** | `list.pop()` | `list[:-1]` |
| **Insert** | `list.insert()` | Slicing |
| **Update dict** | `dict.update()` | `{**dict, key: value}` |
| **Set union** | `set.update()` | `set \| other_set` |
| **Set intersection** | `set.intersection_update()` | `set & other_set` |
| **Set difference** | `set.difference_update()` | `set - other_set` |

---

## Memory/Performance Consideration

```python
import time

# In-place is more memory efficient
data = list(range(1000000))

# In-place: uses less memory
data.sort()  # O(1) extra memory

# Copy: uses more memory
sorted_data = sorted(data)  # O(n) extra memory

# For very large lists, prefer in-place when possible
```

**Key Rule of Thumb:** Use in-place methods when you don't need the original data anymore. Use copy-returning methods when you need to preserve the original for later use or when working with immutable types (strings, tuples).