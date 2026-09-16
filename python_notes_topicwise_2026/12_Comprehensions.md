# Comprehensions ⭐ 🔥 Core

A **comprehension** is a concise way to create a new collection from an iterable, optionally applying a condition or transformation.

Python provides:

* List comprehensions
* Set comprehensions
* Dictionary comprehensions
* Generator expressions

The basic mental model is:

```text
Iterable
   ↓
Loop over items
   ↓
(Optional) condition
   ↓
(Optional) transformation
   ↓
New collection
```

---

# 1. List Comprehension

A **list comprehension** is a compact way to create a list using a loop.

### Normal loop

```python
numbers = [1, 2, 3, 4, 5]

squares = []

for number in numbers:
    squares.append(number ** 2)

print(squares)
```

Output:

```text
[1, 4, 9, 16, 25]
```

### List comprehension

```python
numbers = [1, 2, 3, 4, 5]

squares = [number ** 2 for number in numbers]

print(squares)
```

Output:

```text
[1, 4, 9, 16, 25]
```

The two versions do essentially the same thing.

### Syntax

```python
[expression for item in iterable]
```

Think:

```text
[what to put in list  for  each item  in  iterable]
```

For:

```python
[number ** 2 for number in numbers]
```

we have:

```text
number ** 2 → expression
number       → item
numbers      → iterable
```

---

# 2. List Comprehension with Strings

Comprehensions aren't limited to numbers.

```python
names = ["rahul", "aman", "priya"]

upper_names = [name.upper() for name in names]

print(upper_names)
```

Output:

```text
['RAHUL', 'AMAN', 'PRIYA']
```

---

# 3. Conditional List Comprehension

You can add an `if` condition to select which items should be included.

### Syntax

```python
[expression for item in iterable if condition]
```

Example:

```python
numbers = [1, 2, 3, 4, 5, 6]

even_numbers = [number for number in numbers if number % 2 == 0]

print(even_numbers)
```

Output:

```text
[2, 4, 6]
```

Mental model:

```text
for each number
      ↓
is number even?
   ↓       ↓
 yes       no
  ↓         ↓
include    ignore
```

---

## Conditional Transformation

You can also use an `if-else` expression.

```python
numbers = [1, 2, 3, 4, 5]

result = [
    "even" if number % 2 == 0 else "odd"
    for number in numbers
]

print(result)
```

Output:

```text
['odd', 'even', 'odd', 'even', 'odd']
```

### Important difference

#### Filtering:

```python
[number for number in numbers if number % 2 == 0]
```

Some elements are **excluded**.

#### Conditional expression:

```python
["even" if number % 2 == 0 else "odd" for number in numbers]
```

Every element produces a result, but the result changes depending on the condition.

---

# 4. Nested List Comprehension

A **nested list comprehension** is a comprehension containing multiple `for` clauses.

Example:

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]

result = [number for row in matrix for number in row]

print(result)
```

Output:

```text
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```

This is equivalent to:

```python
result = []

for row in matrix:
    for number in row:
        result.append(number)
```

### How to read it

```python
[number for row in matrix for number in row]
```

Read it from left to right:

```text
for each row in matrix
    ↓
    for each number in row
        ↓
        put number into result
```

---

## Nested Loop for a Cartesian Product

Another example:

```python
colors = ["red", "blue"]
sizes = ["S", "M"]

combinations = [
    (color, size)
    for color in colors
    for size in sizes
]

print(combinations)
```

Output:

```text
[('red', 'S'), ('red', 'M'), ('blue', 'S'), ('blue', 'M')]
```

This is useful, but **don't over-practice complicated nested comprehensions**.

---

# 5. Set Comprehension

Set comprehensions work similarly to list comprehensions, but create a **set**.

### Syntax

```python
{expression for item in iterable}
```

Example:

```python
numbers = [1, 2, 2, 3, 3, 4]

squares = {number ** 2 for number in numbers}

print(squares)
```

Output:

```text
{1, 4, 9, 16}
```

Duplicates are automatically removed because sets contain unique elements.

### Normal loop

```python
squares = set()

for number in numbers:
    squares.add(number ** 2)
```

### Comprehension

```python
squares = {number ** 2 for number in numbers}
```

---

# 6. Dictionary Comprehension

A **dictionary comprehension** creates a dictionary using a concise syntax.

### Syntax

```python
{key: value for item in iterable}
```

Example:

```python
numbers = [1, 2, 3, 4]

