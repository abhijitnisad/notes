# Higher-Order Functions ⭐ 🔥 Core

A **higher-order function (HOF)** is a function that does at least one of these:

1. **Accepts another function as an argument**
2. **Returns another function as its result**

This is possible because Python treats functions as **first-class objects**.

---

# 67. What is a Higher-Order Function?

A higher-order function is simply a function that **works with other functions**.

```python
def greet():
    print("Hello")


def execute(func):
    func()


execute(greet)
```

Output:

```text
Hello
```

Here:

```python
execute(greet)
```

passes the function `greet` to `execute()`.

Therefore, `execute()` is a **higher-order function**.

### Mental Model

```text
Function
   ↓
accepts another function
   ↓
Higher-Order Function
```

Or:

```text
Function
   ↓
returns another function
   ↓
Higher-Order Function
```

---

# 68. Function Accepting Another Function

Because functions are objects, we can pass them as arguments.

Example:

```python
def square(x):
    return x * x


def apply_function(func, value):
    return func(value)


result = apply_function(square, 5)

print(result)
```

Output:

```text
25
```

### How it works

First:

```python
square
```

refers to the function object.

Then:

```python
apply_function(square, 5)
```

passes that function into `apply_function()`.

Inside:

```python
return func(value)
```

`func` refers to `square`.

So Python effectively executes:

```python
square(5)
```

and returns:

```text
25
```

---

## ⚠️ `func` vs `func()`

This is extremely important.

### Passing the function

```python
apply_function(square, 5)
```

Here:

```python
square
```

means:

> "Pass the function itself."

### Calling the function first

```python
apply_function(square(5), 5)
```

This is completely different.

```python
square(5)
```

executes immediately and produces:

```text
25
```

So you would effectively be passing:

```python
apply_function(25, 5)
```

which is not what we want.

### Remember

```text
square     → function object
square()   → result of calling the function
```

---

# Common Built-in Higher-Order Functions

Python already provides several functions that accept other functions.

## `map()`

```python
numbers = [1, 2, 3, 4]

result = map(lambda x: x * 2, numbers)

print(list(result))
```

Output:

```text
[2, 4, 6, 8]
```

`map()` accepts:

```python
lambda x: x * 2
```

as an argument.

Therefore, `map()` is a higher-order function.

---

## `filter()`

```python
numbers = [1, 2, 3, 4, 5, 6]

result = filter(lambda x: x % 2 == 0, numbers)

print(list(result))
```

Output:

```text
[2, 4, 6]
```

`filter()` accepts a function that determines whether each item should be kept.

---

## `sorted()`

```python
students = [
    ("Rahul", 85),
    ("Aman", 92),
    ("Priya", 78)
]

result = sorted(
    students,
    key=lambda student: student[1]
)

print(result)
```

`sorted()` accepts the `key` function.

So `sorted()` is also a higher-order function.

---

# 69. Function Returning Another Function

A higher-order function can also **return a function**.

Example:

```python
def create_greeting():
    
    def greet():
        print("Hello")

    return greet
```

Now:

```python
message = create_greeting()

message()
```

Output:

```text
Hello
```

What happened?

```text
create_greeting()
       ↓
creates greet()
       ↓
returns greet
       ↓
message refers to greet
       ↓
message()
```

Since `create_greeting()` returns a function, it is a **higher-order function**.

---

# Function Factory Example

A very useful pattern is creating customized functions.

```python
def multiplier(factor):

    def multiply(number):
        return number * factor

    return multiply
```

Now:

```python
double = multiplier(2)
triple = multiplier(3)

print(double(10))
print(triple(10))
```

Output:

```text
20
30
```

### What's happening?

First:

```python
double = multiplier(2)
```

creates a function that remembers:

```text
factor = 2
```

Then:

```python
triple = multiplier(3)
```

creates another function that remembers:

```text
factor = 3
```

This also demonstrates a **closure**.

So several concepts are now connected:

```text
Function Objects
       ↓
Nested Functions
       ↓
Enclosing Scope
       ↓
Closures
       ↓
Higher-Order Functions
       ↓
Decorators
```

---

# 70. Practical Examples

## Example 1 — Generic Calculator

Instead of writing separate functions for every operation:

```python
def add(a, b):
    return a + b


def multiply(a, b):
    return a * b


def calculate(func, a, b):
    return func(a, b)
```

Now:

```python
print(calculate(add, 10, 5))
print(calculate(multiply, 10, 5))
```

Output:

```text
15
50
```

`calculate()` doesn't need to know **which operation** to perform.

It receives the operation as a function.

### Mental model

```text
calculate()
    +
    ├── add      → addition
    └── multiply → multiplication
```

This makes code more flexible.

---

# Example 2 — Using Lambda

You don't always need to define a named function.

```python
def calculate(func, value):
    return func(value)


print(calculate(lambda x: x ** 2, 5))
print(calculate(lambda x: x + 10, 5))
```

Output:

```text
25
15
```

Here `calculate()` receives different behavior each time.

---

# Example 3 — Function Factory

