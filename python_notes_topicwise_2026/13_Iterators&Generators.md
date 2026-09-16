# Iterators & Generators ⭐ 🔥 Core

Iterators and generators are important Python concepts because they explain **how Python processes data one item at a time**.

They are especially useful when working with:

* large datasets
* files
* API responses
* streams
* data pipelines
* AI/ML data processing
* potentially unbounded data

The core relationship is:

```text
Iterable
   ↓ iter()
Iterator
   ↓ next()
Next value
   ↓
next()
Next value
   ↓
...
StopIteration
```

And:

```text
Generator function
       ↓
     yield
       ↓
   Generator object
       ↓
     Iterator
```

---

# 1. Iterable vs Iterator

These two terms are often confused.

## Iterable

An **iterable** is an object that can be iterated over.

Examples:

```python
list
tuple
string
set
dictionary
range
```

For example:

```python
numbers = [10, 20, 30]

for number in numbers:
    print(number)
```

`numbers` is an **iterable**.

### Simple definition

> **Iterable = an object you can get an iterator from.**

An iterable generally provides `__iter__()`.

---

# Iterator

An **iterator** is an object that produces values **one at a time**.

An iterator follows the iterator protocol:

```text
__iter__()
__next__()
```

Example:

```python
numbers = [10, 20, 30]

iterator = iter(numbers)

print(next(iterator))
print(next(iterator))
print(next(iterator))
```

Output:

```text
10
20
30
```

Here:

```text
numbers  → iterable
iterator → iterator
```

### Key difference

```text
Iterable
→ can give you an iterator

Iterator
→ gives you the next value
```

---

# 2. `iter()`

`iter()` is used to obtain an **iterator from an iterable**.

Example:

```python
numbers = [10, 20, 30]

iterator = iter(numbers)
```

Now:

```python
print(iterator)
```

will show something similar to:

```text
<list_iterator object at ...>
```

You can then use `next()`:

```python
print(next(iterator))
```

Output:

```text
10
```

Then:

```python
print(next(iterator))
```

Output:

```text
20
```

Then:

```python
print(next(iterator))
```

Output:

```text
30
```

---

# 3. `next()`

`next()` asks an iterator:

> **"Give me the next value."**

Example:

```python
numbers = [10, 20, 30]

iterator = iter(numbers)

print(next(iterator))
print(next(iterator))
print(next(iterator))
```

Output:

```text
10
20
30
```

After all values are consumed:

```python
next(iterator)
```

raises:

```text
StopIteration
```

---

# 4. `StopIteration`

`StopIteration` is the exception used to indicate that an iterator has **no more values to produce**.

Example:

```python
numbers = [10, 20]

iterator = iter(numbers)

print(next(iterator))
print(next(iterator))
print(next(iterator))
```

The third `next()` raises:

```text
StopIteration
```

Conceptually:

```text
next() → 10
next() → 20
next() → StopIteration
```

---

## How Does `for` Handle This?

When you write:

```python
for number in numbers:
    print(number)
```

Python internally uses the iterator protocol.

Conceptually, it behaves somewhat like:

```python
iterator = iter(numbers)

while True:
    try:
        number = next(iterator)
        print(number)
    except StopIteration:
        break
```

You normally don't write this yourself.

The `for` loop handles `StopIteration` automatically.

### Mental model

```text
for loop
   ↓
iter()
   ↓
next()
   ↓
next()
   ↓
next()
   ↓
StopIteration
   ↓
loop ends
```

This is one of the most important things to understand about Python iteration.

---

# 5. What is a Generator? ⭐

A **generator** is a special kind of iterator that produces values **lazily**, usually using the `yield` keyword.

The easiest way to create one is with a **generator function**.

Example:

```python
def numbers():
    yield 1
    yield 2
    yield 3
```

Calling:

```python
result = numbers()
```

does **not** immediately execute the function body.

Instead, it creates a generator object.

```python
print(result)
```

You will see something similar to:

```text
<generator object numbers at ...>
```

Then:

```python
print(next(result))
```

Output:

```text
1
```

Next:

```python
print(next(result))
```

Output:

```text
2
```

And:

```python
print(next(result))
```

Output:

```text
3
```

Finally:

```python
next(result)
```

raises:

```text
StopIteration
```

---

# 6. `yield`

`yield` is used inside a generator function to **produce a value and temporarily pause the function**.

Example:

```python
def numbers():
    yield 1
    yield 2
    yield 3
```