squares = {
    number: number ** 2
    for number in numbers
}

print(squares)
```

Output:

```text
{1: 1, 2: 4, 3: 9, 4: 16}
```

---

## Dictionary Comprehension with Transformation

```python
names = ["rahul", "aman", "priya"]

name_lengths = {
    name: len(name)
    for name in names
}

print(name_lengths)
```

Output:

```text
{'rahul': 5, 'aman': 4, 'priya': 5}
```

This pattern is very useful when processing structured data.

---

## Dictionary Comprehension with Condition

```python
numbers = range(1, 6)

squares = {
    number: number ** 2
    for number in numbers
    if number % 2 == 0
}

print(squares)
```

Output:

```text
{2: 4, 4: 16}
```

---

# 7. Generator Expressions ⭐

A **generator expression** looks similar to a list comprehension but produces a **generator object** instead of immediately creating a complete list.

List comprehension:

```python
squares = [number ** 2 for number in range(5)]
```

Generator expression:

```python
squares = (number ** 2 for number in range(5))
```

Notice:

```text
[ ] → list comprehension
( ) → generator expression
```

### Example

```python
squares = (number ** 2 for number in range(5))

print(squares)
```

You'll get something similar to:

```text
<generator object ...>
```

The values are generated **lazily**, when requested.

```python
print(list(squares))
```

Output:

```text
[0, 1, 4, 9, 16]
```

---

## Why Generator Expressions Matter

Suppose:

```python
numbers = [1, 2, 3, 4, 5]
```

List comprehension:

```python
squares = [number ** 2 for number in numbers]
```

creates the entire list immediately.

Generator expression:

```python
squares = (number ** 2 for number in numbers)
```

generates values as they are requested.

Conceptually:

```text
List comprehension
     ↓
create all results
     ↓
store them

Generator expression
     ↓
produce one value when needed
     ↓
produce next value when needed
```

This makes generator expressions useful when working with **large amounts of data**.

### Important

Don't confuse a **generator expression** with the broader concept of a **generator function using `yield`**.

We'll cover generators separately and in much more depth.

---

# 8. Comprehension vs Normal Loops

Consider:

```python
numbers = [1, 2, 3, 4]

squares = []

for number in numbers:
    squares.append(number ** 2)
```

Comprehension:

```python
squares = [number ** 2 for number in numbers]
```

Both are valid.

### Comparison

| Normal Loop                  | Comprehension                             |
| ---------------------------- | ----------------------------------------- |
| More verbose                 | More concise                              |
| Easy for complex logic       | Best for simple transformations/filtering |
| Multiple statements possible | Designed around expressions               |
| Often easier for beginners   | Can be harder when nested/complex         |
| More control                 | Less code                                 |

### Important

**Comprehensions aren't just about writing fewer characters.**

They provide a clear, Pythonic way of expressing:

> "Create a collection from these items using this transformation/condition."

---

# 9. When to Use Comprehensions

Use comprehensions when the operation is **simple and easy to understand**.

### Good example

```python
numbers = [1, 2, 3, 4, 5]

squares = [number ** 2 for number in numbers]
```

Very readable.

### Good filtering example

```python
even_numbers = [
    number
    for number in numbers
    if number % 2 == 0
]
```

Clear and concise.

### Good dictionary example

```python
users = ["Rahul", "Aman", "Priya"]

user_lengths = {
    user: len(user)
    for user in users
}
```

Also easy to understand.

---

# 10. When NOT to Use Comprehensions

Don't use a comprehension when it becomes difficult to understand.

For example, deeply nested logic like:

```python
result = [
    transform(x, y)
    for x in data
    if condition(x)
    for y in other_data
    if another_condition(x, y)
]
```

It may technically be valid, but a normal loop might be clearer.

Use:

```python
result = []

for x in data:
    if condition(x):
        for y in other_data:
            if another_condition(x, y):
                result.append(transform(x, y))
