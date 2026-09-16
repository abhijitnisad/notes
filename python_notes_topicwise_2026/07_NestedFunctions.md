# Nested Functions ⭐ 🔥 Core

## 53. What is a Nested Function?

A **nested function** is a function defined **inside another function**.

The outer function contains the inner function.

```python
def outer():
    def inner():
        print("Hello from inner")

    inner()

outer()
```

Here:

* `outer()` → outer function
* `inner()` → nested/inner function
* `inner()` can normally be used directly inside `outer()`

### Why use nested functions?

Nested functions are useful when a function is needed only **inside another function**.

They help with:

* keeping helper logic local
* controlling scope
* closures
* decorators
* callbacks and function factories

---

# 54. Function Inside Another Function

A nested function is simply a function definition inside another function.

```python
def calculate():
    
    def add(a, b):
        return a + b
    
    result = add(10, 20)
    return result

print(calculate())
```

Output:

```text
30
```

`add()` is local to `calculate()`.

You generally cannot access it directly from outside:

```python
add(10, 20)   # NameError
```

because `add` belongs to the local scope created by `calculate()`.

### Important

The inner function can access variables from its outer function.

```python
def outer():
    message = "Hello"

    def inner():
        print(message)

    inner()

outer()
```

Output:

```text
Hello
```

Why?

Because `message` is not local to `inner()`, so Python looks in the **enclosing scope**.

---

# 55. Enclosing Scope

The **enclosing scope** is the scope of an outer function surrounding a nested function.

This is part of Python's **LEGB** name-resolution rule:

```text
L → Local
E → Enclosing
G → Global
B → Built-in
```

Example:

```python
def outer():
    message = "Hello"

    def inner():
        print(message)

    inner()

outer()
```

When Python evaluates:

```python
print(message)
```

inside `inner()`:

1. Look in `inner()`'s local scope → not found
2. Look in `outer()`'s enclosing scope → found
3. Use `"Hello"`

So:

```text
inner()
   ↓
Local scope
   ↓
Enclosing scope (outer)
   ↓
Global scope
   ↓
Built-in scope
```

### Important distinction

**Enclosing scope is not the same as global scope.**

```python
message = "Global"

def outer():
    message = "Enclosing"

    def inner():
        print(message)

    inner()

outer()
```

Output:

```text
Enclosing
```

`inner()` finds `message` in the enclosing `outer()` scope before reaching the global scope.

---

# 56. `nonlocal`

The `nonlocal` keyword is used inside a nested function when you want to **modify a variable belonging to an enclosing function**.

Example:

```python
def outer():
    count = 0

    def inner():
        nonlocal count
        count += 1
        print(count)

    inner()
    inner()

outer()
```

Output:

```text
1
2
```

Without `nonlocal`:

```python
def outer():
    count = 0

    def inner():
        count += 1
```

Python treats `count` as a **local variable of ****`inner()`** because you are assigning to it.

But `count` does not yet have a local value.

This results in:

```text
UnboundLocalError
```

### `nonlocal` tells Python:

> "Don't create a new local `count`. Use the `count` variable from the enclosing function."

---

## `nonlocal` vs `global`

| Keyword    | Modifies variable in     |
| ---------- | ------------------------ |
| `nonlocal` | Enclosing function scope |
| `global`   | Module/global scope      |

Example:

```python
x = 10

def outer():
    x = 20

    def inner():
        nonlocal x
        x = 30

    inner()
    print(x)

outer()
```

Output:

```text
30
```

Here `nonlocal x` modifies `outer()`'s `x`.

---

# 57. Closures ⭐ 🔥 Core

A **closure** occurs when an inner function **remembers and can access variables from its enclosing function even after the enclosing function has finished executing**.

This is the key idea:

> **A closure is a function together with the enclosed environment it remembers.**

Example:

```python
def outer(message):

    def inner():
        print(message)

    return inner
```

Now:

```python
func = outer("Hello")

func()
```

Output:

```text
Hello
```

Notice something interesting.

`outer()` has already finished executing:

```python
func = outer("Hello")
```

Yet `func()` still remembers:

```text
message = "Hello"
```

That's a **closure**.

---

## How a Closure Works

Consider:

```python
def outer(message):

    def inner():
        print(message)

    return inner
```

Step-by-step:

### Step 1 — Call `outer()`

```python
func = outer("Hello")
```

`message` becomes:

```text
"Hello"
```

### Step 2 — `inner()` is created

`inner()` uses `message` from the enclosing scope.

### Step 3 — `outer()` returns `inner`

```python
return inner
```

Notice:

```python
return inner
```

not:

```python
return inner()
```

### Step 4 — `func` now refers to `inner`

```python
func()
```

Even though `outer()` has finished, `inner()` remembers the value of `message`.

That remembered environment is what makes it a closure.

---

# Closure Example with Different Values

```python
def create_greeting(name):

    def greet():
        print(f"Hello, {name}")

    return greet


greet_abhijit = create_greeting("Abhijit")
greet_rahul = create_greeting("Rahul")

greet_abhijit()
greet_rahul()
```

Output:

```text
Hello, Abhijit
Hello, Rahul
```

Each returned function remembers its own enclosing value.

Conceptually:

