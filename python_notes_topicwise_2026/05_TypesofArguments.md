# Types of Arguments — 🔥 Core

Python provides different ways to pass values to a function. The important categories are **positional, keyword, default, and variable-length arguments**.

---

## 1. Positional Arguments

Arguments are matched with parameters based on their **position/order**.

```python
def introduce(name, age):
    print(name, age)

introduce("Abhijit", 21)
```

Mapping:

```text
name → "Abhijit"
age  → 21
```

The order matters.

```python
introduce(21, "Abhijit")
```

This is valid Python, but the values are assigned to the wrong parameters for the intended meaning.

---

## 2. Keyword Arguments

Arguments are passed using the **parameter name**.

```python
def introduce(name, age):
    print(name, age)

introduce(name="Abhijit", age=21)
```

The order doesn't matter:

```python
introduce(age=21, name="Abhijit")
```

Both calls produce the same result.

### Advantage

Keyword arguments make function calls more explicit and readable, especially when there are many parameters.

---

## 3. Default Arguments

A parameter can have a **default value** that is used when the caller doesn't provide an argument.

```python
def greet(name, message="Hello"):
    print(f"{message}, {name}")

greet("Abhijit")
```

Output:

```text
Hello, Abhijit
```

If a value is provided, it replaces the default:

```python
greet("Abhijit", "Good morning")
```

Output:

```text
Good morning, Abhijit
```

### ⚠️ Important

Required parameters must generally come before parameters with defaults:

```python
def greet(name, message="Hello"):
    ...
```

Not:

```python
def greet(message="Hello", name):
    ...
```

---

# 4. Variable-Length Arguments

Sometimes we don't know beforehand how many arguments a function will receive.

Python provides:

```text
*args   → variable number of positional arguments
**kwargs → variable number of keyword arguments
```

These allow functions to accept a flexible number of arguments.

---

# 5. `*args`

`*args` collects extra **positional arguments** into a **tuple**.

```python
def add(*args):
    print(args)

add(10, 20, 30)
```

Output:

```text
(10, 20, 30)
```

Inside the function:

```python
args
```

is a tuple.

We can iterate over it:

```python
def add(*args):
    total = 0

    for number in args:
        total += number

    return total

print(add(10, 20, 30))
```

Output:

```text
60
```

### Important

The name `args` is not special.

This also works:

```python
def add(*numbers):
    ...
```

The `*` is what tells Python to collect positional arguments.

---

# 6. `**kwargs`

`**kwargs` collects extra **keyword arguments** into a **dictionary**.

```python
def show_user(**kwargs):
    print(kwargs)

show_user(name="Abhijit", age=21)
```

Output:

```text
{'name': 'Abhijit', 'age': 21}
```

Inside the function:

```python
kwargs
```

is a dictionary.

We can access values normally:

```python
def show_user(**kwargs):
    print(kwargs["name"])
    print(kwargs["age"])

show_user(name="Abhijit", age=21)
```

### Again, `kwargs` is just a convention.

This is also valid:

```python
def show_user(**data):
    ...
```

The `**` is what provides the special behavior.

---

# 7. `*args` vs `**kwargs`

| `*args`                       | `**kwargs`                 |
| ----------------------------- | -------------------------- |
| Collects positional arguments | Collects keyword arguments |
| Creates a tuple               | Creates a dictionary       |
| `func(10, 20)`                | `func(a=10, b=20)`         |
| Accessed by position          | Accessed by key            |

Easy memory:

```text
*args   → positional → tuple
**kwargs → keyword   → dictionary
```

---

# 8. Combining Normal Parameters, `*args`, and `**kwargs`

A function can combine regular parameters with `*args` and `**kwargs`.

```python
def example(name, age, *args, **kwargs):
    print(name)
    print(age)
    print(args)
    print(kwargs)

example(
    "Abhijit",
    21,
    "Python",
    "AI",
    city="Bhopal",
    role="Developer"
)
```

Conceptually:

```text
name   → "Abhijit"
age    → 21
args   → ("Python", "AI")
kwargs → {"city": "Bhopal", "role": "Developer"}
```

### Typical order

When all are present:

```python
def function(normal, *args, **kwargs):
    ...
```

Normal parameters come first, followed by `*args`, then `**kwargs`.

---

