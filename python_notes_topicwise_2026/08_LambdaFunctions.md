# Lambda Functions ⭐ 🟡 Know & Move On

A **lambda function** is a small, anonymous function used when you need a simple function for a short piece of logic.

Lambda functions become especially useful with **`map()`**, **`filter()`**, and **`sorted()`**.

---

# 59. What is a Lambda Function?

A lambda function is a function created using the `lambda` keyword instead of `def`.

Example:

```python
square = lambda x: x * x

print(square(5))
```

Output:

```text
25
```

Here:

```text
lambda x: x * x
   ↓       ↓
argument  expression
```

The lambda takes `x` and returns `x * x`.

### Why is it called "anonymous"?

A lambda function does not need to have a name.

For example:

```python
lambda x: x * 2
```

There is no function name here.

You can assign it to a variable:

```python
double = lambda x: x * 2
```

But in that case, `double` is simply a variable referring to the lambda function object.

### Important

A lambda is still a **function object**.

So the previous concept:

> Functions are objects and can be assigned to variables, passed as arguments, and returned from functions.

also applies to lambda functions.

---

# 60. Lambda Syntax

The basic syntax is:

```python
lambda arguments: expression
```

Example:

```python
lambda x: x * 2
```

Compare it with `def`:

```python
def double(x):
    return x * 2
```

Lambda:

```python
double = lambda x: x * 2
```

The main difference is that a lambda contains a **single expression** whose result is automatically returned.

### Lambda does not use `return`

This:

```python
double = lambda x: x * 2
```

is equivalent to:

```python
def double(x):
    return x * 2
```

You don't write:

```python
lambda x: return x * 2   # ❌ Invalid
```

---

## Lambda Can Have Only One Expression

A lambda can contain an expression:

```python
lambda x: x + 10
```

```python
lambda x: x > 10
```

```python
lambda x: x.upper()
```

But it is not designed for multiple statements like:

```python
x = ...
if ...
for ...
```

For multi-step logic, use a normal `def` function.

---

# 61. Lambda with One Argument

A lambda can take one argument.

```python
square = lambda x: x ** 2

print(square(4))
```

Output:

```text
16
```

Another example:

```python
cube = lambda x: x ** 3

print(cube(3))
```

Output:

```text
27
```

### Mental model

```text
lambda x: x ** 2
       ↓
    input
       ↓
   x ** 2
       ↓
    output
```

---

# 62. Lambda with Multiple Arguments

A lambda can take multiple arguments.

Syntax:

```python
lambda arg1, arg2: expression
```

Example:

```python
add = lambda a, b: a + b

print(add(10, 20))
```

Output:

```text
30
```

Another example:

```python
multiply = lambda a, b: a * b

print(multiply(5, 4))
```

Output:

```text
20
```

You can also have three or more arguments:

```python
total = lambda a, b, c: a + b + c

print(total(10, 20, 30))
```

Output:

```text
60
```

---

# 63. Lambda with `map()`

`map()` applies a function to **every item** in an iterable.

Basic idea:

```text
Input items
    ↓
function applied to each item
    ↓
new results
```

Example without lambda:

```python
numbers = [1, 2, 3, 4]

def square(x):
    return x ** 2

result = map(square, numbers)

print(list(result))
```

Output:

```text
[1, 4, 9, 16]
```

With lambda:

```python
numbers = [1, 2, 3, 4]

result = map(lambda x: x ** 2, numbers)

print(list(result))
```

Output:

```text
[1, 4, 9, 16]
```

Here:

```python
lambda x: x ** 2
```

is the function passed to `map()`.

### Why lambda is useful here?

If the function is very small and used only once, creating a separate `def` can be unnecessary.

Instead of:

```python
def square(x):
    return x ** 2

map(square, numbers)
```

you can write:

```python
map(lambda x: x ** 2, numbers)
```

---

# 64. Lambda with `filter()`

`filter()` keeps the elements for which a function returns a **truthy value**.

Example:

```python
numbers = [1, 2, 3, 4, 5, 6]

result = filter(lambda x: x % 2 == 0, numbers)

print(list(result))
```