Unlike `return`, `yield` does not permanently terminate the function at that point.

It pauses the generator.

When the next value is requested, execution **resumes from where it stopped**.

---

# 7. `yield` vs `return`

This distinction is extremely important.

## `return`

```python
def example():
    return 1
    return 2
```

The function ends at the first `return`.

```python
print(example())
```

Output:

```text
1
```

`return`:

> **ends the function and sends back a final result.**

---

## `yield`

```python
def example():
    yield 1
    yield 2
```

The function produces values one at a time.

```python
result = example()

print(next(result))
print(next(result))
```

Output:

```text
1
2
```

`yield`:

> **produces a value and pauses the function so it can continue later.**

### Comparison

| `return`                 | `yield`                                   |
| ------------------------ | ----------------------------------------- |
| Ends function            | Pauses generator                          |
| Returns a final result   | Produces a value                          |
| Normal function          | Generator function                        |
| Function called normally | Execution resumes with `next()`/iteration |
| Usually one final result | Can produce many values                   |

---

# 8. Generator Execution Flow ⭐

This is the most important generator concept.

Consider:

```python
def numbers():
    print("Start")
    yield 1

    print("Middle")
    yield 2

    print("End")
```

Now:

```python
gen = numbers()
```

At this point:

```text
Nothing inside the function has executed yet.
```

Now:

```python
print(next(gen))
```

Execution starts:

```text
"Start"
   ↓
yield 1
   ↓
pause
```

Output:

```text
Start
1
```

Now:

```python
print(next(gen))
```

Execution resumes from where it paused:

```text
"Middle"
   ↓
yield 2
   ↓
pause
```

Output:

```text
Middle
2
```

Now:

```python
next(gen)
```

resumes again:

```text
"End"
   ↓
function finishes
   ↓
StopIteration
```

### Visualize it like this

```text
gen = numbers()
       ↓
function paused before execution


next(gen)
       ↓
Start
       ↓
yield 1
       ↓
PAUSE


next(gen)
       ↓
resume
       ↓
Middle
       ↓
yield 2
       ↓
PAUSE


next(gen)
       ↓
resume
       ↓
End
       ↓
function finishes
       ↓
StopIteration
```

This **pause → resume → pause → resume** behavior is the heart of generators.

---

# 9. Lazy Evaluation ⭐

**Lazy evaluation** means a value is produced **only when it is needed**, rather than calculating everything immediately.

Example:

```python
def numbers():
    for i in range(5):
        yield i
```

When you create:

```python
gen = numbers()
```

Python does not generate all five values immediately.

Values are produced as you request them:

```python
next(gen)  # 0
next(gen)  # 1
next(gen)  # 2
```

### Compare with a list

List:

```python
numbers = [i for i in range(5)]
```

All values are created immediately.

Generator:

```python
numbers = (i for i in range(5))
```

Values are produced when needed.

### Mental model

```text
List
→ "Give me everything now."

Generator
→ "Give me the next value when I need it."
```

---

# 10. Memory Efficiency ⭐

Generators can be much more **memory-efficient** when dealing with large sequences because they don't need to store all generated values at once.

Consider:

```python
numbers = [x * 2 for x in range(1_000_000)]
```

This creates a large list containing all results.

A generator:

```python
numbers = (x * 2 for x in range(1_000_000))
```

produces values lazily.

Conceptually:

```text
List
┌─────────────────────────────┐
│ all 1,000,000 results       │
│ stored in memory            │
└─────────────────────────────┘


Generator
┌──────────┐
│ next item│ → process
└──────────┘
       ↓
   next item
       ↓
   next item
       ↓
      ...
```

### Important nuance

Generators don't magically make every operation faster.

Their main advantage is often:

> **avoiding the need to keep all results in memory at once.**

---

# 11. Generator Expressions

A **generator expression** is similar to a list comprehension, but uses parentheses.

List comprehension:

```python
squares = [x ** 2 for x in range(10)]
```

Generator expression:

```python
squares = (x ** 2 for x in range(10))
```

Difference:

```text
[ ] → creates list immediately
( ) → creates generator
```

Example:

```python
gen = (x ** 2 for x in range(5))

print(next(gen))
print(next(gen))
```

Output:

```text
0
1
```

You can also iterate over it:

```python
for value in gen:
    print(value)
```

---

# 12. Practical Use Cases

## Use Case 1 — Large Data

Suppose you need to process a huge sequence of numbers.