```

The second version is longer but may be much easier to maintain.

### Rule

> **If a comprehension makes you stop and mentally decode it, use a normal loop.**

Readable code is more important than having the shortest code.

---

# ⚠️ Common Confusions / Traps

## 1. List comprehension vs generator expression

```python
[x * 2 for x in numbers]
```

creates a **list**.

```python
(x * 2 for x in numbers)
```

creates a **generator**.

Remember:

```text
[] → list
() → generator expression
```

---

## 2. `if` placement matters

Filtering:

```python
[x for x in numbers if x > 5]
```

Conditional expression:

```python
["big" if x > 5 else "small" for x in numbers]
```

They do different things.

---

## 3. Dictionary comprehension needs `key: value`

Correct:

```python
{x: x ** 2 for x in numbers}
```

Incorrect:

```python
{x ** 2 for x in numbers}
```

The second one is a **set comprehension**, not a dictionary comprehension.

---

## 4. Nested comprehension can become unreadable

Just because Python allows you to put many loops/conditions into one comprehension doesn't mean you should.

Prefer:

```python
for ...
    for ...
        if ...
```

when it improves readability.

---

## 5. Comprehension doesn't mean "always faster"

Comprehensions are often efficient and Pythonic, but don't choose them solely because you assume they are always faster.

**Readability and appropriate memory behavior matter more.**

---

# ⭐ Important Points

1. **Comprehension** = concise way to create a collection from an iterable.
2. List comprehension:

```python
[expression for item in iterable]
```

3. With filtering:

```python
[expression for item in iterable if condition]
```

4. Set comprehension:

```python
{expression for item in iterable}
```

5. Dictionary comprehension:

```python
{key: value for item in iterable}
```

6. Generator expression:

```python
(expression for item in iterable)
```

7. List/set/dict comprehensions create their corresponding collections immediately.
8. Generator expressions are **lazy** and produce values when requested.
9. Comprehensions are best for **simple transformations and filtering**.
10. Use normal loops when the logic becomes complex.

---

# 🎯 Interview Answer

> **What is a list comprehension?**

"A list comprehension is a concise Python syntax for creating a list by iterating over an iterable and optionally applying a transformation or condition."

Example:

```python
squares = [x ** 2 for x in numbers]
```

> **What is the difference between a list comprehension and a generator expression?**

"A list comprehension creates the complete list immediately, while a generator expression creates a generator that produces values lazily as they are requested. Generator expressions are useful when we want to avoid storing all results in memory at once."

> **When should you avoid comprehensions?**

"When the logic becomes complex or difficult to read. In those cases, a normal `for` loop is usually clearer and more maintainable."

---

# 🤖 Backend / GenAI Relevance

Comprehensions are **very useful in practical Python**, including backend and GenAI code.

You'll frequently process:

* API responses
* JSON data
* lists of objects
* model outputs
* document chunks
* metadata
* search results

For example:

```python
documents = [
    {"text": "Python", "score": 0.91},
    {"text": "AI", "score": 0.72},
    {"text": "Backend", "score": 0.88}
]

high_score_docs = [
    doc
    for doc in documents
    if doc["score"] > 0.8
]
```

Or extracting values:

```python
texts = [doc["text"] for doc in documents]
```

Or creating a mapping:

```python
scores = {
    doc["text"]: doc["score"]
    for doc in documents
}
```

These patterns are extremely common when processing structured data in backend/AI applications.

---

# 📌 Priority

| Topic                            | Priority           |
| -------------------------------- | ------------------ |
| List comprehension               | 🔥 Core            |
| Conditional list comprehension   | 🔥 Core            |
| Nested list comprehension        | 🟡 Know & Move On  |
| Set comprehension                | 🔥 Core            |
| Dictionary comprehension         | 🔥 Core            |
| Generator expressions            | 🔥 Core            |
| Comprehension vs loops           | 🔥 Core            |
| When to use / avoid              | 🔥 Core            |
| Extremely complex comprehensions | ⚪ Optional for Now |

### 🎯 What you should be able to write comfortably

```python
# Transform
squares = [x ** 2 for x in numbers]

# Filter
evens = [x for x in numbers if x % 2 == 0]

# Transform + condition
result = [x ** 2 for x in numbers if x > 5]

# Set
unique = {x.lower() for x in names}

# Dictionary
lengths = {name: len(name) for name in names}

# Generator expression
squares = (x ** 2 for x in numbers)
```

The main thing to internalize is:

> **Comprehension = loop + optional condition + expression that produces the result.**

Once this becomes natural, you can move on. **Don't spend excessive time memorizing fancy nested comprehensions.** The next topic, **Generators**, will build directly on the last part—generator expressions and lazy evaluation.