Output:

```text
[2, 4, 6]
```

The lambda:

```python
lambda x: x % 2 == 0
```

checks whether each number is even.

Conceptually:

```text
1 → False → ❌
2 → True  → ✅
3 → False → ❌
4 → True  → ✅
5 → False → ❌
6 → True  → ✅
```

So only:

```text
[2, 4, 6]
```

remain.

### Another example

```python
names = ["Raj", "Abhijit", "Aman", "Rohit"]

result = filter(lambda name: len(name) > 4, names)

print(list(result))
```

Output:

```text
['Abhijit', 'Rohit']
```

---

# 65. Lambda with `sorted()`

This is one of the **most useful real-world uses of lambda**.

`sorted()` can accept a `key` function that tells Python **what value to use for sorting**.

Example:

```python
students = [
    ("Rahul", 85),
    ("Aman", 92),
    ("Priya", 78)
]

result = sorted(students, key=lambda student: student[1])

print(result)
```

Output:

```text
[
    ('Priya', 78),
    ('Rahul', 85),
    ('Aman', 92)
]
```

Here:

```python
key=lambda student: student[1]
```

means:

> Sort each tuple using its second element.

### Why?

For:

```text
("Rahul", 85)
```

the lambda returns:

```text
85
```

For:

```text
("Aman", 92)
```

it returns:

```text
92
```

For:

```text
("Priya", 78)
```

it returns:

```text
78
```

So Python sorts based on those numbers.

---

## Sorting in Descending Order

Use `reverse=True`:

```python
students = [
    ("Rahul", 85),
    ("Aman", 92),
    ("Priya", 78)
]

result = sorted(
    students,
    key=lambda student: student[1],
    reverse=True
)

print(result)
```

Output:

```text
[
    ('Aman', 92),
    ('Rahul', 85),
    ('Rahul', 85)
]
```

Correction: with the given data, the correct output is:

```text
[
    ('Aman', 92),
    ('Rahul', 85),
    ('Priya', 78)
]
```

---

## Sorting Dictionaries by a Value

This pattern is particularly useful.

```python
students = [
    {"name": "Rahul", "marks": 85},
    {"name": "Aman", "marks": 92},
    {"name": "Priya", "marks": 78}
]

result = sorted(
    students,
    key=lambda student: student["marks"]
)

print(result)
```

The dictionaries are sorted according to their `"marks"` value.

### Another common example

Sort strings by length:

```python
names = ["Abhijit", "Ram", "Aman", "Christopher"]

result = sorted(names, key=lambda name: len(name))

print(result)
```

Output:

```text
['Ram', 'Aman', 'Abhijit', 'Christopher']
```

This is a very useful pattern to remember:

```python
sorted(data, key=lambda item: ...)
```

---

# 66. Lambda vs Normal `def` Function

| Feature       | Lambda                          | `def`                     |
| ------------- | ------------------------------- | ------------------------- |
| Keyword       | `lambda`                        | `def`                     |
| Name          | Usually anonymous               | Usually named             |
| Body          | Single expression               | Multiple statements       |
| `return`      | Not written explicitly          | Can use `return`          |
| Best for      | Small/simple logic              | Complex/reusable logic    |
| Documentation | Limited                         | Docstrings supported      |
| Readability   | Good for short logic            | Better for larger logic   |
| Common use    | `map()`, `filter()`, `sorted()` | General-purpose functions |

### Example

Lambda:

```python
square = lambda x: x ** 2
```

Normal function:

```python
def square(x):
    return x ** 2
```

Both produce the same result.

---

# Lambda Doesn't Mean "More Powerful"

A common misconception is that lambda is a special, more powerful type of function.

It isn't.

A lambda is simply a **compact way of creating a function expression**.

For example:

```python
square = lambda x: x ** 2
```

and:

```python
def square(x):
    return x ** 2
```

both create callable function objects.

The main difference is **syntax and intended use**.

---

# ⚠️ Common Confusions / Traps

### 1. Lambda automatically returns its expression

```python
square = lambda x: x ** 2
```

