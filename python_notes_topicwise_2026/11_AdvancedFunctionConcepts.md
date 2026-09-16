# Advanced Function Concepts ⭐ 🔥 Core / 🟡 Know & Move On

These concepts make your functions more **readable, self-documenting, maintainable, and API-friendly**.

---

# 82. Function Annotations

**Function annotations** allow you to attach additional information to parameters and the return value of a function.

Example:

```python
def add(a: int, b: int) -> int:
    return a + b
```

Here:

```python
a: int
b: int
```

are **parameter annotations**.

And:

```python
-> int
```

is a **return annotation**.

### Important

Annotations by themselves **do not enforce types at runtime**.

This is valid:

```python
def add(a: int, b: int) -> int:
    return a + b

print(add("Hello", "World"))
```

Python doesn't automatically reject it just because `a` and `b` were annotated as `int`.

Output:

```text
HelloWorld
```

The annotations primarily provide information to:

* developers
* IDEs
* type checkers
* documentation tools
* frameworks/libraries

---

## Annotations Are Stored

Python stores function annotations and makes them accessible through:

```python
__annotations__
```

Example:

```python
def add(a: int, b: int) -> int:
    return a + b

print(add.__annotations__)
```

Output will be conceptually:

```python
{'a': <class 'int'>, 'b': <class 'int'>, 'return': <class 'int'>}
```

This connects annotations with **function introspection**, which we'll see later.

---

# 83. Type Hints ⭐ 🔥 Core

**Type hints** are annotations used to communicate the expected types of values.

Example:

```python
def greet(name: str) -> str:
    return f"Hello, {name}"
```

This communicates:

```text
name   → expected to be a string
return → expected to be a string
```

### Why use type hints?

They make code easier to:

* understand
* maintain
* refactor
* autocomplete in IDEs
* analyze with static type checkers
* work on in teams

---

## Variables Can Also Have Type Hints

```python
name: str = "Abhijit"
age: int = 22
scores: list[int] = [80, 90, 95]
```

Type hints aren't limited to functions.

---

## Type Hints with Collections

Modern Python supports syntax such as:

```python
def calculate(scores: list[int]) -> float:
    return sum(scores) / len(scores)
```

This means:

```text
scores → list containing integers
return → float
```

For dictionaries:

```python
def get_user() -> dict[str, str]:
    return {
        "name": "Abhijit",
        "city": "Bhopal"
    }
```

---

## Optional Values

If a value can be either a string or `None`:

```python
def find_name(user_id: int) -> str | None:
    ...
```

This means:

```text
return value → str OR None
```

The `|` union syntax is available in modern Python versions.

---

## Type Hints Do Not Automatically Validate Input

This is a very important interview point.

```python
def square(x: int) -> int:
    return x * x
```

The annotation doesn't force callers to pass an integer.

Type checking can be performed by external/static tools, while some frameworks can also use annotations at runtime for their own purposes.

### Mental Model

```text
Type hint
    ↓
Information about expected types
    ↓
IDE / type checker / developer
    ↓
Better code quality
```

Not:

```text
Type hint
    ↓
Python automatically blocks wrong types
```

---

# 84. Docstrings ⭐ 🔥 Core

A **docstring** is a string placed immediately inside a function, class, or module to document what it does.

Example:

```python
def add(a, b):
    """Return the sum of two numbers."""
    return a + b
```

The string:

```python
"""Return the sum of two numbers."""
```

is the function's **docstring**.

---

## Accessing a Docstring

You can access it using:

```python
print(add.__doc__)
```

Output:

```text
Return the sum of two numbers.
```

You can also use:

```python
help(add)
```

---

## Good Docstring Example

```python
def calculate_average(numbers: list[int]) -> float:
    """
    Calculate the average of a list of integers.

    Args:
        numbers: List of integers.

    Returns:
        The average value.
    """
    return sum(numbers) / len(numbers)
```

Docstrings become especially valuable when functions are part of:

* libraries
* APIs
* backend services
* larger projects
* team codebases

---

## Docstring vs Comment

A **comment** explains something to the programmer reading the code:

```python
# Add all numbers
total = sum(numbers)
```

A **docstring** documents the function/class/module itself:

```python
def calculate_total(numbers):
    """Return the total of all numbers."""
    return sum(numbers)
```

Docstrings are accessible programmatically:

```python
calculate_total.__doc__
```

---

# 85. `pass`

`pass` is a statement that **does nothing**.

It is used when Python requires a statement syntactically, but you don't want to perform an action yet.

Example:

```python
def future_function():
    pass
```

Without `pass`:

```python
def future_function():
```

you would get a syntax error because the function body cannot be empty.

---

## `pass` in Classes

```python
class User:
    pass
```

This creates an empty class.

---

## `pass` in Conditions