# 9. Argument Unpacking with `*`

The `*` has another important use.

It can **unpack an iterable into individual positional arguments** when calling a function.

Suppose:

```python
def add(a, b, c):
    return a + b + c

numbers = [10, 20, 30]

print(add(*numbers))
```

This is equivalent to:

```python
add(10, 20, 30)
```

The `*` takes the elements from the list and passes them as separate positional arguments.

### Mental model

```text
numbers = [10, 20, 30]

       *
       ↓
10    20    30
 ↓     ↓     ↓
a     b     c
```

This is called **iterable unpacking**.

It works with other iterables too:

```python
numbers = (10, 20, 30)

add(*numbers)
```

---

# 10. Dictionary Unpacking with `**`

`**` can unpack a dictionary into **keyword arguments**.

```python
def introduce(name, age):
    print(name, age)

user = {
    "name": "Abhijit",
    "age": 21
}

introduce(**user)
```

This is equivalent to:

```python
introduce(name="Abhijit", age=21)
```

The dictionary keys must correspond to valid parameter names expected by the function.

### Mental model

```text
user = {
    "name": "Abhijit",
    "age": 21
}

          **
          ↓
name="Abhijit"
age=21
```

---

# 11. `*` vs `**` — Two Different Contexts

This is an important point.

### In a function definition

```python
def func(*args, **kwargs):
    ...
```

`*args` **collects** arguments.

### In a function call

```python
func(*values, **data)
```

`*` and `**` **unpack** arguments.

So:

```text
Definition:
*args   → collect positional arguments
**kwargs → collect keyword arguments

Call:
*value  → unpack iterable
**dict  → unpack dictionary
```

---

# ⭐ Important Points

* **Positional arguments** are matched by order.
* **Keyword arguments** are matched by parameter name.
* **Default arguments** provide fallback values.
* `*args` collects extra positional arguments into a **tuple**.
* `**kwargs` collects extra keyword arguments into a **dictionary**.
* `args` and `kwargs` are conventional names; the `*` and `**` provide the behavior.
* `*` during a function call performs **iterable unpacking**.
* `**` during a function call performs **dictionary/keyword unpacking**.
* In a typical flexible function:

```python
def func(normal, *args, **kwargs):
    ...
```

* In a function definition → `*`/`**` **collect**.
* In a function call → `*`/`**` **unpack**.

---

# ⚠️ Common Confusion

Don't confuse these:

```python
def func(*args):
    ...
```

and:

```python
func(*values)
```

The first **collects** positional arguments.

The second **unpacks** an iterable into positional arguments.

Similarly:

```python
def func(**kwargs):
    ...
```

collects keyword arguments, while:

```python
func(**data)
```

unpacks a dictionary into keyword arguments.

---

# 🎯 Interview Answer

**Q: What are `*args` and `**kwargs`?**

> **`*args` allows a function to accept a variable number of positional arguments and collects them into a tuple. `**kwargs` allows a function to accept a variable number of keyword arguments and collects them into a dictionary. They are useful when a function needs to support a flexible number of arguments.**

**Q: What is argument unpacking?**

> **Argument unpacking allows an iterable to be expanded into positional arguments using `*`, and a dictionary to be expanded into keyword arguments using `**`. For example, `func(*values)` passes the elements of `values` as separate positional arguments, while `func(**data)` passes dictionary entries as keyword arguments.**

---

# 🤖 Backend / AI Relevance

🔥 **Very useful**

You'll encounter these patterns in:

* decorators
* wrapper functions
* framework APIs
* FastAPI
* Python libraries
* SDKs
* configuration handling
* AI/LLM libraries

A particularly important future pattern is:

```python
def wrapper(*args, **kwargs):
    return original_function(*args, **kwargs)
```

This allows a wrapper to accept and forward **almost any arguments** to another function.

That pattern will become important when you learn **decorators**.

---

## Priority

🔥 **Deeply understand**

* Positional vs keyword arguments
* Default arguments
* `*args`
* `**kwargs`
* `*` unpacking
* `**` dictionary unpacking
* Collect vs unpack distinction

🟡 **Know & Move On**

* Complex combinations of every possible argument type

You should be able to look at a function using `*args`/`**kwargs` and immediately understand **what is being collected or unpacked and what type of object is involved**.
