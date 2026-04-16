# Python: A Comprehensive Guide

> A practical, plain-English guide to the Python programming language — from the fundamentals to intermediate concepts — with thorough examples throughout.

---

## Table of Contents

1. [Variables and Data Types](#1-variables-and-data-types)
2. [Strings](#2-strings)
3. [Data Structures](#3-data-structures)
4. [Conditionals](#4-conditionals)
5. [Loops](#5-loops)
6. [Functions](#6-functions)
7. [List Comprehensions](#7-list-comprehensions)
8. [Slicing](#8-slicing)
9. [Useful Built-in Functions](#9-useful-built-in-functions)
10. [Working with Files](#10-working-with-files)
11. [Error Handling](#11-error-handling)
12. [Modules and Imports](#12-modules-and-imports)
13. [Classes](#13-classes)
14. [Dataclasses](#14-dataclasses)
15. [Decorators](#15-decorators)
16. [Logging](#16-logging)
17. [Async Functions](#17-async-functions)
18. [DataFrames with pandas](#18-dataframes-with-pandas)

---

## 1. Variables and Data Types

A variable is a name that points to a value stored in memory. You create one by writing a name, an equal sign, and the value. Python figures out the type automatically — you never need to declare it.

```python
age = 30
name = "Alice"
temperature = 36.6
is_active = True
```

### Core data types

| Type | Name | Example | Description |
|---|---|---|---|
| `int` | Integer | `42` | Whole numbers (positive, negative, or zero) |
| `float` | Float | `3.14` | Decimal numbers |
| `str` | String | `"hello"` | Text, always in quotes |
| `bool` | Boolean | `True` / `False` | Logical values — only two possible |
| `NoneType` | None | `None` | Represents "nothing" or "no value" |

### Checking the type

```python
print(type(42))        # <class 'int'>
print(type(3.14))      # <class 'float'>
print(type("hello"))   # <class 'str'>
print(type(True))      # <class 'bool'>
print(type(None))      # <class 'NoneType'>
```

### Type conversion (casting)

You can convert between types explicitly:

```python
# String to integer
num = int("10")         # 10

# Integer to string
text = str(100)         # "100"

# String to float
price = float("19.99")  # 19.99

# Float to integer (truncates, does not round)
whole = int(9.99)        # 9

# Integer to boolean (0 is False, everything else is True)
print(bool(0))    # False
print(bool(1))    # True
print(bool(-5))   # True

# String to boolean (empty string is False, anything else is True)
print(bool(""))       # False
print(bool("hello"))  # True
```

### Multiple assignment

```python
# Assign multiple variables at once
x, y, z = 1, 2, 3

# Same value to multiple variables
a = b = c = 0
```

### Arithmetic operators

| Operator | Description | Example | Result |
|---|---|---|---|
| `+` | Addition | `5 + 3` | `8` |
| `-` | Subtraction | `5 - 3` | `2` |
| `*` | Multiplication | `5 * 3` | `15` |
| `/` | Division (always returns float) | `7 / 2` | `3.5` |
| `//` | Floor division (rounds down) | `7 // 2` | `3` |
| `%` | Modulo (remainder) | `7 % 2` | `1` |
| `**` | Exponentiation | `2 ** 3` | `8` |

### Augmented assignment

Instead of writing `x = x + 1`, you can write:

```python
x = 10
x += 5    # x is now 15
x -= 3    # x is now 12
x *= 2    # x is now 24
x /= 4    # x is now 6.0
x //= 2   # x is now 3.0
x **= 3   # x is now 27.0
x %= 5    # x is now 2.0
```

---

## 2. Strings

Strings are sequences of characters. You can create them with single quotes, double quotes, or triple quotes. They are **immutable** — once created, you cannot change individual characters in place. Every operation that looks like it modifies a string actually creates a new one.

```python
single = 'hello'
double = "hello"
multi_line = """This is
a multi-line
string."""
```

### String concatenation and repetition

```python
first = "Hello"
last = "World"

combined = first + " " + last   # "Hello World"
repeated = "ha" * 3              # "hahaha"
```

### f-strings (formatted strings)

The modern way to embed variables inside strings. Prefix the string with `f` and put variables or expressions inside curly braces:

```python
name = "Alice"
age = 30

print(f"My name is {name} and I am {age} years old.")
# My name is Alice and I am 30 years old.

# You can put any expression inside the braces
print(f"Next year I will be {age + 1}.")
# Next year I will be 31.

# Formatting numbers
pi = 3.14159
print(f"Pi to 2 decimal places: {pi:.2f}")
# Pi to 2 decimal places: 3.14

price = 1234567.89
print(f"Price: {price:,.2f}")
# Price: 1,234,567.89

# Padding and alignment
print(f"{'left':<20}")    # left-aligned, 20 chars wide
print(f"{'right':>20}")   # right-aligned
print(f"{'center':^20}")  # centered
```

### Common string methods

Strings have dozens of built-in methods. Here are the most useful:

```python
text = "  Hello, World!  "

# Case changes
text.upper()           # "  HELLO, WORLD!  "
text.lower()           # "  hello, world!  "
text.title()           # "  Hello, World!  "
text.capitalize()      # "  hello, world!  " → "  hello, world!  " (only first char)
text.swapcase()        # "  hELLO, wORLD!  "

# Whitespace removal
text.strip()           # "Hello, World!"      (removes leading and trailing whitespace)
text.lstrip()          # "Hello, World!  "    (left side only)
text.rstrip()          # "  Hello, World!"    (right side only)

# Searching
text.find("World")     # 9  (index of first occurrence, -1 if not found)
text.index("World")    # 9  (same, but raises ValueError if not found)
text.count("l")        # 3  (how many times "l" appears)
text.startswith("  He")  # True
text.endswith("!  ")     # True

# Replacing
text.replace("World", "Python")  # "  Hello, Python!  "

# Splitting and joining
sentence = "one,two,three"
parts = sentence.split(",")      # ["one", "two", "three"]
back = "-".join(parts)           # "one-two-three"

# Checking content
"hello".isalpha()      # True  (only letters)
"12345".isdigit()      # True  (only digits)
"hello123".isalnum()   # True  (letters and digits)
"   ".isspace()        # True  (only whitespace)
```

### Escape characters

| Character | Meaning |
|---|---|
| `\n` | Newline |
| `\t` | Tab |
| `\\` | Literal backslash |
| `\'` | Literal single quote |
| `\"` | Literal double quote |

```python
print("Line one\nLine two")
# Line one
# Line two

print("Column1\tColumn2")
# Column1    Column2
```

### String membership

```python
print("World" in "Hello, World!")      # True
print("world" in "Hello, World!")      # False (case-sensitive)
print("xyz" not in "Hello, World!")    # True
```

---

## 3. Data Structures

Python has four core built-in data structures. Think of them as different kinds of containers, each designed for a specific purpose.

| Structure | Ordered | Mutable | Duplicates | Syntax |
|---|---|---|---|---|
| **List** | Yes | Yes | Yes | `[1, 2, 3]` |
| **Tuple** | Yes | No | Yes | `(1, 2, 3)` |
| **Set** | No | Yes | No | `{1, 2, 3}` |
| **Dictionary** | Yes (3.7+) | Yes | Keys: No, Values: Yes | `{"a": 1}` |

- **Ordered** means items keep the position in which they were inserted, so you can access them by index.
- **Mutable** means you can add, remove, or change items after creation.
- **Duplicates** means the same value can appear more than once.

---

### 3.1 Lists

A list is the most versatile data structure. It is an ordered, mutable collection that can hold items of any type, including other lists.

```python
fruits = ["apple", "banana", "cherry"]
mixed = [1, "hello", 3.14, True, None]
nested = [[1, 2], [3, 4], [5, 6]]
empty = []
```

#### Accessing items

```python
fruits = ["apple", "banana", "cherry", "date", "elderberry"]

fruits[0]    # "apple"      (first item)
fruits[1]    # "banana"     (second item)
fruits[-1]   # "elderberry" (last item)
fruits[-2]   # "date"       (second to last)
```

#### Modifying items

```python
fruits = ["apple", "banana", "cherry"]

fruits[1] = "blueberry"
print(fruits)  # ["apple", "blueberry", "cherry"]
```

#### Adding items

```python
fruits = ["apple", "banana"]

# Add to the end
fruits.append("cherry")
print(fruits)  # ["apple", "banana", "cherry"]

# Insert at a specific position
fruits.insert(1, "avocado")
print(fruits)  # ["apple", "avocado", "banana", "cherry"]

# Add multiple items to the end
fruits.extend(["date", "elderberry"])
print(fruits)  # ["apple", "avocado", "banana", "cherry", "date", "elderberry"]
```

#### Removing items

```python
fruits = ["apple", "banana", "cherry", "banana", "date"]

# Remove first occurrence of a value
fruits.remove("banana")
print(fruits)  # ["apple", "cherry", "banana", "date"]

# Remove by index and return the value
popped = fruits.pop(1)
print(popped)   # "cherry"
print(fruits)   # ["apple", "banana", "date"]

# Remove last item
last = fruits.pop()
print(last)     # "date"

# Remove by index (no return value)
del fruits[0]
print(fruits)   # ["banana"]

# Remove all items
fruits.clear()
print(fruits)   # []
```

#### Other useful list methods

```python
numbers = [3, 1, 4, 1, 5, 9, 2, 6, 5]

numbers.sort()                  # Sorts in place: [1, 1, 2, 3, 4, 5, 5, 6, 9]
numbers.sort(reverse=True)      # Descending: [9, 6, 5, 5, 4, 3, 2, 1, 1]
numbers.reverse()               # Reverses in place: [1, 1, 2, 3, 4, 5, 5, 6, 9]
numbers.count(5)                # 2  (how many times 5 appears)
numbers.index(4)                # 4  (index of first occurrence of 4)

# sorted() returns a NEW sorted list (does not modify the original)
original = [3, 1, 2]
new_list = sorted(original)
print(original)   # [3, 1, 2] (unchanged)
print(new_list)   # [1, 2, 3]
```

#### Copying a list

Assigning a list to a new variable does **not** make a copy — both names point to the same list:

```python
a = [1, 2, 3]
b = a           # b points to the SAME list as a
b.append(4)
print(a)        # [1, 2, 3, 4]  — a was also changed!
```

To create an independent copy:

```python
a = [1, 2, 3]

b = a.copy()         # Method 1: .copy()
c = list(a)          # Method 2: list() constructor
d = a[:]             # Method 3: slice

b.append(4)
print(a)   # [1, 2, 3]  — a is unaffected
print(b)   # [1, 2, 3, 4]
```

> **Warning:** These create a **shallow copy**. If the list contains other lists (nested), the inner lists are still shared. For a fully independent copy of nested structures, use `import copy; copy.deepcopy(a)`.

---

### 3.2 Tuples

A tuple is like a list that **cannot be changed** after creation. It is ordered and allows duplicates, but it is **immutable**. Use tuples when you want to store a fixed collection of values that should not be modified.

```python
coordinates = (10.5, 20.3)
rgb = (255, 128, 0)
single = (42,)         # A single-element tuple NEEDS a trailing comma
not_a_tuple = (42)     # This is just the integer 42 in parentheses
empty = ()
```

#### Accessing items

Works exactly like lists:

```python
point = (10, 20, 30)
print(point[0])    # 10
print(point[-1])   # 30
```

#### Tuple unpacking

One of the most useful features of tuples is **unpacking** — assigning each element to a separate variable in one line:

```python
coordinates = (10.5, 20.3, 5.0)
x, y, z = coordinates
print(x)  # 10.5
print(y)  # 20.3
print(z)  # 5.0

# Swap two variables without a temporary variable
a, b = 1, 2
a, b = b, a
print(a, b)  # 2 1

# Use * to collect remaining items
first, *rest = (1, 2, 3, 4, 5)
print(first)  # 1
print(rest)   # [2, 3, 4, 5]     (rest becomes a list)

first, *middle, last = (1, 2, 3, 4, 5)
print(first)   # 1
print(middle)  # [2, 3, 4]
print(last)    # 5
```

#### Why use tuples over lists?

- **Safety**: You guarantee the data won't be accidentally modified.
- **Performance**: Tuples are slightly faster than lists because Python can optimize immutable objects.
- **Dictionary keys**: Tuples can be used as dictionary keys; lists cannot (because dictionary keys must be immutable).

#### Tuple methods

Tuples only have two methods because they can't be changed:

```python
t = (1, 2, 3, 2, 2, 4)
t.count(2)    # 3  (how many times 2 appears)
t.index(3)    # 2  (index of first occurrence of 3)
```

---

### 3.3 Sets

A set is an **unordered** collection of **unique** items. It automatically removes duplicates. Sets are excellent for membership testing ("is this item in the collection?") and for mathematical set operations (union, intersection, difference).

```python
colors = {"red", "green", "blue"}
numbers = {1, 2, 3, 2, 1}     # Duplicates are removed automatically
print(numbers)                  # {1, 2, 3}

# Create a set from a list (useful for removing duplicates)
names = ["Alice", "Bob", "Alice", "Charlie", "Bob"]
unique_names = set(names)
print(unique_names)  # {"Alice", "Bob", "Charlie"}

empty_set = set()    # You must use set(), not {} — {} creates an empty dictionary
```

#### Adding and removing items

```python
fruits = {"apple", "banana"}

fruits.add("cherry")
print(fruits)  # {"apple", "banana", "cherry"}

fruits.discard("banana")   # Removes "banana" — does nothing if not found
fruits.remove("apple")     # Removes "apple" — raises KeyError if not found

fruits.clear()             # Empties the set
```

#### Set operations

This is where sets truly shine:

```python
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

# Union — all items from both sets
a | b                 # {1, 2, 3, 4, 5, 6}
a.union(b)            # same

# Intersection — only items in BOTH sets
a & b                 # {3, 4}
a.intersection(b)     # same

# Difference — items in a but NOT in b
a - b                 # {1, 2}
a.difference(b)       # same

# Symmetric difference — items in either set but NOT in both
a ^ b                 # {1, 2, 5, 6}
a.symmetric_difference(b)  # same
```

```
Set a:  {1, 2, 3, 4}
Set b:       {3, 4, 5, 6}

Union:        {1, 2, 3, 4, 5, 6}   ← everything
Intersection:      {3, 4}          ← overlap
Difference (a-b): {1, 2}           ← only in a
Symmetric diff:   {1, 2, 5, 6}    ← not shared
```

#### Set membership testing

Checking if an item is in a set is extremely fast — much faster than checking a list:

```python
allowed = {"admin", "editor", "viewer"}

print("admin" in allowed)     # True
print("hacker" in allowed)    # False
```

#### Subset and superset

```python
a = {1, 2, 3}
b = {1, 2, 3, 4, 5}

a.issubset(b)      # True  — every element of a is in b
b.issuperset(a)    # True  — b contains every element of a
a.isdisjoint({6})  # True  — no common elements
```

---

### 3.4 Dictionaries

A dictionary stores data as **key-value pairs**. Think of it like a real dictionary: you look up a word (the key) and get its definition (the value). Keys must be unique and immutable (strings, numbers, or tuples). Values can be anything.

```python
person = {
    "name": "Alice",
    "age": 30,
    "city": "London"
}

empty = {}
```

#### Accessing values

```python
person = {"name": "Alice", "age": 30, "city": "London"}

# Using brackets — raises KeyError if key doesn't exist
person["name"]      # "Alice"
person["age"]       # 30

# Using .get() — returns None (or a default) if key doesn't exist
person.get("name")           # "Alice"
person.get("email")          # None
person.get("email", "N/A")  # "N/A"
```

> **Tip:** Use `.get()` when you are not sure the key exists. Use brackets when you are certain it does and want an error if it's missing.

#### Adding and updating items

```python
person = {"name": "Alice", "age": 30}

# Add a new key-value pair
person["email"] = "alice@example.com"

# Update an existing value
person["age"] = 31

# Update multiple keys at once
person.update({"city": "Paris", "age": 32})

print(person)
# {"name": "Alice", "age": 32, "email": "alice@example.com", "city": "Paris"}
```

#### Removing items

```python
person = {"name": "Alice", "age": 30, "city": "London"}

# Remove by key and return the value
age = person.pop("age")
print(age)     # 30

# Remove by key — returns default if key doesn't exist (prevents KeyError)
email = person.pop("email", "not found")
print(email)   # "not found"

# Remove a specific key (no return value)
del person["city"]

# Remove the last inserted key-value pair
last = person.popitem()
print(last)    # ("name", "Alice") — returns a tuple

person.clear()  # Empties the dictionary
```

#### Looping through dictionaries

```python
person = {"name": "Alice", "age": 30, "city": "London"}

# Loop through keys (default behavior)
for key in person:
    print(key)
# name
# age
# city

# Loop through values
for value in person.values():
    print(value)
# Alice
# 30
# London

# Loop through both keys and values
for key, value in person.items():
    print(f"{key}: {value}")
# name: Alice
# age: 30
# city: London
```

#### Checking if a key exists

```python
person = {"name": "Alice", "age": 30}

print("name" in person)       # True
print("email" in person)      # False
print("email" not in person)  # True
```

#### Getting all keys, values, or pairs

```python
person = {"name": "Alice", "age": 30, "city": "London"}

list(person.keys())     # ["name", "age", "city"]
list(person.values())   # ["Alice", 30, "London"]
list(person.items())    # [("name", "Alice"), ("age", 30), ("city", "London")]
```

#### Nested dictionaries

Dictionaries can contain other dictionaries — this is very common when working with JSON data from APIs:

```python
users = {
    "user1": {
        "name": "Alice",
        "age": 30,
        "hobbies": ["reading", "cycling"]
    },
    "user2": {
        "name": "Bob",
        "age": 25,
        "hobbies": ["gaming", "cooking"]
    }
}

# Access nested data
print(users["user1"]["name"])           # "Alice"
print(users["user2"]["hobbies"][0])     # "gaming"
```

#### Dictionary comprehension

Just like list comprehensions, you can build dictionaries in a single line:

```python
# Create a dictionary of squares
squares = {x: x**2 for x in range(6)}
print(squares)  # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25}

# Filter: only even numbers
even_squares = {x: x**2 for x in range(10) if x % 2 == 0}
print(even_squares)  # {0: 0, 2: 4, 4: 16, 6: 36, 8: 64}

# Swap keys and values
original = {"a": 1, "b": 2, "c": 3}
flipped = {v: k for k, v in original.items()}
print(flipped)  # {1: "a", 2: "b", 3: "c"}
```

---

## 4. Conditionals

Conditionals let your program make decisions. The code inside a conditional block only runs if the condition evaluates to `True`.

### if, elif, else

```python
temperature = 35

if temperature > 30:
    print("It's hot outside.")
elif temperature > 20:
    print("Nice weather.")
elif temperature > 10:
    print("A bit chilly.")
else:
    print("It's cold.")

# Output: It's hot outside.
```

- `if` is checked first.
- `elif` (short for "else if") is checked only if the previous conditions were `False`. You can have as many `elif` as you need.
- `else` runs only if none of the above were `True`. It is optional.

### Comparison operators

| Operator | Meaning | Example |
|---|---|---|
| `==` | Equal to | `5 == 5` → `True` |
| `!=` | Not equal to | `5 != 3` → `True` |
| `>` | Greater than | `5 > 3` → `True` |
| `<` | Less than | `5 < 3` → `False` |
| `>=` | Greater than or equal | `5 >= 5` → `True` |
| `<=` | Less than or equal | `3 <= 5` → `True` |

### Logical operators

You can combine multiple conditions using `and`, `or`, and `not`:

```python
age = 25
has_id = True

# and — both must be True
if age >= 18 and has_id:
    print("Entry allowed.")

# or — at least one must be True
if age < 13 or age > 65:
    print("Discount applies.")

# not — inverts the value
if not has_id:
    print("ID required.")
```

### Chained comparisons

Python lets you chain comparisons naturally, like you would in math:

```python
age = 25

# Instead of: age >= 18 and age <= 65
if 18 <= age <= 65:
    print("Working age.")

# Instead of: x > 0 and x < 100
x = 42
if 0 < x < 100:
    print("x is between 0 and 100.")
```

### Truthy and falsy values

In Python, every value has a boolean interpretation. You don't always need an explicit comparison:

| Falsy (evaluates to `False`) | Truthy (evaluates to `True`) |
|---|---|
| `False` | `True` |
| `0`, `0.0` | Any non-zero number |
| `""` (empty string) | Any non-empty string |
| `[]` (empty list) | Any non-empty list |
| `{}` (empty dict) | Any non-empty dict |
| `set()` (empty set) | Any non-empty set |
| `None` | Everything else |

```python
name = ""

if name:
    print(f"Hello, {name}!")
else:
    print("Name is empty.")
# Output: Name is empty.

items = [1, 2, 3]
if items:
    print(f"There are {len(items)} items.")
# Output: There are 3 items.
```

### Ternary (inline) conditional

A compact way to write simple `if/else` in a single line:

```python
age = 20
status = "adult" if age >= 18 else "minor"
print(status)  # "adult"

# Equivalent to:
# if age >= 18:
#     status = "adult"
# else:
#     status = "minor"
```

### Membership operators (in, not in)

```python
fruits = ["apple", "banana", "cherry"]

if "banana" in fruits:
    print("We have bananas!")

if "mango" not in fruits:
    print("No mangoes available.")
```

### Identity operators (is, is not)

`is` checks if two variables point to the **exact same object in memory**, not just if they have the same value. Use `is` primarily for comparing with `None`:

```python
result = None

if result is None:
    print("No result yet.")

if result is not None:
    print(f"Got a result: {result}")
```

> **Warning:** Never use `is` to compare numbers or strings. Use `==` for value comparison. `is` is for identity (same object), `==` is for equality (same value).

### match-case (Python 3.10+)

Python 3.10 introduced **structural pattern matching**, similar to switch-case in other languages but more powerful:

```python
command = "start"

match command:
    case "start":
        print("Starting the engine.")
    case "stop":
        print("Stopping the engine.")
    case "pause":
        print("Pausing.")
    case _:
        print("Unknown command.")

# Output: Starting the engine.
```

The `_` is a wildcard — it matches anything and acts like the `else` or `default` branch.

You can also match against patterns:

```python
point = (0, 5)

match point:
    case (0, 0):
        print("Origin")
    case (x, 0):
        print(f"On the x-axis at x={x}")
    case (0, y):
        print(f"On the y-axis at y={y}")
    case (x, y):
        print(f"Point at ({x}, {y})")

# Output: On the y-axis at y=5
```

---

## 5. Loops

Loops let you repeat a block of code multiple times. Python has two kinds: `for` (iterate over something) and `while` (repeat while a condition is true).

### for loops

A `for` loop iterates over a **sequence** — a list, tuple, string, range, dictionary, or any other iterable.

```python
# Loop over a list
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)
# apple
# banana
# cherry

# Loop over a string (character by character)
for char in "hello":
    print(char)
# h
# e
# l
# l
# o

# Loop over a range of numbers
for i in range(5):       # 0, 1, 2, 3, 4
    print(i)

for i in range(2, 8):    # 2, 3, 4, 5, 6, 7
    print(i)

for i in range(0, 10, 2):  # 0, 2, 4, 6, 8 (step of 2)
    print(i)

for i in range(10, 0, -1):  # 10, 9, 8, ..., 1 (counting down)
    print(i)
```

### while loops

A `while` loop repeats as long as its condition is `True`. Be careful — if the condition never becomes `False`, the loop runs forever.

```python
count = 0
while count < 5:
    print(count)
    count += 1
# 0
# 1
# 2
# 3
# 4

# Common pattern: loop until user input
while True:
    answer = input("Type 'quit' to exit: ")
    if answer == "quit":
        break
```

### break, continue, pass

These three keywords control the flow inside a loop:

#### break — exit the loop immediately

```python
for number in range(10):
    if number == 5:
        break
    print(number)
# 0
# 1
# 2
# 3
# 4
# (5, 6, 7, 8, 9 are never printed — the loop stopped at 5)
```

#### continue — skip the rest of this iteration, jump to the next one

```python
for number in range(10):
    if number % 2 == 0:
        continue          # Skip even numbers
    print(number)
# 1
# 3
# 5
# 7
# 9
```

#### pass — do nothing (placeholder)

`pass` is used when Python requires a statement but you don't want to do anything yet. It's often used as a placeholder during development:

```python
for item in range(10):
    if item == 5:
        pass    # TODO: handle this case later
    print(item)
# Prints 0 through 9 — pass has no effect

# Common use: empty function or class placeholder
def process_data():
    pass    # Will implement later

class MyModel:
    pass
```

### else clause on loops

Python uniquely allows an `else` block after `for` and `while` loops. The `else` block runs only if the loop completed **without** hitting a `break`:

```python
# Search for a number
numbers = [1, 3, 5, 7, 9]

for n in numbers:
    if n == 5:
        print("Found 5!")
        break
else:
    print("5 was not found.")
# Output: Found 5!

# When the number is not found
for n in numbers:
    if n == 42:
        print("Found 42!")
        break
else:
    print("42 was not found.")
# Output: 42 was not found.
```

### Nested loops

```python
for i in range(3):
    for j in range(3):
        print(f"({i}, {j})", end="  ")
    print()   # New line after each row

# (0, 0)  (0, 1)  (0, 2)
# (1, 0)  (1, 1)  (1, 2)
# (2, 0)  (2, 1)  (2, 2)
```

### enumerate — get the index alongside the value

Instead of manually tracking an index with a counter variable, use `enumerate`:

```python
fruits = ["apple", "banana", "cherry"]

for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")
# 0: apple
# 1: banana
# 2: cherry

# Start counting from a different number
for index, fruit in enumerate(fruits, start=1):
    print(f"{index}: {fruit}")
# 1: apple
# 2: banana
# 3: cherry
```

### zip — loop over multiple sequences in parallel

```python
names = ["Alice", "Bob", "Charlie"]
scores = [85, 92, 78]

for name, score in zip(names, scores):
    print(f"{name}: {score}")
# Alice: 85
# Bob: 92
# Charlie: 78

# zip stops at the shortest sequence
letters = ["a", "b"]
numbers = [1, 2, 3, 4]
for letter, number in zip(letters, numbers):
    print(letter, number)
# a 1
# b 2
```

---

## 6. Functions

A function is a reusable block of code that performs a specific task. You define it once and call it whenever you need it. Functions help you organize your program into logical, manageable pieces.

### Defining and calling

```python
def greet():
    print("Hello, World!")

greet()   # Hello, World!
greet()   # Hello, World!  — call it as many times as you want
```

### Parameters and arguments

Parameters are the variables listed in the function definition. Arguments are the actual values you pass when calling the function.

```python
def greet(name):             # "name" is a parameter
    print(f"Hello, {name}!")

greet("Alice")               # "Alice" is an argument
# Hello, Alice!
```

### Return values

Functions can send a result back to the caller using `return`. Without `return`, a function returns `None` by default.

```python
def add(a, b):
    return a + b

result = add(3, 5)
print(result)  # 8

# Return multiple values (as a tuple)
def divide(a, b):
    quotient = a // b
    remainder = a % b
    return quotient, remainder

q, r = divide(17, 5)
print(q)  # 3
print(r)  # 2
```

### Default parameters

You can give parameters a default value. If the caller doesn't provide that argument, the default is used:

```python
def greet(name, greeting="Hello"):
    print(f"{greeting}, {name}!")

greet("Alice")                # Hello, Alice!
greet("Alice", "Good morning") # Good morning, Alice!
```

> **Important:** Default parameters with mutable values (like lists or dicts) are a common trap. The default is created once and shared across all calls:
>
> ```python
> # BAD — the same list is reused every call
> def add_item(item, items=[]):
>     items.append(item)
>     return items
>
> print(add_item("a"))  # ["a"]
> print(add_item("b"))  # ["a", "b"]  — unexpected!
>
> # GOOD — use None as default and create a new list inside
> def add_item(item, items=None):
>     if items is None:
>         items = []
>     items.append(item)
>     return items
> ```

### Positional vs keyword arguments

```python
def describe(name, age, city):
    print(f"{name}, {age}, from {city}")

# Positional — order matters
describe("Alice", 30, "London")

# Keyword — order does not matter
describe(city="London", name="Alice", age=30)

# Mix — positional must come first
describe("Alice", city="London", age=30)
```

### *args — variable number of positional arguments

When you don't know how many arguments will be passed, use `*args`. It collects them into a tuple:

```python
def total(*numbers):
    return sum(numbers)

print(total(1, 2, 3))         # 6
print(total(10, 20, 30, 40))  # 100
print(total())                 # 0
```

### **kwargs — variable number of keyword arguments

`**kwargs` collects extra keyword arguments into a dictionary:

```python
def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="Alice", age=30, city="London")
# name: Alice
# age: 30
# city: London
```

### Combining all parameter types

The order must be: positional, `*args`, keyword-only, `**kwargs`:

```python
def example(a, b, *args, option=False, **kwargs):
    print(f"a={a}, b={b}")
    print(f"args={args}")
    print(f"option={option}")
    print(f"kwargs={kwargs}")

example(1, 2, 3, 4, option=True, color="red", size="large")
# a=1, b=2
# args=(3, 4)
# option=True
# kwargs={'color': 'red', 'size': 'large'}
```

### Lambda functions (anonymous functions)

A lambda is a small, unnamed function defined in a single line. It can take any number of arguments but can only contain a single expression:

```python
# Regular function
def square(x):
    return x ** 2

# Equivalent lambda
square = lambda x: x ** 2

print(square(5))  # 25
```

Lambdas are most useful when you need a small function as an argument to another function:

```python
# Sort a list of tuples by the second element
pairs = [(1, "banana"), (3, "apple"), (2, "cherry")]
pairs.sort(key=lambda pair: pair[1])
print(pairs)  # [(3, 'apple'), (1, 'banana'), (2, 'cherry')]

# Filter even numbers
numbers = [1, 2, 3, 4, 5, 6]
evens = list(filter(lambda x: x % 2 == 0, numbers))
print(evens)  # [2, 4, 6]
```

### Scope — where variables live

Variables defined inside a function exist only within that function. This is called **local scope**. Variables defined outside all functions are in **global scope**.

```python
x = 10          # Global variable

def my_function():
    y = 5       # Local variable — only exists inside this function
    print(x)    # Can READ global variables
    print(y)

my_function()
print(x)        # 10
# print(y)      # NameError: y is not defined (y is local to the function)
```

If you need to modify a global variable inside a function (generally discouraged):

```python
counter = 0

def increment():
    global counter
    counter += 1

increment()
print(counter)  # 1
```

### Type hints

Type hints are optional annotations that document what types a function expects and returns. They do not enforce anything at runtime — Python will not raise an error if you pass the wrong type — but they help with readability and tools like linters and IDEs:

```python
def greet(name: str, times: int = 1) -> str:
    return (f"Hello, {name}! ") * times

# With more complex types
from typing import Optional

def find_user(user_id: int) -> Optional[dict]:
    # Returns a dict if found, None otherwise
    ...
```

---

## 7. List Comprehensions

A list comprehension is a compact way to create a list from an existing iterable. It replaces the pattern of creating an empty list, looping, and appending.

### Basic syntax

```python
# Traditional approach
squares = []
for x in range(10):
    squares.append(x ** 2)

# List comprehension — same result, one line
squares = [x ** 2 for x in range(10)]
print(squares)  # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

The pattern is: `[expression for item in iterable]`

### With a condition (filter)

```python
# Only even squares
even_squares = [x ** 2 for x in range(10) if x % 2 == 0]
print(even_squares)  # [0, 4, 16, 36, 64]
```

The pattern is: `[expression for item in iterable if condition]`

### With if/else (transform)

When you need `if/else`, the condition goes **before** the `for`:

```python
labels = ["even" if x % 2 == 0 else "odd" for x in range(5)]
print(labels)  # ['even', 'odd', 'even', 'odd', 'even']
```

### Nested comprehensions

```python
# Flatten a 2D list
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flat = [num for row in matrix for num in row]
print(flat)  # [1, 2, 3, 4, 5, 6, 7, 8, 9]

# Create a 2D grid
grid = [[(i, j) for j in range(3)] for i in range(3)]
print(grid)
# [[(0, 0), (0, 1), (0, 2)],
#  [(1, 0), (1, 1), (1, 2)],
#  [(2, 0), (2, 1), (2, 2)]]
```

### Set and dictionary comprehensions

The same syntax works for sets and dictionaries:

```python
# Set comprehension
unique_lengths = {len(word) for word in ["hello", "world", "hi", "hey"]}
print(unique_lengths)  # {2, 3, 5}

# Dictionary comprehension
word_lengths = {word: len(word) for word in ["hello", "world", "hi"]}
print(word_lengths)  # {'hello': 5, 'world': 5, 'hi': 2}
```

### Generator expressions

If you replace the square brackets with parentheses, you get a **generator expression**. It produces values one at a time instead of building the entire list in memory — useful for very large sequences:

```python
# List comprehension — builds entire list in memory
total = sum([x ** 2 for x in range(1_000_000)])

# Generator expression — produces values on-the-fly, much less memory
total = sum(x ** 2 for x in range(1_000_000))
```

---

## 8. Slicing

Slicing lets you extract a portion of a sequence — a list, tuple, or string. The syntax is:

```
sequence[start:stop:step]
```

- `start` — the index where the slice begins (inclusive). Default is `0`.
- `stop` — the index where the slice ends (exclusive). Default is the end.
- `step` — how many items to skip. Default is `1`.

### Basic slicing

```python
letters = ["a", "b", "c", "d", "e", "f", "g"]
#           0    1    2    3    4    5    6
#          -7   -6   -5   -4   -3   -2   -1

letters[2:5]      # ["c", "d", "e"]       (index 2, 3, 4)
letters[:3]       # ["a", "b", "c"]       (from the start up to index 3)
letters[4:]       # ["e", "f", "g"]       (from index 4 to the end)
letters[:]        # ["a", "b", "c", "d", "e", "f", "g"]  (full copy)
```

### Negative indices

Negative indices count from the end:

```python
letters = ["a", "b", "c", "d", "e", "f", "g"]

letters[-3:]      # ["e", "f", "g"]       (last 3 items)
letters[:-2]      # ["a", "b", "c", "d", "e"]  (everything except last 2)
letters[-4:-1]    # ["d", "e", "f"]       (from 4th-to-last up to, but not including, last)
```

### Step

```python
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

numbers[::2]      # [0, 2, 4, 6, 8]       (every 2nd item)
numbers[1::2]     # [1, 3, 5, 7, 9]       (every 2nd item, starting from index 1)
numbers[::3]      # [0, 3, 6, 9]          (every 3rd item)
numbers[::-1]     # [9, 8, 7, 6, 5, 4, 3, 2, 1, 0]  (reversed)
numbers[::-2]     # [9, 7, 5, 3, 1]       (reversed, every 2nd)
```

### Slicing strings

Strings support the same slicing syntax:

```python
text = "Hello, World!"

text[7:12]     # "World"
text[:5]       # "Hello"
text[-6:-1]    # "World"
text[::-1]     # "!dlroW ,olleH"   (reversed string)
```

### Slice assignment (lists only)

You can replace a slice of a list with new values:

```python
numbers = [0, 1, 2, 3, 4, 5]

# Replace a range
numbers[2:4] = [20, 30]
print(numbers)  # [0, 1, 20, 30, 4, 5]

# Insert without removing (empty slice)
numbers[2:2] = [99, 98]
print(numbers)  # [0, 1, 99, 98, 20, 30, 4, 5]

# Delete a range
numbers[2:4] = []
print(numbers)  # [0, 1, 20, 30, 4, 5]
```

---

## 9. Useful Built-in Functions

Python comes with many built-in functions that require no imports. Here are the ones you'll use most often.

### Working with numbers

```python
abs(-7)             # 7          (absolute value)
round(3.14159, 2)   # 3.14       (round to 2 decimal places)
round(2.5)          # 2          (banker's rounding — rounds to nearest even)
min(3, 1, 4, 1, 5)  # 1
max(3, 1, 4, 1, 5)  # 5
sum([1, 2, 3, 4])   # 10
pow(2, 10)          # 1024       (same as 2 ** 10)
divmod(17, 5)       # (3, 2)     (quotient and remainder)
```

### Working with iterables

```python
# len — number of items
len([1, 2, 3])         # 3
len("hello")           # 5
len({"a": 1, "b": 2})  # 2

# sorted — returns a new sorted list
sorted([3, 1, 4, 1, 5])                    # [1, 1, 3, 4, 5]
sorted([3, 1, 4], reverse=True)             # [4, 3, 1]
sorted(["banana", "apple", "cherry"])       # ["apple", "banana", "cherry"]
sorted(["banana", "apple"], key=len)        # ["apple", "banana"]

# reversed — returns an iterator in reverse order
list(reversed([1, 2, 3]))    # [3, 2, 1]

# enumerate — pairs each item with its index (see Loops section)
list(enumerate(["a", "b", "c"]))  # [(0, "a"), (1, "b"), (2, "c")]

# zip — pairs items from multiple iterables (see Loops section)
list(zip([1, 2], ["a", "b"]))    # [(1, "a"), (2, "b")]

# map — applies a function to every item
list(map(str.upper, ["hello", "world"]))    # ["HELLO", "WORLD"]
list(map(lambda x: x * 2, [1, 2, 3]))      # [2, 4, 6]

# filter — keeps items where the function returns True
list(filter(lambda x: x > 0, [-2, -1, 0, 1, 2]))  # [1, 2]

# any — True if at least one item is truthy
any([False, False, True])    # True
any([0, 0, 0])               # False

# all — True if every item is truthy
all([True, True, True])      # True
all([True, False, True])     # False
all([1, 2, 3])               # True
```

### Type and conversion functions

```python
type(42)             # <class 'int'>
isinstance(42, int)  # True
isinstance("hi", (str, int))  # True (checks multiple types)

int("10")            # 10
float("3.14")        # 3.14
str(100)             # "100"
bool(0)              # False
list((1, 2, 3))      # [1, 2, 3]   (tuple → list)
tuple([1, 2, 3])     # (1, 2, 3)   (list → tuple)
set([1, 2, 2, 3])    # {1, 2, 3}   (list → set, removes duplicates)
dict([("a", 1), ("b", 2)])  # {"a": 1, "b": 2}
```

### Other handy built-ins

```python
# range — generates a sequence of numbers
list(range(5))           # [0, 1, 2, 3, 4]
list(range(2, 8))        # [2, 3, 4, 5, 6, 7]
list(range(0, 10, 3))    # [0, 3, 6, 9]

# input — read text from the user
name = input("What is your name? ")

# print — output to the console
print("Hello", "World", sep=", ")     # Hello, World
print("No newline", end="")           # Stays on the same line
print("Loading", end="...\n")         # Loading...

# id — unique identifier of an object in memory
x = [1, 2, 3]
print(id(x))    # Something like 140234866534400

# help — interactive help on any object or function
help(len)
help(str.split)

# dir — list all attributes and methods of an object
dir([])          # Shows all list methods
dir(str)         # Shows all string methods
```

---

## 10. Working with Files

Python's built-in `open()` function lets you read from and write to files. The recommended approach is to use the `with` statement, which automatically closes the file when the block ends — even if an error occurs.

### The with statement and open()

```python
with open("example.txt", "r") as file:
    content = file.read()
    print(content)
# The file is automatically closed here, even if an error happened inside the block.
```

Without `with`, you would need to manually close the file:

```python
file = open("example.txt", "r")
content = file.read()
file.close()   # Easy to forget, especially if an error occurs before this line
```

> **Always use `with`** — it is safer and cleaner.

### File modes

The second argument to `open()` controls what you can do with the file:

| Mode | Description |
|---|---|
| `"r"` | Read (default). File must exist. |
| `"w"` | Write. Creates the file if it doesn't exist. **Overwrites** everything if it does. |
| `"a"` | Append. Creates the file if it doesn't exist. Adds to the end without erasing. |
| `"x"` | Exclusive creation. Creates a new file, raises an error if it already exists. |
| `"r+"` | Read and write. File must exist. |
| `"b"` | Binary mode. Add to other modes, e.g., `"rb"` for reading binary files (images, PDFs). |

### Reading files

```python
# Read the entire file as a single string
with open("data.txt", "r") as f:
    content = f.read()
    print(content)

# Read all lines into a list (each line is a string, including the newline character)
with open("data.txt", "r") as f:
    lines = f.readlines()
    print(lines)     # ["line one\n", "line two\n", "line three\n"]

# Read one line at a time
with open("data.txt", "r") as f:
    first_line = f.readline()    # "line one\n"
    second_line = f.readline()   # "line two\n"

# Loop through the file line by line (most memory-efficient for large files)
with open("data.txt", "r") as f:
    for line in f:
        print(line.strip())    # .strip() removes the trailing newline
```

### Writing files

```python
# Write (creates or overwrites)
with open("output.txt", "w") as f:
    f.write("Hello, World!\n")
    f.write("Second line.\n")

# Write multiple lines at once
lines = ["Line 1\n", "Line 2\n", "Line 3\n"]
with open("output.txt", "w") as f:
    f.writelines(lines)

# Append to an existing file
with open("log.txt", "a") as f:
    f.write("New log entry.\n")
```

### Working with CSV files

CSV (Comma-Separated Values) is one of the most common file formats in data science. Python has a built-in `csv` module:

```python
import csv

# Writing a CSV file
with open("people.csv", "w", newline="") as f:
    writer = csv.writer(f)
    writer.writerow(["name", "age", "city"])       # Header
    writer.writerow(["Alice", 30, "London"])
    writer.writerow(["Bob", 25, "Paris"])

# Reading a CSV file
with open("people.csv", "r") as f:
    reader = csv.reader(f)
    header = next(reader)           # Read the header row
    for row in reader:
        print(row)                  # Each row is a list of strings
# ['Alice', '30', 'London']
# ['Bob', '25', 'Paris']

# Reading into dictionaries (keys are column headers)
with open("people.csv", "r") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row["name"], row["age"])
# Alice 30
# Bob 25
```

### Working with JSON files

```python
import json

# Write Python data to a JSON file
data = {"name": "Alice", "scores": [85, 92, 78]}

with open("data.json", "w") as f:
    json.dump(data, f, indent=4)     # indent makes it human-readable

# Read JSON data from a file
with open("data.json", "r") as f:
    loaded = json.load(f)
    print(loaded["name"])     # Alice
    print(loaded["scores"])   # [85, 92, 78]
```

### Checking if a file exists

```python
from pathlib import Path

# Modern approach using pathlib
path = Path("data.txt")
if path.exists():
    print("File exists.")
if path.is_file():
    print("It's a file.")
if path.is_dir():
    print("It's a directory.")

# Older approach using os
import os
os.path.exists("data.txt")    # True or False
os.path.isfile("data.txt")    # True or False
```

### Common pathlib operations

```python
from pathlib import Path

# Build paths in a cross-platform way
data_dir = Path("data") / "raw" / "file.csv"
print(data_dir)          # data/raw/file.csv

# Get parts of a path
p = Path("/home/user/data/report.csv")
p.name       # "report.csv"
p.stem       # "report"        (filename without extension)
p.suffix     # ".csv"
p.parent     # Path("/home/user/data")

# List all CSV files in a directory
for csv_file in Path("data").glob("*.csv"):
    print(csv_file)

# Recursively find all Python files
for py_file in Path(".").rglob("*.py"):
    print(py_file)
```

---

## 11. Error Handling

When something goes wrong during execution, Python raises an **exception**. If you don't handle it, your program crashes. Error handling lets you catch exceptions and decide what to do instead.

### try / except

```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero!")
# Output: Cannot divide by zero!
```

### Catching specific exceptions

Always catch specific exceptions rather than a generic `except`. This way you know exactly what went wrong:

```python
try:
    number = int("not a number")
except ValueError:
    print("That's not a valid integer.")

try:
    data = {"name": "Alice"}
    print(data["age"])
except KeyError:
    print("Key not found in dictionary.")

try:
    items = [1, 2, 3]
    print(items[10])
except IndexError:
    print("Index out of range.")
```

### Catching multiple exceptions

```python
# Separate handlers for different exceptions
try:
    value = int(input("Enter a number: "))
    result = 100 / value
except ValueError:
    print("That's not a number.")
except ZeroDivisionError:
    print("Cannot divide by zero.")

# Same handler for multiple exceptions
try:
    # some code
    pass
except (ValueError, TypeError) as e:
    print(f"Error: {e}")
```

### else and finally

```python
try:
    number = int("42")
except ValueError:
    print("Not a valid number.")
else:
    # Runs only if NO exception occurred
    print(f"Successfully converted: {number}")
finally:
    # Runs ALWAYS, whether an exception occurred or not
    # Useful for cleanup (closing connections, releasing resources)
    print("Done.")

# Output:
# Successfully converted: 42
# Done.
```

### Common exception types

| Exception | When it happens |
|---|---|
| `ValueError` | Right type, wrong value: `int("abc")` |
| `TypeError` | Wrong type: `"2" + 2` |
| `KeyError` | Dictionary key not found: `d["missing"]` |
| `IndexError` | List index out of range: `[1,2][5]` |
| `FileNotFoundError` | File doesn't exist: `open("nope.txt")` |
| `ZeroDivisionError` | Division by zero: `1 / 0` |
| `AttributeError` | Object doesn't have that attribute: `5.append(1)` |
| `ImportError` | Module not found: `import nonexistent` |
| `NameError` | Variable not defined: `print(undefined_var)` |
| `StopIteration` | Iterator has no more items |
| `PermissionError` | No permission to access a file or resource |
| `OSError` | General operating system error |

### Raising exceptions

You can intentionally raise exceptions in your own code to signal that something went wrong:

```python
def set_age(age):
    if age < 0:
        raise ValueError("Age cannot be negative.")
    if not isinstance(age, int):
        raise TypeError("Age must be an integer.")
    return age

try:
    set_age(-5)
except ValueError as e:
    print(e)    # Age cannot be negative.
```

### Creating custom exceptions

```python
class InsufficientFundsError(Exception):
    def __init__(self, balance, amount):
        self.balance = balance
        self.amount = amount
        super().__init__(f"Cannot withdraw {amount}. Balance is only {balance}.")

def withdraw(balance, amount):
    if amount > balance:
        raise InsufficientFundsError(balance, amount)
    return balance - amount

try:
    withdraw(100, 250)
except InsufficientFundsError as e:
    print(e)            # Cannot withdraw 250. Balance is only 100.
    print(e.balance)    # 100
    print(e.amount)     # 250
```

---

## 12. Modules and Imports

A module is simply a Python file (`.py`) that contains code — functions, classes, variables — that you can reuse in other files. A package is a folder of modules.

### Importing modules

```python
# Import the entire module
import math
print(math.sqrt(16))    # 4.0
print(math.pi)          # 3.141592653589793

# Import specific things
from math import sqrt, pi
print(sqrt(16))          # 4.0
print(pi)                # 3.141592653589793

# Import with an alias
import numpy as np
import pandas as pd

# Import everything (generally discouraged — pollutes namespace)
from math import *
```

### Creating your own modules

Any Python file is a module. If you have `helpers.py`:

```python
# helpers.py
def clean_text(text):
    return text.strip().lower()

PI = 3.14159
```

You can import it from another file in the same directory:

```python
# main.py
from helpers import clean_text, PI

result = clean_text("  HELLO  ")
print(result)  # "hello"
print(PI)      # 3.14159
```

### The if __name__ == "__main__" pattern

When Python runs a file directly, the special variable `__name__` is set to `"__main__"`. When the file is imported as a module, `__name__` is set to the module's name instead. This lets you write code that only runs when the file is executed directly:

```python
# my_module.py
def greet(name):
    return f"Hello, {name}!"

if __name__ == "__main__":
    # This block only runs when you execute: python my_module.py
    # It does NOT run when someone does: import my_module
    print(greet("World"))
```

### Useful standard library modules

| Module | Purpose |
|---|---|
| `os` | Operating system interaction (paths, environment variables) |
| `sys` | System-specific parameters and functions |
| `pathlib` | Object-oriented filesystem paths |
| `math` | Mathematical functions (`sqrt`, `log`, `sin`, `pi`) |
| `random` | Generate random numbers and make random choices |
| `datetime` | Work with dates and times |
| `json` | Encode and decode JSON data |
| `csv` | Read and write CSV files |
| `re` | Regular expressions (pattern matching in strings) |
| `collections` | Specialized container types (`Counter`, `defaultdict`, `deque`) |
| `itertools` | Tools for efficient looping |
| `functools` | Higher-order functions and operations on callables |
| `copy` | Shallow and deep copying of objects |

### Quick examples of common modules

```python
# random
import random
random.random()              # Random float between 0 and 1
random.randint(1, 100)       # Random integer between 1 and 100
random.choice(["a", "b", "c"])   # Random pick from a list
random.shuffle([1, 2, 3, 4])    # Shuffle a list in place
random.sample(range(100), 5)    # 5 unique random items from range

# datetime
from datetime import datetime, timedelta
now = datetime.now()
print(now.strftime("%Y-%m-%d %H:%M"))    # "2026-04-07 14:30"

yesterday = now - timedelta(days=1)
future = now + timedelta(hours=5, minutes=30)

# collections.Counter
from collections import Counter
words = ["apple", "banana", "apple", "cherry", "banana", "apple"]
count = Counter(words)
print(count)                     # Counter({'apple': 3, 'banana': 2, 'cherry': 1})
print(count.most_common(2))      # [('apple', 3), ('banana', 2)]

# collections.defaultdict
from collections import defaultdict
grouped = defaultdict(list)
for name, score in [("Alice", 85), ("Bob", 90), ("Alice", 92)]:
    grouped[name].append(score)
print(dict(grouped))     # {'Alice': [85, 92], 'Bob': [90]}
```

---

## 13. Classes

A class is a blueprint for creating objects. Objects bundle data (attributes) and behavior (methods) together. This is the core of **object-oriented programming (OOP)**.

Think of a class like a cookie cutter: you define the shape once, then stamp out as many cookies (objects) as you want.

### Defining a class

```python
class Dog:
    # The __init__ method runs automatically when you create a new object.
    # "self" refers to the specific object being created.
    def __init__(self, name, breed, age):
        self.name = name       # Instance attribute
        self.breed = breed
        self.age = age

    # A method — a function that belongs to the class
    def bark(self):
        return f"{self.name} says Woof!"

    def describe(self):
        return f"{self.name} is a {self.age}-year-old {self.breed}."
```

### Creating objects (instances)

```python
dog1 = Dog("Rex", "German Shepherd", 5)
dog2 = Dog("Bella", "Labrador", 3)

print(dog1.name)          # Rex
print(dog2.bark())        # Bella says Woof!
print(dog1.describe())    # Rex is a 5-year-old German Shepherd.
```

### Instance vs class attributes

```python
class Dog:
    species = "Canis familiaris"    # Class attribute — shared by ALL instances

    def __init__(self, name):
        self.name = name            # Instance attribute — unique to each object

dog1 = Dog("Rex")
dog2 = Dog("Bella")

print(dog1.species)       # Canis familiaris
print(dog2.species)       # Canis familiaris (same for all dogs)
print(dog1.name)          # Rex
print(dog2.name)          # Bella (different for each dog)
```

### String representation (__str__ and __repr__)

When you print an object, Python uses the `__str__` method. When you type the object's name in the console, Python uses `__repr__`. If `__str__` is not defined, `__repr__` is used as a fallback.

```python
class Dog:
    def __init__(self, name, breed):
        self.name = name
        self.breed = breed

    def __str__(self):
        return f"{self.name} ({self.breed})"

    def __repr__(self):
        return f"Dog(name='{self.name}', breed='{self.breed}')"

dog = Dog("Rex", "German Shepherd")
print(dog)        # Rex (German Shepherd)        — uses __str__
print(repr(dog))  # Dog(name='Rex', breed='German Shepherd')  — uses __repr__
```

### Inheritance

Inheritance lets you create a new class based on an existing one. The new class (child) inherits all attributes and methods from the parent class and can add or override them.

```python
class Animal:
    def __init__(self, name, sound):
        self.name = name
        self.sound = sound

    def speak(self):
        return f"{self.name} says {self.sound}!"

class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name, sound="Woof")   # Call the parent's __init__
        self.breed = breed

    def fetch(self):
        return f"{self.name} fetches the ball!"

class Cat(Animal):
    def __init__(self, name):
        super().__init__(name, sound="Meow")

dog = Dog("Rex", "Labrador")
cat = Cat("Whiskers")

print(dog.speak())     # Rex says Woof!
print(dog.fetch())     # Rex fetches the ball!
print(cat.speak())     # Whiskers says Meow!
```

### Method overriding

A child class can redefine a method from the parent:

```python
class Animal:
    def speak(self):
        return "Some generic sound."

class Dog(Animal):
    def speak(self):        # Override the parent's speak method
        return "Woof!"

class Cat(Animal):
    def speak(self):
        return "Meow!"

for animal in [Dog(), Cat(), Animal()]:
    print(animal.speak())
# Woof!
# Meow!
# Some generic sound.
```

### Properties — controlled access to attributes

Properties let you define getter and setter methods that look like regular attribute access:

```python
class Circle:
    def __init__(self, radius):
        self._radius = radius     # Convention: underscore means "private-ish"

    @property
    def radius(self):
        return self._radius

    @radius.setter
    def radius(self, value):
        if value < 0:
            raise ValueError("Radius cannot be negative.")
        self._radius = value

    @property
    def area(self):
        return 3.14159 * self._radius ** 2

circle = Circle(5)
print(circle.radius)     # 5        — uses the getter
print(circle.area)       # 78.53975 — computed on the fly

circle.radius = 10       # Uses the setter
print(circle.area)       # 314.159

# circle.radius = -1     # Raises ValueError: Radius cannot be negative.
```

### Static methods and class methods

```python
class MathHelper:
    multiplier = 2     # Class attribute

    @staticmethod
    def add(a, b):
        # Does not access the instance (self) or the class (cls)
        return a + b

    @classmethod
    def scaled_add(cls, a, b):
        # Accesses the class through "cls"
        return cls.multiplier * (a + b)

print(MathHelper.add(3, 5))          # 8
print(MathHelper.scaled_add(3, 5))   # 16
```

- **`@staticmethod`**: A utility function that lives inside the class for organizational purposes but doesn't need access to the instance or the class.
- **`@classmethod`**: A method that receives the class itself as the first argument (`cls`), which is useful for factory methods or accessing class-level attributes.

### Dunder (magic) methods

Python uses special methods with double underscores (called "dunder" methods) to define how objects behave with operators and built-in functions:

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):          # v1 + v2
        return Vector(self.x + other.x, self.y + other.y)

    def __sub__(self, other):          # v1 - v2
        return Vector(self.x - other.x, self.y - other.y)

    def __eq__(self, other):           # v1 == v2
        return self.x == other.x and self.y == other.y

    def __len__(self):                 # len(v)
        return int((self.x ** 2 + self.y ** 2) ** 0.5)

    def __repr__(self):
        return f"Vector({self.x}, {self.y})"

v1 = Vector(1, 2)
v2 = Vector(3, 4)
print(v1 + v2)      # Vector(4, 6)
print(v1 == v2)     # False
print(len(v2))      # 5
```

Common dunder methods:

| Method | Triggered by |
|---|---|
| `__init__` | Object creation: `MyClass()` |
| `__str__` | `print(obj)`, `str(obj)` |
| `__repr__` | `repr(obj)`, console display |
| `__len__` | `len(obj)` |
| `__getitem__` | `obj[key]` |
| `__setitem__` | `obj[key] = value` |
| `__contains__` | `item in obj` |
| `__eq__` | `obj1 == obj2` |
| `__lt__` | `obj1 < obj2` |
| `__add__` | `obj1 + obj2` |
| `__iter__` | `for item in obj` |
| `__call__` | `obj()` (calling the object like a function) |
| `__enter__`, `__exit__` | `with obj as x:` (context manager) |

---

## 14. Dataclasses

Dataclasses (introduced in Python 3.7) are a shortcut for creating classes that mainly hold data. They automatically generate `__init__`, `__repr__`, `__eq__`, and other methods for you, so you write much less boilerplate.

### The problem they solve

Without dataclasses, a simple data-holding class requires a lot of repetitive code:

```python
# Without dataclasses — lots of boilerplate
class Person:
    def __init__(self, name, age, city):
        self.name = name
        self.age = age
        self.city = city

    def __repr__(self):
        return f"Person(name='{self.name}', age={self.age}, city='{self.city}')"

    def __eq__(self, other):
        return (self.name == other.name and
                self.age == other.age and
                self.city == other.city)
```

### With dataclasses

```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    age: int
    city: str

# That's it! __init__, __repr__, and __eq__ are generated automatically.

alice = Person("Alice", 30, "London")
bob = Person("Bob", 25, "Paris")

print(alice)            # Person(name='Alice', age=30, city='London')
print(alice == bob)     # False
print(alice.name)       # Alice
```

### Default values

```python
from dataclasses import dataclass

@dataclass
class Config:
    host: str = "localhost"
    port: int = 8080
    debug: bool = False

config1 = Config()                          # All defaults
config2 = Config(host="0.0.0.0", debug=True)

print(config1)  # Config(host='localhost', port=8080, debug=False)
print(config2)  # Config(host='0.0.0.0', port=8080, debug=True)
```

### Default factory for mutable defaults

Just like with regular functions, mutable default values (lists, dicts) are dangerous. Use `field(default_factory=...)`:

```python
from dataclasses import dataclass, field

@dataclass
class Student:
    name: str
    grades: list = field(default_factory=list)    # Each instance gets its own list

s1 = Student("Alice")
s2 = Student("Bob")

s1.grades.append(90)
print(s1.grades)   # [90]
print(s2.grades)   # []   — not affected by s1
```

### Frozen dataclasses (immutable)

Adding `frozen=True` makes instances immutable — you cannot change their attributes after creation:

```python
from dataclasses import dataclass

@dataclass(frozen=True)
class Point:
    x: float
    y: float

p = Point(1.0, 2.0)
# p.x = 5.0    # Raises FrozenInstanceError

# Frozen dataclasses can be used as dictionary keys or in sets
points = {Point(0, 0), Point(1, 1), Point(0, 0)}
print(points)   # {Point(x=0, y=0), Point(x=1, y=1)}  — duplicate removed
```

### Post-initialization processing

If you need to compute something after the object is created, use `__post_init__`:

```python
from dataclasses import dataclass

@dataclass
class Rectangle:
    width: float
    height: float
    area: float = field(init=False)    # Not included in __init__

    def __post_init__(self):
        self.area = self.width * self.height

r = Rectangle(5, 10)
print(r)        # Rectangle(width=5, height=10, area=50)
print(r.area)   # 50
```

### Ordering

Add `order=True` to automatically generate comparison methods (`<`, `<=`, `>`, `>=`):

```python
from dataclasses import dataclass

@dataclass(order=True)
class Student:
    gpa: float
    name: str    # Compared after gpa (field order matters)

students = [Student(3.5, "Alice"), Student(3.9, "Bob"), Student(3.5, "Charlie")]
print(sorted(students))
# [Student(gpa=3.5, name='Alice'), Student(gpa=3.5, name='Charlie'), Student(gpa=3.9, name='Bob')]
```

---

## 15. Decorators

A decorator is a function that takes another function and extends or modifies its behavior without changing the original function's code. Decorators are one of Python's most elegant features.

### Functions are objects

To understand decorators, you first need to know that functions in Python are just objects. You can assign them to variables, pass them as arguments, and return them from other functions:

```python
def greet(name):
    return f"Hello, {name}!"

# Assign a function to a variable
say_hello = greet
print(say_hello("Alice"))   # Hello, Alice!

# Pass a function as an argument
def call_twice(func, arg):
    print(func(arg))
    print(func(arg))

call_twice(greet, "Bob")
# Hello, Bob!
# Hello, Bob!
```

### A decorator step by step

A decorator is a function that:
1. Takes a function as input.
2. Defines a new inner function (wrapper) that adds behavior.
3. Returns the wrapper.

```python
def my_decorator(func):
    def wrapper():
        print("Something before the function.")
        func()
        print("Something after the function.")
    return wrapper

def say_hello():
    print("Hello!")

# Apply the decorator manually
decorated = my_decorator(say_hello)
decorated()
# Something before the function.
# Hello!
# Something after the function.
```

### The @ syntax

Using `@decorator_name` above a function definition is shorthand for `func = decorator(func)`:

```python
def my_decorator(func):
    def wrapper():
        print("Before.")
        func()
        print("After.")
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

say_hello()
# Before.
# Hello!
# After.
```

### Decorating functions with arguments

To make decorators work with any function (regardless of its parameters), use `*args` and `**kwargs`:

```python
import functools

def log_call(func):
    @functools.wraps(func)    # Preserves the original function's name and docstring
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__} with args={args}, kwargs={kwargs}")
        result = func(*args, **kwargs)
        print(f"{func.__name__} returned {result}")
        return result
    return wrapper

@log_call
def add(a, b):
    return a + b

@log_call
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

add(3, 5)
# Calling add with args=(3, 5), kwargs={}
# add returned 8

greet("Alice", greeting="Hi")
# Calling greet with args=('Alice',), kwargs={'greeting': 'Hi'}
# greet returned Hi, Alice!
```

> **Important:** Always use `@functools.wraps(func)` in your wrapper. Without it, the decorated function loses its original name, docstring, and other metadata.

### Practical decorator examples

#### Timing a function

```python
import functools
import time

def timer(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        elapsed = time.time() - start
        print(f"{func.__name__} took {elapsed:.4f} seconds")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(1)
    return "done"

slow_function()
# slow_function took 1.0012 seconds
```

#### Retry on failure

```python
import functools
import time

def retry(max_attempts=3, delay=1):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(1, max_attempts + 1):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    print(f"Attempt {attempt} failed: {e}")
                    if attempt < max_attempts:
                        time.sleep(delay)
            raise Exception(f"All {max_attempts} attempts failed.")
        return wrapper
    return decorator

@retry(max_attempts=3, delay=0.5)
def unreliable_api_call():
    import random
    if random.random() < 0.7:
        raise ConnectionError("Server unavailable")
    return "Success!"
```

#### Caching results (memoization)

```python
import functools

def memoize(func):
    cache = {}
    @functools.wraps(func)
    def wrapper(*args):
        if args in cache:
            print(f"Cache hit for {args}")
            return cache[args]
        result = func(*args)
        cache[args] = result
        return result
    return wrapper

@memoize
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

print(fibonacci(10))   # 55 (only computes each value once)
```

> **Tip:** Python has a built-in decorator for this: `@functools.lru_cache`. Use it instead of writing your own for production code:
>
> ```python
> @functools.lru_cache(maxsize=128)
> def fibonacci(n):
>     if n < 2:
>         return n
>     return fibonacci(n - 1) + fibonacci(n - 2)
> ```

### Stacking decorators

You can apply multiple decorators to a single function. They are applied bottom-up (closest to the function first):

```python
@timer
@log_call
def compute(x, y):
    return x ** y

# Equivalent to: compute = timer(log_call(compute))
```

---

## 16. Logging

When your code is small, `print()` is fine for debugging. But as projects grow, you need something better — a way to record what your program is doing, at different levels of detail, with timestamps and context, and without littering your code with print statements you'll later have to remove. That's what the `logging` module is for.

Python's `logging` module is part of the standard library — no installation needed.

### Why not just use print()?

| Feature | `print()` | `logging` |
|---|---|---|
| Severity levels (debug, info, warning, error) | No | Yes |
| Timestamps | Manual | Built-in |
| Output to files | Manual | Built-in |
| Easy to turn on/off | No (must delete/comment out) | Yes (change level) |
| Production-ready | No | Yes |

### Basic usage

```python
import logging

logging.warning("This is a warning.")     # WARNING:root:This is a warning.
logging.info("This is informational.")    # (nothing — default level is WARNING)
```

By default, only messages at level `WARNING` and above are shown. Messages below that threshold are silently ignored.

### Log levels

Log levels represent how serious a message is. From lowest to highest:

| Level | Numeric value | When to use |
|---|---|---|
| `DEBUG` | 10 | Detailed diagnostic info — only needed when hunting bugs |
| `INFO` | 20 | Confirmation that things are working as expected |
| `WARNING` | 30 | Something unexpected happened, but the program still works |
| `ERROR` | 40 | A serious problem — some functionality failed |
| `CRITICAL` | 50 | A very serious error — the program may not be able to continue |

```python
import logging

logging.debug("Variable x = 42")         # Diagnostic detail
logging.info("Server started.")           # Normal operation
logging.warning("Disk space low.")        # Something to watch
logging.error("Failed to save file.")     # Something broke
logging.critical("Database is down!")     # System-level failure
```

### Configuring the log level

Use `basicConfig()` to set the minimum level that gets displayed. Call it **once, at the start of your program**, before any logging calls:

```python
import logging

logging.basicConfig(level=logging.DEBUG)

logging.debug("Now this will appear.")     # DEBUG:root:Now this will appear.
logging.info("And so will this.")          # INFO:root:And so will this.
logging.warning("And this too.")           # WARNING:root:And this too.
```

Set it to `logging.INFO` if you want to see informational messages but not debug noise. Set it to `logging.WARNING` (the default) for quieter output.

### Formatting log messages

You can customize what each log line looks like with the `format` parameter:

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s"
)

logging.info("Server started on port 8080.")
# 2026-04-07 14:30:00,123 - INFO - Server started on port 8080.
```

Common format placeholders:

| Placeholder | Description | Example |
|---|---|---|
| `%(asctime)s` | Timestamp | `2026-04-07 14:30:00,123` |
| `%(levelname)s` | Level name | `INFO`, `ERROR` |
| `%(message)s` | The log message | `Server started.` |
| `%(name)s` | Logger name | `root`, `my_module` |
| `%(filename)s` | Source file name | `app.py` |
| `%(funcName)s` | Function name | `process_data` |
| `%(lineno)d` | Line number | `42` |

You can also customize the date format:

```python
logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s [%(levelname)s] %(message)s",
    datefmt="%Y-%m-%d %H:%M:%S"
)
```

### Logging to a file

Instead of (or in addition to) printing to the console, you can write logs to a file:

```python
import logging

logging.basicConfig(
    filename="app.log",
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s"
)

logging.info("Application started.")    # Written to app.log, not the console
logging.error("Something went wrong.")  # Also written to app.log
```

To log to **both** the console and a file, you need handlers (covered below).

### Named loggers

In larger projects, don't use the root logger (`logging.info(...)`) directly. Create a **named logger** for each module so you can see exactly where each message came from:

```python
import logging

logger = logging.getLogger(__name__)    # __name__ is the module's name (e.g., "my_module")

logger.info("This message is tagged with the module name.")
```

When you see `__name__` in log output, you immediately know which file generated the message. This is the recommended approach for any project with more than one file.

### Handlers — controlling where logs go

A **handler** determines where log messages are sent. You can have multiple handlers on a single logger — for example, one that prints to the console and another that writes to a file.

```python
import logging

# Create a named logger
logger = logging.getLogger("my_app")
logger.setLevel(logging.DEBUG)    # Set the logger's own level

# Handler 1: Console output
console_handler = logging.StreamHandler()
console_handler.setLevel(logging.INFO)    # Only INFO and above go to console

# Handler 2: File output
file_handler = logging.FileHandler("app.log")
file_handler.setLevel(logging.DEBUG)       # Everything goes to the file

# Create a formatter and attach it to both handlers
formatter = logging.Formatter("%(asctime)s - %(name)s - %(levelname)s - %(message)s")
console_handler.setFormatter(formatter)
file_handler.setFormatter(formatter)

# Add handlers to the logger
logger.addHandler(console_handler)
logger.addHandler(file_handler)

# Now use the logger
logger.debug("Debug details.")    # Only in app.log
logger.info("Server started.")    # Console AND app.log
logger.error("Disk is full.")     # Console AND app.log
```

### Logging exceptions

When catching exceptions, use `logger.exception()` or pass `exc_info=True` to include the full traceback in the log:

```python
import logging

logger = logging.getLogger(__name__)

try:
    result = 10 / 0
except ZeroDivisionError:
    logger.exception("Division failed")    # Logs the message + full traceback at ERROR level

# Alternatively:
try:
    result = 10 / 0
except ZeroDivisionError:
    logger.error("Division failed", exc_info=True)   # Same result
```

Output:

```
ERROR:__main__:Division failed
Traceback (most recent call last):
  File "app.py", line 7, in <module>
    result = 10 / 0
ZeroDivisionError: division by zero
```

### Logging variables

Use the `%` style or f-strings to include variable values in log messages:

```python
import logging

logger = logging.getLogger(__name__)

user = "Alice"
action = "login"

# %-style (traditional, slightly more efficient — string is only formatted if the message is actually logged)
logger.info("User %s performed %s", user, action)

# f-string style (always formats, even if the level is too low to display)
logger.info(f"User {user} performed {action}")
```

> **Tip:** The `%`-style is preferred in performance-critical code because the string is only formatted if the message actually passes the log level filter. With f-strings, the string is always built, even if the message is ultimately discarded.

### Rotating log files

In long-running applications, log files can grow very large. Use `RotatingFileHandler` to automatically rotate to a new file when the current one gets too big:

```python
import logging
from logging.handlers import RotatingFileHandler

logger = logging.getLogger("my_app")
logger.setLevel(logging.INFO)

handler = RotatingFileHandler(
    "app.log",
    maxBytes=5_000_000,    # 5 MB per file
    backupCount=3           # Keep 3 old files: app.log.1, app.log.2, app.log.3
)
handler.setFormatter(logging.Formatter("%(asctime)s - %(levelname)s - %(message)s"))
logger.addHandler(handler)
```

When `app.log` reaches 5 MB, it gets renamed to `app.log.1`, the previous `app.log.1` becomes `app.log.2`, and a new empty `app.log` is created. The oldest file is deleted when `backupCount` is exceeded.

### A practical logging setup

Here is a complete, production-ready logging configuration you can use as a starting point for any project:

```python
import logging
import sys

def setup_logging(level=logging.INFO):
    """Configure logging with console and file output."""
    logger = logging.getLogger()
    logger.setLevel(level)

    formatter = logging.Formatter(
        "%(asctime)s [%(levelname)-8s] %(name)s: %(message)s",
        datefmt="%Y-%m-%d %H:%M:%S"
    )

    # Console handler
    console = logging.StreamHandler(sys.stdout)
    console.setLevel(level)
    console.setFormatter(formatter)
    logger.addHandler(console)

    # File handler
    file_handler = logging.FileHandler("app.log")
    file_handler.setLevel(logging.DEBUG)    # File gets everything
    file_handler.setFormatter(formatter)
    logger.addHandler(file_handler)

    return logger

# In your main script:
if __name__ == "__main__":
    setup_logging()
    logger = logging.getLogger(__name__)

    logger.info("Application started.")
    logger.debug("Debug mode details.")
    logger.warning("Low memory.")
```

---

## 17. Async Functions

Normally, Python runs code **one line at a time**, from top to bottom. If one line takes a long time — like downloading a file, calling an API, or querying a database — the entire program sits there and waits. Nothing else can happen until that slow operation finishes. This is called **synchronous** (or blocking) execution.

**Asynchronous** programming solves this. It lets your program say: *"Start this slow task, and while you're waiting for it, go do something else useful."* When the slow task finishes, the program comes back and picks up where it left off.

Python's `asyncio` module (part of the standard library) provides the tools for this.

### A real-world analogy

Imagine you're a chef in a kitchen:

- **Synchronous (blocking):** You put bread in the toaster. You stand there staring at the toaster doing nothing until the toast pops. Then you start boiling water. You stand there doing nothing until it boils. Then you crack the eggs.
- **Asynchronous (non-blocking):** You put bread in the toaster. While the toast is toasting, you start boiling water. While the water is heating, you crack the eggs. When the toast pops, you go grab it. When the water boils, you go use it.

You're still one person (one thread), but you're **switching between tasks** during the waiting periods instead of doing nothing. That's exactly what `asyncio` does.

### When is async useful?

Async shines when your program spends most of its time **waiting** — not computing:

| Good use cases (I/O-bound) | Not helpful (CPU-bound) |
|---|---|
| Calling web APIs | Heavy math calculations |
| Reading/writing files | Image processing |
| Querying databases | Training ML models |
| Web scraping multiple pages | Sorting massive arrays |
| Chat bots / WebSocket servers | Video encoding |

If your bottleneck is the CPU doing hard work, async won't help — you need multiprocessing or threading instead. Async is for **I/O-bound** tasks where you're waiting on external things.

### The building blocks: async and await

Python uses two keywords:

- **`async def`** — defines an asynchronous function (called a **coroutine**).
- **`await`** — pauses the coroutine and lets other tasks run while waiting for a result.

```python
import asyncio

async def say_hello():
    print("Hello ...")
    await asyncio.sleep(1)    # Simulate a 1-second wait (like an API call)
    print("... World!")

asyncio.run(say_hello())
# Hello ...
# (1 second passes)
# ... World!
```

Let's break this down:

1. `async def say_hello()` — this makes `say_hello` a coroutine, not a regular function. Calling it doesn't run it immediately — it creates a coroutine object.
2. `await asyncio.sleep(1)` — this tells Python: *"Pause here for 1 second, but you're free to do other things in the meantime."* `asyncio.sleep` is the async version of `time.sleep`.
3. `asyncio.run(say_hello())` — this starts the async event loop and runs the coroutine. This is how you kick off async code from regular (synchronous) code.

> **Important:** You can only use `await` inside a function defined with `async def`. Using it in a regular function is a syntax error.

### Why not just use time.sleep()?

`time.sleep(1)` blocks **everything** — the entire program freezes for 1 second. `asyncio.sleep(1)` only pauses the current coroutine — other coroutines can run during that time. This difference is the whole point:

```python
import asyncio
import time

# WRONG — blocking sleep, defeats the purpose of async
async def bad_example():
    time.sleep(1)    # Blocks the entire event loop!

# RIGHT — non-blocking sleep
async def good_example():
    await asyncio.sleep(1)    # Other coroutines can run during this second
```

### Running multiple tasks concurrently

The real power of async appears when you run multiple tasks at the same time.

#### Sequential (slow)

```python
import asyncio
import time

async def fetch_data(name, seconds):
    print(f"Starting {name}...")
    await asyncio.sleep(seconds)    # Simulates an API call taking `seconds` seconds
    print(f"Finished {name}.")
    return f"{name} result"

async def main():
    start = time.time()

    # Sequential — one after another, total time = 1 + 2 + 3 = 6 seconds
    result1 = await fetch_data("API 1", 1)
    result2 = await fetch_data("API 2", 2)
    result3 = await fetch_data("API 3", 3)

    elapsed = time.time() - start
    print(f"Total time: {elapsed:.1f}s")
    print(result1, result2, result3)

asyncio.run(main())
# Starting API 1...
# Finished API 1.
# Starting API 2...
# Finished API 2.
# Starting API 3...
# Finished API 3.
# Total time: 6.0s
# API 1 result API 2 result API 3 result
```

Each call waits for the previous one to finish. This is no different from synchronous code.

#### Concurrent with asyncio.gather (fast)

```python
import asyncio
import time

async def fetch_data(name, seconds):
    print(f"Starting {name}...")
    await asyncio.sleep(seconds)
    print(f"Finished {name}.")
    return f"{name} result"

async def main():
    start = time.time()

    # Concurrent — all three start at the same time, total time = max(1, 2, 3) = 3 seconds
    results = await asyncio.gather(
        fetch_data("API 1", 1),
        fetch_data("API 2", 2),
        fetch_data("API 3", 3),
    )

    elapsed = time.time() - start
    print(f"Total time: {elapsed:.1f}s")
    print(results)

asyncio.run(main())
# Starting API 1...
# Starting API 2...
# Starting API 3...
# Finished API 1.
# Finished API 2.
# Finished API 3.
# Total time: 3.0s
# ['API 1 result', 'API 2 result', 'API 3 result']
```

`asyncio.gather()` is the key function. It takes multiple coroutines and runs them **concurrently**. The total time is the time of the slowest task, not the sum of all tasks.

Notice all three started immediately, and then they finished in order of their duration. During the time API 1 was sleeping, APIs 2 and 3 were also sleeping — all three waits happened at the same time.

### Tasks — an alternative to gather

`asyncio.create_task()` schedules a coroutine to run in the background. It gives you more control than `gather()`:

```python
import asyncio

async def fetch_data(name, seconds):
    print(f"Starting {name}...")
    await asyncio.sleep(seconds)
    print(f"Finished {name}.")
    return f"{name} result"

async def main():
    # Create tasks — they start running immediately in the background
    task1 = asyncio.create_task(fetch_data("API 1", 2))
    task2 = asyncio.create_task(fetch_data("API 2", 1))

    print("Tasks are running in the background...")

    # Await them when you need the results
    result1 = await task1
    result2 = await task2

    print(result1, result2)

asyncio.run(main())
# Starting API 1...
# Starting API 2...
# Tasks are running in the background...
# Finished API 2.
# Finished API 1.
# API 1 result API 2 result
```

The difference from `gather()`:

| Feature | `asyncio.gather()` | `asyncio.create_task()` |
|---|---|---|
| Returns results | All at once (list) | One at a time (per task) |
| Cancellation | Cancel all if one fails (optional) | Cancel individually |
| When tasks start | When gather is awaited | Immediately when created |
| Best for | Fire-and-collect pattern | Fine-grained control |

### Handling errors in async code

Exceptions in async functions work just like in regular code — but there are some nuances when running concurrent tasks:

```python
import asyncio

async def might_fail(name, should_fail=False):
    await asyncio.sleep(1)
    if should_fail:
        raise ValueError(f"{name} failed!")
    return f"{name} succeeded"

# Option 1: try/except around a single await
async def main_single():
    try:
        result = await might_fail("task", should_fail=True)
    except ValueError as e:
        print(f"Caught error: {e}")

asyncio.run(main_single())
# Caught error: task failed!
```

With `gather()`, by default, if one task raises an exception, `gather` re-raises it and the other results are lost. Use `return_exceptions=True` to get exceptions as return values instead:

```python
import asyncio

async def might_fail(name, should_fail=False):
    await asyncio.sleep(1)
    if should_fail:
        raise ValueError(f"{name} failed!")
    return f"{name} succeeded"

async def main():
    results = await asyncio.gather(
        might_fail("Task 1", should_fail=False),
        might_fail("Task 2", should_fail=True),
        might_fail("Task 3", should_fail=False),
        return_exceptions=True    # Don't crash — return exceptions as values
    )

    for result in results:
        if isinstance(result, Exception):
            print(f"Error: {result}")
        else:
            print(f"Success: {result}")

asyncio.run(main())
# Success: Task 1 succeeded
# Error: Task 2 failed!
# Success: Task 3 succeeded
```

### Timeouts

Set a maximum time for an async operation. If it takes too long, cancel it:

```python
import asyncio

async def slow_operation():
    await asyncio.sleep(10)    # Takes 10 seconds
    return "done"

async def main():
    try:
        result = await asyncio.wait_for(slow_operation(), timeout=3.0)
        print(result)
    except asyncio.TimeoutError:
        print("Operation timed out!")

asyncio.run(main())
# Operation timed out!   (after 3 seconds, not 10)
```

### Async for and async with

Just like regular `for` and `with`, Python has async versions for use with asynchronous iterators and context managers:

```python
import asyncio

# Async iterator — yields values with pauses in between
async def countdown(n):
    while n > 0:
        yield n
        await asyncio.sleep(0.5)
        n -= 1

async def main():
    async for number in countdown(5):
        print(number)

asyncio.run(main())
# 5
# 4
# 3
# 2
# 1
```

`async with` is used with async context managers — most commonly for network connections or database sessions that need async setup and teardown:

```python
import asyncio

class AsyncConnection:
    async def __aenter__(self):
        print("Opening connection...")
        await asyncio.sleep(0.5)    # Simulate connection setup
        return self

    async def __aexit__(self, exc_type, exc_val, exc_tb):
        print("Closing connection...")
        await asyncio.sleep(0.5)    # Simulate cleanup

    async def query(self, sql):
        await asyncio.sleep(0.3)
        return f"Results for: {sql}"

async def main():
    async with AsyncConnection() as conn:
        result = await conn.query("SELECT * FROM users")
        print(result)

asyncio.run(main())
# Opening connection...
# Results for: SELECT * FROM users
# Closing connection...
```

### Real-world example: fetching multiple web pages

Here is a practical example using the `aiohttp` library (install with `uv pip install aiohttp`) to fetch multiple web pages concurrently:

```python
import asyncio
import aiohttp
import time

async def fetch_url(session, url):
    """Fetch a single URL and return its status and length."""
    async with session.get(url) as response:
        text = await response.text()
        return {"url": url, "status": response.status, "length": len(text)}

async def main():
    urls = [
        "https://httpbin.org/delay/1",
        "https://httpbin.org/delay/2",
        "https://httpbin.org/delay/1",
    ]

    start = time.time()

    # aiohttp.ClientSession is the async equivalent of requests.Session
    async with aiohttp.ClientSession() as session:
        tasks = [fetch_url(session, url) for url in urls]
        results = await asyncio.gather(*tasks)

    elapsed = time.time() - start

    for result in results:
        print(f"{result['url']} — status {result['status']}, {result['length']} chars")

    print(f"\nFetched {len(urls)} URLs in {elapsed:.1f}s")
    # Without async, this would take 1+2+1 = 4 seconds
    # With async, it takes ~2 seconds (the slowest request)

asyncio.run(main())
```

> **Note:** You cannot use the regular `requests` library inside async functions — it blocks the event loop. Use `aiohttp` for async HTTP requests, or `httpx` which supports both sync and async.

### Semaphores — limiting concurrency

If you're making hundreds of requests at once, you might overwhelm the server or run out of connections. A **semaphore** limits how many coroutines can run a specific section at the same time:

```python
import asyncio

async def fetch_with_limit(semaphore, name, seconds):
    async with semaphore:    # Only N coroutines can enter this block at once
        print(f"Starting {name}")
        await asyncio.sleep(seconds)
        print(f"Finished {name}")
        return f"{name} result"

async def main():
    semaphore = asyncio.Semaphore(2)    # Allow max 2 concurrent tasks

    tasks = [
        fetch_with_limit(semaphore, f"Task {i}", 1)
        for i in range(6)
    ]

    results = await asyncio.gather(*tasks)
    print(results)

asyncio.run(main())
# Tasks run in batches of 2:
# Starting Task 0
# Starting Task 1
# Finished Task 0
# Finished Task 1
# Starting Task 2
# Starting Task 3
# ... and so on
```

### Summary: sync vs async side by side

```python
# ── Synchronous ──────────────────────
import requests
import time

def fetch_sync(url):
    return requests.get(url).text

start = time.time()
for url in urls:
    fetch_sync(url)                   # One at a time, total = sum of all
print(f"Sync: {time.time() - start:.1f}s")


# ── Asynchronous ─────────────────────
import aiohttp
import asyncio
import time

async def fetch_async(session, url):
    async with session.get(url) as resp:
        return await resp.text()

async def main():
    start = time.time()
    async with aiohttp.ClientSession() as session:
        tasks = [fetch_async(session, url) for url in urls]
        await asyncio.gather(*tasks)   # All at once, total = max of all
    print(f"Async: {time.time() - start:.1f}s")

asyncio.run(main())
```

The synchronous version processes URLs one by one. The async version fires all requests at roughly the same time and waits for them to finish together.

---

## 18. DataFrames with pandas

pandas is the standard Python library for working with structured, tabular data — like spreadsheets or SQL tables. Its core object is the **DataFrame**: a two-dimensional table with labeled rows and columns.

### Installing pandas

```bash
uv pip install pandas
```

### Importing

```python
import pandas as pd
```

### Creating a DataFrame

```python
# From a dictionary
data = {
    "name": ["Alice", "Bob", "Charlie", "Diana"],
    "age": [30, 25, 35, 28],
    "city": ["London", "Paris", "Berlin", "Madrid"],
    "score": [85.5, 92.0, 78.3, 95.1]
}
df = pd.DataFrame(data)
print(df)
#       name  age    city  score
# 0    Alice   30  London   85.5
# 1      Bob   25   Paris   92.0
# 2  Charlie   35  Berlin   78.3
# 3    Diana   28  Madrid   95.1

# From a list of dictionaries
records = [
    {"name": "Alice", "age": 30},
    {"name": "Bob", "age": 25},
]
df = pd.DataFrame(records)
```

### Reading data from files

```python
# CSV
df = pd.read_csv("data.csv")

# Excel
df = pd.read_excel("data.xlsx")

# JSON
df = pd.read_json("data.json")

# From a URL
df = pd.read_csv("https://example.com/data.csv")
```

### Quick exploration

```python
df.head()          # First 5 rows
df.head(10)        # First 10 rows
df.tail()          # Last 5 rows
df.shape           # (number_of_rows, number_of_columns)
df.columns         # List of column names
df.dtypes          # Data type of each column
df.info()          # Summary: column names, types, non-null counts
df.describe()      # Statistics for numeric columns: mean, std, min, max, quartiles
```

### Selecting columns

```python
# Single column — returns a Series (one-dimensional)
df["name"]
df.name             # Shorthand (doesn't work if column name has spaces or conflicts with a method)

# Multiple columns — returns a DataFrame
df[["name", "age"]]
```

### Selecting rows

```python
# By position (integer-based) — iloc
df.iloc[0]          # First row (returns a Series)
df.iloc[0:3]        # Rows 0, 1, 2 (returns a DataFrame)
df.iloc[-1]         # Last row

# By label (index-based) — loc
df.loc[0]           # Row with index label 0
df.loc[0:2]         # Rows with labels 0 through 2 (inclusive, unlike iloc!)
df.loc[0, "name"]   # Specific cell: row 0, column "name"

# Selecting specific rows and columns
df.loc[0:2, ["name", "age"]]       # Rows 0–2, only name and age columns
df.iloc[0:3, 0:2]                  # Rows 0–2, first 2 columns (by position)
```

### Filtering rows (boolean indexing)

```python
# Rows where age > 28
df[df["age"] > 28]

# Rows where city is Paris
df[df["city"] == "Paris"]

# Multiple conditions — use & (and), | (or), ~ (not)
# Important: each condition must be in parentheses
df[(df["age"] > 25) & (df["score"] > 80)]
df[(df["city"] == "London") | (df["city"] == "Paris")]
df[~(df["city"] == "Berlin")]     # NOT Berlin

# Using isin for multiple values
df[df["city"].isin(["London", "Paris"])]

# String methods
df[df["name"].str.startswith("A")]
df[df["name"].str.contains("li")]
```

### Adding and modifying columns

```python
# New column from a calculation
df["score_doubled"] = df["score"] * 2

# New column with a condition
df["passed"] = df["score"] >= 80

# Apply a function to a column
df["name_upper"] = df["name"].apply(str.upper)
df["name_length"] = df["name"].apply(len)

# Apply a custom function
df["age_group"] = df["age"].apply(lambda x: "young" if x < 30 else "senior")
```

### Removing columns and rows

```python
# Drop a column
df = df.drop(columns=["score_doubled"])

# Drop multiple columns
df = df.drop(columns=["passed", "name_upper"])

# Drop rows by index
df = df.drop(index=[0, 2])

# Drop rows with missing values
df = df.dropna()                     # Drop any row with at least one NaN
df = df.dropna(subset=["score"])     # Only check the "score" column
```

### Handling missing data

```python
# Check for missing values
df.isnull()              # DataFrame of True/False
df.isnull().sum()        # Count of NaN per column

# Fill missing values
df["score"] = df["score"].fillna(0)           # Replace NaN with 0
df["score"] = df["score"].fillna(df["score"].mean())   # Replace with mean

# Drop rows with any missing values
df = df.dropna()

# Drop rows where a specific column is NaN
df = df.dropna(subset=["score"])
```

### Sorting

```python
# Sort by a column
df = df.sort_values("age")                       # Ascending (default)
df = df.sort_values("score", ascending=False)     # Descending

# Sort by multiple columns
df = df.sort_values(["city", "age"], ascending=[True, False])

# Reset the index after sorting
df = df.sort_values("age").reset_index(drop=True)
```

### Grouping and aggregation

This is one of the most powerful features of pandas. Group rows by a column and compute summary statistics:

```python
# Average score by city
df.groupby("city")["score"].mean()

# Multiple aggregations
df.groupby("city")["score"].agg(["mean", "min", "max", "count"])

# Group by multiple columns
df.groupby(["city", "passed"])["score"].mean()

# Custom aggregation per column
df.groupby("city").agg(
    avg_score=("score", "mean"),
    oldest=("age", "max"),
    count=("name", "count")
)
```

### Merging DataFrames

```python
# Like SQL JOIN
orders = pd.DataFrame({
    "order_id": [1, 2, 3],
    "customer_id": [101, 102, 101],
    "amount": [50, 75, 30]
})

customers = pd.DataFrame({
    "customer_id": [101, 102, 103],
    "name": ["Alice", "Bob", "Charlie"]
})

# Inner join (default) — only matching rows
merged = pd.merge(orders, customers, on="customer_id")

# Left join — keep all rows from the left table
merged = pd.merge(orders, customers, on="customer_id", how="left")

# Other join types: "right", "outer" (keep all rows from both)
```

### Concatenating DataFrames

```python
# Stack vertically (add more rows)
df1 = pd.DataFrame({"name": ["Alice"], "age": [30]})
df2 = pd.DataFrame({"name": ["Bob"], "age": [25]})
combined = pd.concat([df1, df2], ignore_index=True)

# Stack horizontally (add more columns)
scores = pd.DataFrame({"score": [85, 92]})
combined = pd.concat([df1, scores], axis=1)
```

### Saving data

```python
# To CSV
df.to_csv("output.csv", index=False)      # index=False prevents writing the index column

# To Excel
df.to_excel("output.xlsx", index=False)

# To JSON
df.to_json("output.json", orient="records", indent=2)
```

### Pivot tables

```python
sales = pd.DataFrame({
    "region": ["North", "North", "South", "South"],
    "product": ["A", "B", "A", "B"],
    "revenue": [100, 150, 200, 50]
})

pivot = sales.pivot_table(
    values="revenue",
    index="region",
    columns="product",
    aggfunc="sum"
)
print(pivot)
# product     A    B
# region
# North     100  150
# South     200   50
```

### Value counts

```python
# Count occurrences of each unique value in a column
df["city"].value_counts()
# London    1
# Paris     1
# Berlin    1
# Madrid    1

# As percentages
df["city"].value_counts(normalize=True)
```

### Method chaining

pandas methods return DataFrames, so you can chain them together for clean, readable pipelines:

```python
result = (
    df[df["score"] > 80]
    .sort_values("score", ascending=False)
    .reset_index(drop=True)
    [["name", "score"]]
)
```

> **Tip:** When dealing with large datasets, consider using `.query()` for filtering — it's often more readable:
>
> ```python
> df.query("age > 25 and score > 80")
> ```