```python
def power(exponent):

    def calculate(number):
        return number ** exponent

    return calculate
```

Now:

```python
square = power(2)
cube = power(3)

print(square(5))
print(cube(5))
```

Output:

```text
25
125
```

The outer function creates customized functions.

This is a **function factory** and also uses a **closure**.

---

# Example 4 — Processing Data

Suppose you have:

```python
numbers = [1, 2, 3, 4, 5]
```

You want to transform every value.

```python
def process(data, func):
    return [func(item) for item in data]
```

Now:

```python
result = process(numbers, lambda x: x * 10)

print(result)
```

Output:

```text
[10, 20, 30, 40, 50]
```

The processing function doesn't care what transformation is used.

You can change the behavior:

```python
result = process(numbers, lambda x: x ** 2)

print(result)
```

Output:

```text
[1, 4, 9, 16, 25]
```

Same `process()` function, different behavior.

---

# Example 5 — Callback Pattern

A callback is a function passed to another function so that it can be called later.

```python
def success():
    print("Operation successful")


def process(callback):
    print("Processing...")
    callback()


process(success)
```

Output:

```text
Processing...
Operation successful
```

Here:

```python
process(success)
```

passes `success` as a callback.

This is another practical use of higher-order functions.

---

# Example 6 — Backend/AI-Style Configuration

Imagine a processing pipeline:

```python
def process_text(text, processor):
    return processor(text)
```

You can provide different processors:

```python
def clean_text(text):
    return text.strip().lower()


def uppercase_text(text):
    return text.upper()
```

Then:

```python
print(process_text("  Hello World  ", clean_text))
print(process_text("Hello World", uppercase_text))
```

Output:

```text
hello world
HELLO WORLD
```

The main function doesn't need to know the internal implementation of the processor.

It simply receives a function.

This idea appears frequently in software systems where behavior needs to be configurable.

---

# ⭐ Important Points

1. A **higher-order function** works with other functions.
2. It can:

   * accept a function as an argument
   * return a function
   * or do both
3. This is possible because Python functions are **first-class objects**.
4. Examples of built-in higher-order functions include:

   * `map()`
   * `filter()`
   * `sorted()`
5. A function passed into another function is often called a **callback** depending on how it is used.
6. Function factories are a common pattern where one function returns customized functions.
7. Function factories often use **closures** to remember configuration/state.

---

# ⚠️ Common Confusions / Traps

### 1. Higher-order function ≠ any function that calls another function

This alone doesn't necessarily make it a higher-order function:

```python
def calculate():
    add(10, 20)
```

The important characteristic is that the function **accepts or returns a function as a value**.

---

### 2. Don't confuse function with function call

```python
calculate(add, 10, 20)
```

passes `add` as a function.

While:

```python
calculate(add(10, 20), ...)
```

calls `add()` first and passes its result.

Remember:

```text
add     → function
add()   → result
```

---

### 3. `lambda` and higher-order functions are different concepts

A lambda is simply a way to create a small function:

```python
lambda x: x * 2
```

A higher-order function is a function that **accepts or returns functions**:

```python
def apply(func, value):
    return func(value)
```

They are often used together, but they are not the same thing.

---

# 🎯 Interview Answer

> **What is a higher-order function?**

"A higher-order function is a function that accepts another function as an argument, returns a function, or both. Python supports higher-order functions because functions are first-class objects."

### Example:

```python
def apply(func, value):
    return func(value)


def square(x):
    return x ** 2


print(apply(square, 5))
```

Output:

```text
25
```

### If asked for built-in examples:

> "`map()`, `filter()`, and `sorted()` are common examples because they accept functions as arguments."

---

# 🤖 Backend / GenAI Relevance

This concept is **more important than lambda itself**.

Higher-order functions introduce an important programming pattern:

> **Separate the operation from the mechanism that executes it.**

For example:

```python
def process(data, processor):
    return processor(data)
```

The `process()` function controls **how processing happens**, while the supplied function controls **what processing happens**.

This idea appears in:

* callbacks
* middleware
* decorators
* configurable processing pipelines
* event handlers
* framework APIs
* data transformation
* validation/processing hooks

And decorators—which you're about to learn—are heavily based on these ideas.

### The progression to remember

```text
Functions are objects
        ↓
Can pass functions around
        ↓
Higher-order functions
        ↓
Nested functions + closures
        ↓
Decorators
```

---

# 📌 Priority

| Topic                               | Priority           |
| ----------------------------------- | ------------------ |
| What is a higher-order function?    | 🔥 Core            |
| Function accepting another function | 🔥 Core            |
| Function returning another function | 🔥 Core            |
| Function factories                  | 🟡 Know & Move On  |
| Callbacks                           | 🟡 Know & Move On  |
| Advanced functional programming     | ⚪ Optional for Now |

### 🎯 What you should remember

If you remember only one definition:

> **A higher-order function is a function that accepts another function or returns another function.**

And the simplest example:

```python
def apply(func, value):
    return func(value)
```

This is the foundation for understanding **decorators**, which is the next particularly important topic.