Instead of:

```python
numbers = [process(x) for x in huge_dataset]
```

you can use:

```python
numbers = (process(x) for x in huge_dataset)
```

and process values one at a time.

---

# Use Case 2 — Reading Large Files

Instead of loading an entire large file into memory:

```python
with open("large_file.txt") as file:
    for line in file:
        process(line)
```

File objects themselves are iterable and support iteration over lines.

For custom processing, you can also create a generator:

```python
def read_lines(file):
    with open(file) as f:
        for line in f:
            yield line.strip()
```

Then:

```python
for line in read_lines("large_file.txt"):
    print(line)
```

This allows the processing to happen incrementally.

---

# Use Case 3 — Data Pipelines

Generators are useful when data passes through multiple processing stages.

```python
def numbers():
    for i in range(10):
        yield i


def squares(values):
    for value in values:
        yield value ** 2


def even(values):
    for value in values:
        if value % 2 == 0:
            yield value
```

You can connect them:

```python
data = numbers()
data = squares(data)
data = even(data)

for value in data:
    print(value)
```

The values can flow through the pipeline **one at a time**.

Conceptually:

```text
Source
  ↓
Transform
  ↓
Filter
  ↓
Output
```

This pattern is particularly useful in data-processing systems.

---

# Use Case 4 — Infinite or Very Large Sequences

Generators can represent sequences that don't have a practical end.

```python
def counter():
    number = 0

    while True:
        yield number
        number += 1
```

Now:

```python
gen = counter()

print(next(gen))
print(next(gen))
print(next(gen))
```

Output:

```text
0
1
2
```

The generator doesn't need to create an infinite list in memory.

### ⚠️ Important

Don't do:

```python
list(counter())
```

because the generator is infinite.

---

# 13. Creating Your Own Iterator

You can create an iterator manually using a class.

```python
class Count:
    def __init__(self, max_value):
        self.current = 1
        self.max_value = max_value

    def __iter__(self):
        return self

    def __next__(self):
        if self.current > self.max_value:
            raise StopIteration

        value = self.current
        self.current += 1
        return value
```

Usage:

```python
counter = Count(3)

print(next(counter))
print(next(counter))
print(next(counter))
```

Output:

```text
1
2
3
```

After that:

```python
next(counter)
```

raises:

```text
StopIteration
```

### Why generators are popular

The same behavior can be written much more simply:

```python
def count(max_value):
    for number in range(1, max_value + 1):
        yield number
```

This is one of the major benefits of generators:

> **Generators provide a convenient way to implement the iterator protocol.**

You don't need to manually write `__iter__()` and `__next__()` in many cases.

---

# 14. Iterable → Iterator → Generator

Keep these concepts separate.

### Iterable

```python
numbers = [1, 2, 3]
```

You can iterate over it.

### Iterator

```python
iterator = iter(numbers)
```

It produces values using:

```python
next(iterator)
```

### Generator

```python
def numbers():
    yield 1
    yield 2
    yield 3
```

Calling:

```python
gen = numbers()
```

creates a generator object.

A generator is an **iterator**.

So:

```text
Iterable
   ↓ iter()
Iterator
   ↓
next()
```

and:

```text
Generator function
   ↓ yield
Generator object
   ↓
Iterator
```

---

# ⭐ Important Points

1. **Iterable** = an object you can get an iterator from.
2. **Iterator** = an object that produces values one at a time.
3. `iter()` gets an iterator from an iterable.
4. `next()` requests the next value from an iterator.
5. `StopIteration` signals that there are no more values.
6. `for` loops use the iterator protocol internally.
7. A **generator** is a special type of iterator.
8. Generator functions use `yield`.
9. `yield` pauses execution and allows it to resume later.
10. `return` ends a function.
11. Generators use **lazy evaluation**.
12. Lazy evaluation can reduce memory usage for large sequences.
13. Generator expressions use:

```python
(expression for item in iterable)
```

14. Generators are useful for:

* large datasets
* files
* streams
* data pipelines
* incremental processing
* potentially infinite sequences

---

# ⚠️ Common Confusions / Traps

### 1. Iterable ≠ Iterator

A list is iterable:

```python
numbers = [1, 2, 3]
```

But:

```python
next(numbers)
```

doesn't work.

You first need:

```python
iterator = iter(numbers)
next(iterator)
```

---

### 2. A generator function is not the same as a generator object

This:

```python
def numbers():
    yield 1
```

is a **generator function**.