```python
for number in numbers:
    if number < 0:
        pass
    else:
        print(number)
```

When the condition is true, nothing happens.

### `pass` vs `continue`

These are **not the same**.

`pass`:

```python
if condition:
    pass
```

means:

> Do nothing.

`continue`:

```python
if condition:
    continue
```

means:

> Skip the rest of the current loop iteration and move to the next iteration.

Example:

```python
for i in range(5):
    if i == 2:
        pass
    print(i)
```

Output:

```text
0
1
2
3
4
```

`pass` does not skip `print(i)`.

But:

```python
for i in range(5):
    if i == 2:
        continue
    print(i)
```

Output:

```text
0
1
3
4
```

### Priority

🟡 **Know & Move On**

You should understand `pass`, but there is no reason to spend much time practicing it.

---

# 86. Positional-Only Parameters `/`

Python allows you to specify that certain parameters can **only be passed positionally**.

The `/` separator is used for this.

Example:

```python
def greet(name, /):
    print(f"Hello, {name}")
```

You can call:

```python
greet("Abhijit")
```

But this is not allowed:

```python
greet(name="Abhijit")
```

because `name` is positional-only.

---

## Multiple Positional-Only Parameters

```python
def add(a, b, /):
    return a + b
```

Valid:

```python
add(10, 20)
```

Invalid:

```python
add(a=10, b=20)
```

The `/` means:

> **Parameters before `/` must be supplied positionally.**

---

# 87. Keyword-Only Parameters `*`

The `*` separator can require parameters to be supplied using their **keyword names**.

Example:

```python
def create_user(name, *, age):
    print(name, age)
```

Valid:

```python
create_user("Abhijit", age=22)
```

Invalid:

```python
create_user("Abhijit", 22)
```

because `age` is keyword-only.

The `*` means:

> **Parameters after `*` must be supplied using keywords.**

---

## Why Use Keyword-Only Parameters?

They make function calls clearer.

Compare:

```python
create_user("Abhijit", 22, True)
```

with:

```python
create_user(
    "Abhijit",
    age=22,
    active=True
)
```

The second version makes the meaning of each value much clearer.

This becomes particularly useful when functions have many optional/configuration parameters.

---

# Positional-Only + Keyword-Only Together

You can use both `/` and `*`.

```python
def example(a, b, /, c, *, d, e):
    pass
```

Rules:

```text
a, b → positional-only
c    → positional OR keyword
d, e → keyword-only
```

Valid:

```python
example(1, 2, 3, d=4, e=5)
```

Also valid:

```python
example(1, 2, c=3, d=4, e=5)
```

But:

```python
example(a=1, b=2, c=3, d=4, e=5)
```

is invalid because `a` and `b` are positional-only.

And:

```python
example(1, 2, 3, 4, 5)
```

is invalid because `d` and `e` are keyword-only.

---

# Why `/` and `*` Matter

These features are especially useful when **designing APIs**.

They let you control how users interact with your function.

For example:

```python
def connect(host, port, *, timeout=30):
    ...
```

You might want:

```python
connect("localhost", 8000, timeout=10)
```

rather than:

```python
connect("localhost", 8000, 10)
```

The keyword makes the meaning of `10` obvious.

### Priority

🟡 **Know & Move On**

Understand the syntax and be able to read it. You don't need extensive practice.

---

# 88. Function Introspection Basics

**Introspection** means examining information about an object while the program is running.

Since functions are objects, Python allows you to inspect them.

---

## `type()`

```python
def greet():
    print("Hello")

print(type(greet))
```

Output:

```text
<class 'function'>
```

---

## `__name__`

You can get a function's name:

```python
print(greet.__name__)
```

Output:

```text
greet
```

---

## `__doc__`

You can access its docstring:

```python
def greet():
    """Greet the user."""
    print("Hello")

print(greet.__doc__)
```

Output:

```text
Greet the user.
```

---

## `__annotations__`

You can inspect annotations:

```python
def add(a: int, b: int) -> int:
    return a + b

print(add.__annotations__)
```

Conceptually:

```python
{
    'a': int,
    'b': int,
    'return': int
}
```

---

# `dir()`

`dir()` shows many attributes and methods available on an object.

```python
print(dir(greet))
```

You'll see things such as:

```text
__name__
__doc__
__annotations__
...
```

You don't need to memorize the entire output.

The important idea is:

> `dir()` helps you discover what attributes an object provides.

---

# `help()`

Python's built-in `help()` provides documentation about objects.

```python
help(greet)
```

This can show information such as:

* function name
* documentation
* signature information

It is useful when exploring unfamiliar Python objects.

---

# `inspect` Module

Python also provides the `inspect` module for more detailed introspection.

For example:

```python
import inspect


def greet(name: str, age: int = 20):
    """Greet a user."""
    print(name, age)


print(inspect.signature(greet))
```

Output:

```text
(name: str, age: int = 20)
```

This is useful when you need to inspect:

* parameters
* defaults
* signatures
* source information
* callable objects

You don't need to deeply study `inspect` now.

Just know that it exists.

---

# ⭐ Important Points

1. **Function annotations** attach metadata to parameters and return values.
2. **Type hints** communicate expected types.
3. Type hints do **not automatically enforce types at runtime**.
4. **Docstrings** document functions, classes, and modules.
5. A docstring is available through `__doc__`.
6. `pass` means **do nothing**.
7. `/` makes parameters **positional-only**.
8. `*` can make parameters **keyword-only**.
9. Functions are objects, so Python allows **introspection**.
10. Useful introspection tools/attributes include:

* `type()`
* `dir()`
* `help()`
* `__name__`
* `__doc__`
* `__annotations__`
* `inspect.signature()`

---

# ⚠️ Common Confusions / Traps

### Type hints don't enforce types

```python
def add(a: int, b: int) -> int:
    return a + b
```

This does not mean Python will automatically reject:

```python
add("A", "B")
```

Type hints primarily communicate intent and support tooling.

---

### `pass` doesn't skip execution

```python
if condition:
    pass

print("Hello")
```

`print()` still executes.

Use `continue` to skip the current loop iteration.

---

### `/` and `*` have different meanings in function definitions

```python
def example(a, /, b, *, c):
    pass
```

Means:

```text
a → positional-only
b → positional or keyword
c → keyword-only
```

---

### `*args` is different from bare `*`

Don't confuse:

```python
def func(*args):
    ...
```

with:

```python
def func(*, name):
    ...
```

`*args` **collects extra positional arguments**.

Bare `*` **marks the following parameters as keyword-only**.

---

# 🎯 Interview Answer

> **What are function annotations/type hints?**

"Function annotations allow us to attach information to function parameters and return values. Type hints use these annotations to communicate expected types and help IDEs, static type checkers, and developers understand the code. They don't enforce types at runtime by default."

> **What is a docstring?**

"A docstring is a string placed inside a function, class, or module to document its purpose. It can be accessed through the object's `__doc__` attribute."

> **What does `/` mean in a function definition?**

"`/` marks parameters before it as positional-only, meaning they cannot be passed using keyword arguments."

> **What does `*` mean in a function definition?**

"A bare `*` makes all parameters after it keyword-only."

> **What is function introspection?**

"Function introspection means examining information about a function at runtime, such as its name, documentation, annotations, and signature. Python provides tools such as `dir()`, `help()`, function attributes, and the `inspect` module."

---

# 🤖 Backend / GenAI Relevance

These concepts have **different levels of importance** for your future backend/GenAI work.

### Type hints → 🔥 Very useful

Professional Python code commonly uses type hints:

```python
def generate_response(prompt: str) -> str:
    ...
```

They make larger codebases easier to understand and maintain.

They are also particularly relevant to modern Python frameworks and data-validation/modeling libraries.

---

### Docstrings → 🔥 Very useful

When building reusable backend services, utilities, or AI components, documenting functions helps other developers understand their purpose.

```python
def retrieve_documents(query: str, top_k: int = 5) -> list[str]:
    """
    Retrieve the most relevant documents for a query.
    """
    ...
```

---

### `/` and `*` → 🟡 Useful

These are primarily **API/function design tools**.

You should understand them, but they aren't something you'll use constantly.

---

### Introspection → 🟡 Useful

Introspection becomes more interesting when working with:

* decorators
* frameworks
* dynamic Python code
* libraries
* function signatures
* debugging

It's particularly useful for understanding how Python frameworks can inspect functions and their parameters.

---

# 📌 Priority

| Topic                         | Priority           |
| ----------------------------- | ------------------ |
| Function annotations          | 🔥 Core            |
| Type hints                    | 🔥 Core            |
| Docstrings                    | 🔥 Core            |
| `pass`                        | 🟡 Know & Move On  |
| Positional-only `/`           | 🟡 Know & Move On  |
| Keyword-only `*`              | 🟡 Know & Move On  |
| Function introspection basics | 🟡 Know & Move On  |
| Advanced `inspect` usage      | ⚪ Optional for Now |

### 🎯 What you should be able to write without thinking

```python
def process_text(
    text: str,
    *,
    max_length: int = 1000
) -> str:
    """Process text and return the result."""
    ...
```

You should immediately understand:

```text
text → str
max_length → keyword-only int with default 1000
return → str
docstring → documentation
```

That level of understanding is enough for now.

**The next high-value Python concepts are modules/packages and then exceptions.** After those, iterators/generators and decorators will build naturally on the function concepts you've just learned.