```text
create_greeting("Abhijit")
        ↓
   greet function
        ↓
 remembers name = "Abhijit"


create_greeting("Rahul")
        ↓
   greet function
        ↓
 remembers name = "Rahul"
```

This is one reason closures are powerful.

---

# Closure + `nonlocal`

Closures can also maintain and update state.

```python
def counter():

    count = 0

    def increment():
        nonlocal count
        count += 1
        return count

    return increment
```

Now:

```python
c = counter()

print(c())
print(c())
print(c())
```

Output:

```text
1
2
3
```

The `count` variable continues to exist through the closure.

The important relationship is:

```text
Nested function
      ↓
Enclosing variable
      ↓
Closure remembers it
      ↓
nonlocal can modify it
```

---

# 58. Why Are Closures Useful?

Closures are useful when you want a function to **remember some state without using a global variable or creating a class**.

## 1. Maintaining State

```python
def counter():
    count = 0

    def increment():
        nonlocal count
        count += 1
        return count

    return increment
```

The returned function remembers `count`.

---

## 2. Data/State Encapsulation

A closure can keep data inside the enclosing function so that outside code cannot directly access the variable.

```python
def account():
    balance = 1000

    def get_balance():
        return balance

    return get_balance
```

The `balance` variable is not directly exposed.

---

## 3. Function Factories

A function can create customized functions.

```python
def multiplier(factor):

    def multiply(number):
        return number * factor

    return multiply
```

Usage:

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

Here `multiplier()` acts like a **function factory**.

---

## 4. Used in Decorators ⭐

Closures are one of the fundamental concepts behind Python decorators.

For example:

```python
def decorator(func):

    def wrapper():
        print("Before function")
        func()
        print("After function")

    return wrapper
```

The `wrapper()` function remembers `func`.

That is closure behavior.

So understanding:

```text
Nested functions
        ↓
Enclosing scope
        ↓
Closures
        ↓
Decorators
```

is very important.

---

# ⭐ Important Points

1. A **nested function** is a function defined inside another function.
2. The inner function can access variables from its **enclosing function**.
3. Enclosing scope is part of the **LEGB** rule.
4. `nonlocal` allows a nested function to modify a variable from an enclosing function.
5. A **closure** is a function that retains access to variables from its enclosing scope even after the outer function has finished.
6. Closures are useful for:

   * maintaining state
   * encapsulation
   * function factories
   * decorators
   * callbacks
7. `nonlocal` is for an enclosing function's variable; `global` is for a module/global variable.

---

# ⚠️ Common Confusions / Traps

### 1. Nested function ≠ automatically a closure

This is nested:

```python
def outer():

    def inner():
        print("Hello")

    inner()
```

But `inner()` does not capture any enclosing variable.

A closure specifically involves the inner function **retaining access to an enclosing variable**.

---

### 2. `return inner` vs `return inner()`

```python
return inner
```

Returns the **function object**.

```python
return inner()
```

Calls the function and returns **its result**.

This distinction is extremely important.

---

### 3. `nonlocal` cannot access a global variable

This is incorrect:

```python
x = 10

def inner():
    nonlocal x
```

`nonlocal` requires a variable in an **enclosing function scope**.

For a global variable, use:

```python
global x
```

---

### 4. `nonlocal` is not required just to read

This works:

```python
def outer():
    x = 10

    def inner():
        print(x)

    inner()
```

You need `nonlocal` when you want to **assign/modify** the enclosing variable:

```python
def outer():
    x = 10

    def inner():
        nonlocal x
        x += 1
```

---

# 🎯 Interview Answer

> **What is a nested function?**

"A nested function is a function defined inside another function. The inner function can access variables from its enclosing function's scope."

> **What is a closure?**

"A closure is a function that retains access to variables from its enclosing scope even after the enclosing function has finished execution. Closures are useful for maintaining state, encapsulation, function factories, and are an important concept behind decorators."

> **What is ****`nonlocal`****?**

"`nonlocal` is used inside a nested function to modify a variable belonging to an enclosing function scope."

---

# 🤖 Backend / GenAI Relevance

These concepts become particularly useful as you move toward backend and AI development.

### Closures → Decorators

Decorators are heavily used in Python backend frameworks and libraries.

```text
Nested functions
      ↓
Closures
      ↓
Decorators
      ↓
Framework functionality
```

### Closures → State/Configuration

Function factories can create functions configured for a particular behavior.

```python
def create_processor(model_name):

    def process(data):
        print(model_name, data)

    return process
```

This pattern can be useful for creating configurable behavior.

### Most importantly

You don't need to memorize complicated closure tricks.

Understand this mental model:

> **An inner function can remember variables from its outer function.**

Once that is clear, decorators become much easier to understand.

---

# 📌 Priority

| Topic                   | Priority          |
| ----------------------- | ----------------- |
| Nested functions        | 🔥 Core           |
| Enclosing scope         | 🔥 Core           |
| `nonlocal`              | 🔥 Core           |
| Closures                | 🔥 Core           |
| Why closures are useful | 🔥 Core           |
| Advanced closure tricks | 🟡 Know & Move On |

**Do not spend time solving lots of closure problems.** Understand the concept well and be able to explain and write a simple closure. Your next major concept after this should be **lambda functions → higher-order functions → decorators**, where these ideas start coming together.