This:

```python
gen = numbers()
```

creates a **generator object**.

---

### 3. Calling a generator function doesn't execute it normally

```python
gen = numbers()
```

creates the generator.

The function body starts executing when you request a value:

```python
next(gen)
```

or iterate over it:

```python
for value in gen:
    ...
```

---

### 4. `yield` does not mean "return and finish"

```python
def numbers():
    yield 1
    yield 2
```

After:

```python
next(gen)
```

the generator pauses.

It can resume later.

---

### 5. Generators are generally consumed

For example:

```python
gen = (x for x in range(3))

print(list(gen))
print(list(gen))
```

Output:

```text
[0, 1, 2]
[]
```

Once the generator has been exhausted, there are no values left.

If you need to iterate again, create a new generator.

---

# 🎯 Interview Answer

> **What is the difference between an iterable and an iterator?**

"An iterable is an object that can provide an iterator, while an iterator is an object that produces values one at a time using `__next__()`. `iter()` can be used to obtain an iterator from an iterable, and `next()` retrieves the next value."

> **What is a generator?**

"A generator is a special type of iterator that produces values lazily, usually using the `yield` keyword. It pauses execution at each `yield` and resumes when the next value is requested."

> **`yield` vs `return`?**

"`return` ends a function and sends back a final result, whereas `yield` produces a value and pauses a generator so execution can resume later."

> **Why use generators?**

"Generators allow values to be processed lazily, which can reduce memory usage when working with large datasets, files, streams, or pipelines."

---

# 🤖 Backend / GenAI Relevance ⭐

This topic is **highly relevant** to your eventual backend and AI work.

### 1. Large Data Processing

You may process:

```text
documents
   ↓
chunks
   ↓
embeddings
   ↓
results
```

Instead of loading everything into memory at once, generators can allow incremental processing.

---

### 2. Streaming

The fundamental idea behind generators—**produce data incrementally rather than all at once**—is closely related to streaming patterns.

For example, an AI application may receive output incrementally:

```text
chunk 1
   ↓
chunk 2
   ↓
chunk 3
   ↓
chunk 4
   ↓
...
```

Generators themselves aren't the same thing as network/LLM streaming, but the **one-item-at-a-time processing model** is highly relevant.

---

### 3. Data Pipelines

A pipeline might look like:

```text
Load documents
      ↓
Clean
      ↓
Chunk
      ↓
Transform
      ↓
Embed
      ↓
Store
```

Generators can help build memory-efficient stages where data flows through incrementally.

---

### 4. Backend APIs

When working with large responses or streaming data, understanding:

```text
iterator
   ↓
next value
   ↓
process
   ↓
next value
```

helps you understand why streaming-oriented APIs often don't behave like ordinary lists.

---

# 📌 Priority

| Topic                                  | Priority           |
| -------------------------------------- | ------------------ |
| Iterable vs Iterator                   | 🔥 Core            |
| `iter()`                               | 🔥 Core            |
| `next()`                               | 🔥 Core            |
| `StopIteration`                        | 🔥 Core            |
| What is a Generator?                   | 🔥 Core            |
| `yield`                                | 🔥 Core            |
| `yield` vs `return`                    | 🔥 Core            |
| Generator execution flow               | 🔥 Core            |
| Lazy evaluation                        | 🔥 Core            |
| Memory efficiency                      | 🔥 Core            |
| Generator expressions                  | 🔥 Core            |
| Practical use cases                    | 🔥 Core            |
| Manually implementing iterator classes | 🟡 Know & Move On  |
| Advanced iterator protocol details     | ⚪ Optional for Now |

### 🎯 The mental model you should remember

```text
ITERABLE
    │
    │ iter()
    ↓
ITERATOR
    │
    │ next()
    ↓
VALUE
    │
    │ next()
    ↓
VALUE
    │
    ↓
...
    │
    ↓
StopIteration
```

And for generators:

```text
Generator function
       │
       │ yield
       ↓
Generator object
       │
       │ next()
       ↓
produce value
       │
       ↓
pause
       │
       │ next()
       ↓
resume
       │
       ↓
produce next value
```

If you understand **`iter()` → `next()` → `StopIteration`** and **`yield` → pause → resume → lazy evaluation**, you have the foundation you need.

**This topic deserves more attention than recursion for your Python roadmap**, because iterators and generators show up naturally in real Python code and become especially useful when you start working with backend data processing and AI/LLM streaming patterns.