There is no explicit `return`.

The result of the expression is returned automatically.

---

### 2. Don't write `return` inside lambda

```python
lambda x: return x * 2   # ❌
```

Correct:

```python
lambda x: x * 2
```

---

### 3. `map()` and `filter()` don't normally give you a list directly

In Python 3:

```python
result = map(lambda x: x * 2, numbers)
```

`result` is a **map object**.

To see the values as a list:

```python
list(result)
```

Similarly:

```python
result = filter(lambda x: x > 10, numbers)

print(list(result))
```

---

### 4. `sorted()` returns a new list

```python
numbers = [3, 1, 2]

result = sorted(numbers)
```

`numbers` remains unchanged.

```python
print(numbers)
# [3, 1, 2]

print(result)
# [1, 2, 3]
```

This is different from `list.sort()`, which modifies the list in place.

---

### 5. Don't force lambda into complicated logic

Bad style:

```python
process = lambda x: x * 2 if x > 10 else x + 5 if x > 5 else x
```

It may technically work, but it becomes difficult to read.

A normal function is often better:

```python
def process(x):
    if x > 10:
        return x * 2
    elif x > 5:
        return x + 5
    return x
```

**Use lambda when it makes the code simpler, not merely shorter.**

---

# ⭐ Important Points

1. A lambda is a **small anonymous function**.
2. Syntax:

```python
lambda arguments: expression
```

3. A lambda can have:

   * one argument
   * multiple arguments
4. A lambda contains a **single expression**.
5. The expression's result is automatically returned.
6. Lambda is commonly used with:

   * `map()`
   * `filter()`
   * `sorted()`
7. `sorted(..., key=lambda ...)` is particularly important and practical.
8. Lambda functions are still **function objects**.
9. Use `def` when logic is complex, reusable, or deserves a meaningful name.

---

# 🎯 Interview Answer

> **What is a lambda function in Python?**

"A lambda function is a small anonymous function defined using the `lambda` keyword. It can take multiple arguments but contains a single expression whose result is automatically returned. Lambda functions are commonly used for short operations with functions such as `map()`, `filter()`, and `sorted()`."

### If asked for an example:

```python
square = lambda x: x ** 2

print(square(5))
```

Output:

```text
25
```

### If asked about lambda vs `def`:

> "Lambda is useful for short, simple, one-expression functions, especially when passing a function as an argument. `def` is preferred for complex, reusable, or documented functions."

---

# 🤖 Backend / GenAI Relevance

Lambda itself is **not a major GenAI concept**, so don't over-prioritize it.

But the underlying idea is important:

```text
Function objects
      ↓
Functions passed as arguments
      ↓
Higher-order functions
      ↓
map / filter / sorted
      ↓
Decorators / callbacks
```

You will encounter this style of Python programming in backend code and libraries.

For example, sorting API/JSON data:

```python
results = sorted(
    results,
    key=lambda item: item["score"],
    reverse=True
)
```

This kind of code can appear when processing:

* API responses
* search results
* model outputs
* ranked documents
* structured data

So learn the **pattern**, but don't spend excessive time memorizing lambda tricks.

---

# 📌 Priority

| Topic                  | Priority           |
| ---------------------- | ------------------ |
| What lambda is         | 🔥 Core            |
| Lambda syntax          | 🔥 Core            |
| One/multiple arguments | 🔥 Core            |
| Lambda + `map()`       | 🟡 Know & Move On  |
| Lambda + `filter()`    | 🟡 Know & Move On  |
| Lambda + `sorted()`    | 🔥 Core            |
| Lambda vs `def`        | 🔥 Core            |
| Advanced lambda tricks | ⚪ Optional for Now |

### 🎯 What you should remember

If you remember only three things:

```python
lambda x: x * 2
```

means:

> **"Create a small function that takes `x` and returns `x * 2`."**

And these two patterns are especially worth remembering:

```python
map(lambda x: x * 2, numbers)
```

```python
sorted(data, key=lambda item: item["score"])
```

Don't try to replace every `def` with a lambda. **Use lambda when a small function makes the surrounding code clearer.**
