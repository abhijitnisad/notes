# Decorators — Python Notes 🔥 Core

## 1. What is a Decorator?

A **decorator** is a function that **takes another function, adds or modifies its behavior, and returns a function**.

In simple words:

> **Decorator = wrap an existing function with additional behavior without changing its original code.**

For example, suppose we have:

```python
def greet():
    print("Hello")
```

We want to add logging before and after `greet()` runs.

Instead of modifying `greet()` itself, we can create a decorator.

```python
def logger(func):
    def wrapper():
        print("Function started")
        func()
        print("Function finished")

    return wrapper
```

Then:

```python
greet = logger(greet)

greet()
```

Output:

```text
Function started
Hello
Function finished
```

### Mental Model

Think of a decorator as a **wrapper around a function**:

```text
Original Function
       ↓
   Decorator
       ↓
   Wrapper
       ↓
Original Function + Extra Behavior
```

---

# 2. Why Do We Need Decorators?

Suppose you have 20 functions and want to add:

* logging
* authentication
* timing
* validation
* caching

Without decorators, you might repeat the same code inside every function.

Decorators allow you to define that behavior **once** and reuse it.

```text
Function A ──┐
Function B ──┼──→ same decorator
Function C ──┘
```

This follows the idea:

> **Don't repeat the same cross-cutting logic in every function.**

---

# 3. Functions as Objects

Decorators depend heavily on the fact that **functions are first-class objects** in Python.

A function can be:

* assigned to a variable
* passed as an argument
* returned from another function
* stored in a collection

Example:

```python
def greet():
    print("Hello")


my_function = greet

my_function()
```

Output:

```text
Hello
```

Here:

```python
my_function = greet
```

doesn't execute `greet()`.

It makes `my_function` refer to the same function object.

### Important

```python
greet     # function object
greet()   # calls the function
```

This distinction is extremely important for decorators.

---

# 4. Passing Functions as Arguments

A decorator takes a function as an argument.

```python
def decorator(func):
    print(func)
```

Example:

```python
def greet():
    print("Hello")


decorator(greet)
```

Here:

```text
greet
 ↓
passed as argument
 ↓
decorator(func)
```

The parameter `func` now refers to the `greet` function.

---

# 5. Returning Functions

A decorator usually returns another function.

```python
def decorator(func):

    def wrapper():
        print("Before")
        func()
        print("After")

    return wrapper
```

Here:

```text
decorator()
     ↓
  wrapper()
     ↓
returned
```

This works because Python allows functions to be returned like normal objects.

---

# 6. Inner / Wrapper Functions

The function returned by a decorator is commonly called a **wrapper function**.

Example:

```python
def decorator(func):

    def wrapper():
        print("Before function")
        func()
        print("After function")

    return wrapper
```

The `wrapper()` function:

1. performs additional work
2. calls the original function
3. performs additional work

Conceptually:

```text
wrapper()
   │
   ├── Before
   │
   ├── original function
   │
   └── After
```

The wrapper is usually a nested function, so it can access `func` from the enclosing scope.

This is where **closures** become important.

---

# 7. Basic Decorator Without `@`

Let's build one step by step.

### Original function

```python
def greet():
    print("Hello")
```

### Decorator

```python
def decorator(func):

    def wrapper():
        print("Before")
        func()
        print("After")

    return wrapper
```

### Apply decorator manually

```python
greet = decorator(greet)
```

Now:

```python
greet()
```

Output:

```text
Before
Hello
After
```

Notice something important:

Originally:

```python
greet
```

referred to the original function.

After:

```python
greet = decorator(greet)
```

`greet` now refers to the **wrapper function**.

The wrapper still has access to the original function through `func`.

---

# 8. `@decorator` Syntax

Python provides cleaner syntax for applying decorators.

Instead of:

```python
greet = decorator(greet)
```

we can write:

```python
@decorator
def greet():
    print("Hello")
```

Then:

```python
greet()
```

Output:

```text
Before
Hello
After
```

### Important Rule

This:

```python
@decorator
def greet():
    ...
```

is essentially equivalent to:

```python
def greet():
    ...

greet = decorator(greet)
```

The `@decorator` syntax is called **decorator syntax** or **decorator syntax sugar**.

---

# 9. How Decorators Work Internally

Consider:

```python
def decorator(func):

    def wrapper():
        print("Before")
        func()
        print("After")

    return wrapper


@decorator
def greet():
    print("Hello")
```

Conceptually Python does:

```python
def greet():
    print("Hello")

greet = decorator(greet)
```

Then:

```python
greet()
```

actually calls:

```text
wrapper()
   ↓
print("Before")
   ↓
func()
   ↓
original greet()
   ↓
print("After")
```

### Flow

```text
@decorator
     ↓
decorator(greet)
     ↓
wrapper function returned
     ↓
greet now refers to wrapper
     ↓
greet()
     ↓
wrapper()
     ↓
original greet()
```

### Key Insight

The decorator usually runs **when the function is defined**, while the wrapper's body runs **when the decorated function is called**.

---

# 10. Decorators with `*args` and `**kwargs`

A decorator should usually work with functions having different arguments.

Consider:

```python
def greet(name):
    print(f"Hello {name}")
```

A simple wrapper like this won't work:

```python
def decorator(func):

    def wrapper():
        func()

    return wrapper
```

Because `greet()` expects an argument.

We can make the wrapper flexible:

```python
def decorator(func):

    def wrapper(*args, **kwargs):
        print("Before")

        result = func(*args, **kwargs)

        print("After")

        return result

    return wrapper
```

Now it can handle:

```python
@decorator
def greet(name):
    print(f"Hello {name}")
```

Call:

```python
greet("Abhijit")
```

Output:

```text
Before
Hello Abhijit
After
```

### Why `*args` and `**kwargs`?

They allow the wrapper to accept:

* any number of positional arguments
* any number of keyword arguments

And:

```python
func(*args, **kwargs)
```

passes them to the original function.

### Important Pattern

This is one of the most important decorator patterns:

```python
def decorator(func):

    def wrapper(*args, **kwargs):
        # extra behavior

        result = func(*args, **kwargs)

        # extra behavior

        return result

    return wrapper
```

---

# 11. `functools.wraps`

There is an important problem with basic decorators.

Consider:

```python
def decorator(func):

    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)

    return wrapper
```

Then:

```python
@decorator
def greet():
    """Greets the user."""
    print("Hello")
```

Now:

```python
print(greet.__name__)
```

may give:

```text
wrapper
```

instead of:

```text
greet
```

The wrapper has replaced the original function metadata.

Python provides:

```python
from functools import wraps
```

Use:

```python
def decorator(func):

    @wraps(func)
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)

    return wrapper
```

Now:

```python
print(greet.__name__)
```

returns:

```text
greet
```

And the original docstring is preserved as well.

### What does `@wraps(func)` do?

It copies important metadata from the original function to the wrapper.

For example:

* `__name__`
* `__doc__`
* other relevant metadata

### Best Practice

When writing decorators, normally use:

```python
from functools import wraps
```

and:

```python
@wraps(func)
```

---

# 12. Decorators with Arguments

This is where decorators become slightly more advanced.

Suppose we want:

```python
@repeat(3)
def greet():
    print("Hello")
```

We can't directly make `repeat` behave like a normal decorator because:

```python
@repeat(3)
```

means Python first calls:

```python
repeat(3)
```

and expects that call to return a decorator.

So we need **three levels**:

```text
repeat(3)
   ↓
decorator
   ↓
wrapper
```

Example:

```python
from functools import wraps

def repeat(times):

    def decorator(func):

        @wraps(func)
        def wrapper(*args, **kwargs):

            for _ in range(times):
                func(*args, **kwargs)

        return wrapper

    return decorator
```

Usage:

```python
@repeat(3)
def greet():
    print("Hello")
```

Then:

```python
greet()
```

Output:

```text
Hello
Hello
Hello
```

### Mental Model

Normal decorator:

```text
decorator
   ↓
wrapper
```

Decorator with arguments:

```text
decorator_factory
       ↓
   decorator
       ↓
    wrapper
```

---

# 13. Multiple Decorators

A function can have multiple decorators.

```python
@decorator1
@decorator2
def greet():
    print("Hello")
```

This is equivalent to:

```python
greet = decorator1(decorator2(greet))
```

### Execution Order

The decorator closest to the function is applied first.

```text
greet
 ↓
decorator2
 ↓
decorator1
```

But when the resulting function is called, the outer wrapper runs first.

Conceptually:

```text
decorator1 wrapper
       ↓
decorator2 wrapper
       ↓
original greet
```

Example:

```python
def A(func):

    @wraps(func)
    def wrapper(*args, **kwargs):
        print("A before")
        result = func(*args, **kwargs)
        print("A after")
        return result

    return wrapper


def B(func):

    @wraps(func)
    def wrapper(*args, **kwargs):
        print("B before")
        result = func(*args, **kwargs)
        print("B after")
        return result

    return wrapper
```

```python
@A
@B
def greet():
    print("Hello")
```

Output:

```text
A before
B before
Hello
B after
A after
```

---

# 14. Practical Use Case — Logging

Decorators are useful for automatically logging function calls.

```python
from functools import wraps

def log_call(func):

    @wraps(func)
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")

        result = func(*args, **kwargs)

        print(f"Finished {func.__name__}")

        return result

    return wrapper
```

Usage:

```python
@log_call
def add(a, b):
    return a + b
```

```python
result = add(10, 20)
```

Output:

```text
Calling add
Finished add
```

---

# 15. Practical Use Case — Timing

We can measure execution time.

```python
import time
from functools import wraps

def timer(func):

    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.perf_counter()

        result = func(*args, **kwargs)

        end = time.perf_counter()

        print(f"{func.__name__}: {end - start:.4f}s")

        return result

    return wrapper
```

Usage:

```python
@timer
def process_data():
    time.sleep(1)
```

Calling:

```python
process_data()
```

might print:

```text
process_data: 1.0002s
```

Useful for:

* performance measurement
* debugging slow functions
* profiling specific operations

---

# 16. Practical Use Case — Authentication

A decorator can check whether a user is authenticated before allowing a function to run.

Conceptually:

```python
def require_login(func):

    @wraps(func)
    def wrapper(user, *args, **kwargs):

        if not user.is_authenticated:
            raise PermissionError("Login required")

        return func(user, *args, **kwargs)

    return wrapper
```

Usage:

```python
@require_login
def view_profile(user):
    return "Profile data"
```

Now authentication logic doesn't need to be repeated inside every protected function.

### Backend relevance

This pattern appears frequently in:

* web applications
* API endpoints
* permission checks
* middleware-like logic
* access control

---

# 17. Practical Use Case — Validation

A decorator can validate inputs before executing a function.

Example:

```python
from functools import wraps

def positive_only(func):

    @wraps(func)
    def wrapper(number):
        if number <= 0:
            raise ValueError("Number must be positive")

        return func(number)

    return wrapper
```

Usage:

```python
@positive_only
def square(number):
    return number ** 2
```

Now:

```python
square(5)
```

works.

But:

```python
square(-5)
```

raises:

```text
ValueError
```

---

# 18. Practical Use Case — Caching

Caching means storing a previous result so that repeated calls don't have to perform the same expensive work again.

Python provides a built-in decorator:

```python
from functools import lru_cache
```

Example:

```python
@lru_cache
def expensive_function(number):
    print("Calculating...")
    return number ** 2
```

First call:

```python
expensive_function(10)
```

calculates the result.

Second call:

```python
expensive_function(10)
```

can reuse the cached result instead of recalculating it.

### Why useful?

Caching is useful when:

* computation is expensive
* the same inputs occur repeatedly
* results are reusable

In backend/AI systems, caching can reduce:

* repeated computation
* database queries
* repeated API calls
* potentially expensive model/API operations

For production AI APIs, caching strategy needs additional considerations such as cache keys, freshness, and whether the underlying result can safely be reused.

---

# 19. Complete Decorator Example

Here's a reusable decorator pattern:

```python
from functools import wraps

def log_call(func):

    @wraps(func)
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")

        result = func(*args, **kwargs)

        print(f"Returned: {result}")

        return result

    return wrapper
```

Usage:

```python
@log_call
def add(a, b):
    return a + b
```

Call:

```python
result = add(10, 20)
```

Output:

```text
Calling add
Returned: 30
```

---

# 20. Important Decorator Concepts

| Concept                  | Meaning                                       |
| ------------------------ | --------------------------------------------- |
| Decorator                | Function that modifies/wraps another function |
| `func`                   | Original function passed to decorator         |
| Wrapper                  | New function that adds behavior               |
| `@decorator`             | Cleaner syntax for applying decorator         |
| `*args`                  | Captures positional arguments                 |
| `**kwargs`               | Captures keyword arguments                    |
| `functools.wraps`        | Preserves original function metadata          |
| Decorator with arguments | Usually requires an extra outer function      |
| Multiple decorators      | Decorators can be stacked                     |
| Closure                  | Wrapper can retain access to original `func`  |

---

# 21. Common Mistakes & Traps

### Trap 1: Calling the function instead of passing it

Wrong:

```python
decorator(greet())
```

Correct:

```python
decorator(greet)
```

Remember:

```python
greet     # pass function
greet()   # execute function
```

---

### Trap 2: Forgetting to return the wrapper

Wrong:

```python
def decorator(func):

    def wrapper():
        func()

    # missing return
```

Correct:

```python
return wrapper
```

Otherwise the decorated name becomes `None`.

---

### Trap 3: Forgetting `*args` and `**kwargs`

A wrapper like:

```python
def wrapper():
```

only works with functions requiring no arguments.

A general-purpose decorator usually uses:

```python
def wrapper(*args, **kwargs):
```

---

### Trap 4: Forgetting to return the original result

Suppose:

```python
def add(a, b):
    return a + b
```

If the wrapper does:

```python
def wrapper(*args, **kwargs):
    func(*args, **kwargs)
```

the result is lost.

Better:

```python
result = func(*args, **kwargs)
return result
```

Or simply:

```python
return func(*args, **kwargs)
```

---

### Trap 5: Forgetting `@wraps`

Without:

```python
@wraps(func)
```

the wrapper can hide the original function's metadata.

For reusable decorators, prefer `functools.wraps`.

---

### Trap 6: Confusing decorator execution with function execution

For:

```python
@decorator
def greet():
    ...
```

the decorator application happens when the function definition is processed.

The wrapper's body runs later when:

```python
greet()
```

is called.

---

# 22. Decorators vs Higher-Order Functions

A **higher-order function** is any function that:

* accepts a function as an argument, or
* returns a function.

A **decorator** is a common practical use of higher-order functions where one function wraps/modifies another function.

So:

```text
Higher-Order Function
        ↓
General concept

Decorator
        ↓
Specific pattern/use of functions
```

Every decorator relies on first-class functions, but not every higher-order function is a decorator.

---

# 23. Decorators and Closures

Decorators commonly use **closures**.

Example:

```python
def decorator(func):

    def wrapper():
        func()

    return wrapper
```

After `decorator()` finishes, `wrapper()` still remembers:

```python
func
```

from the enclosing scope.

That's a closure-like behavior.

This connects several concepts you've already learned:

```text
Functions as objects
        ↓
Functions passed as arguments
        ↓
Nested functions
        ↓
Closures
        ↓
Higher-order functions
        ↓
Decorators
```

---

# 24. Backend & GenAI Relevance

Decorators are **very important** for backend development.

You will encounter them in frameworks and libraries for:

* authentication
* authorization
* routing
* validation
* logging
* caching
* dependency handling
* error handling
* performance monitoring

For example, web frameworks often use decorator syntax to register functions as routes.

Conceptually:

```python
@some_route("/users")
def get_users():
    ...
```

The decorator tells the framework something about the function and/or wraps/registers it.

In AI/GenAI systems, decorators can also be useful for:

* logging model calls
* measuring latency
* retry logic
* caching results
* validating inputs/outputs
* tracing
* tool/function execution
* monitoring AI pipelines

---

# 25. Interview Answer

### What is a decorator in Python?

> **A decorator is a function that takes another function, adds or modifies its behavior, and returns a new function, usually a wrapper. Python provides `@decorator` syntax as a convenient way to apply decorators. Decorators rely on Python's first-class functions and are commonly used for logging, authentication, validation, timing, caching, and other cross-cutting concerns.**

### How does `@decorator` work?

> `@decorator` is syntactic sugar for assigning the decorated function to the result of the decorator. For example, `@decorator def func(): ...` is essentially equivalent to defining `func` first and then doing `func = decorator(func)`.

### Why use `functools.wraps`?

> `functools.wraps` preserves important metadata of the original function, such as its name and docstring, when the function is wrapped by a decorator.

---

# 26. Priority

| Topic                              | Priority           |
| ---------------------------------- | ------------------ |
| What is a decorator?               | 🔥 Core            |
| Functions as objects               | 🔥 Core            |
| Inner/wrapper functions            | 🔥 Core            |
| Passing functions as arguments     | 🔥 Core            |
| Returning functions                | 🔥 Core            |
| `@decorator` syntax                | 🔥 Core            |
| How decorators work internally     | 🔥 Core            |
| `*args` / `**kwargs` in decorators | 🔥 Core            |
| `functools.wraps`                  | 🔥 Core            |
| Multiple decorators                | 🟡 Know & Move On  |
| Decorators with arguments          | 🟡 Know & Move On  |
| Logging                            | 🔥 Core            |
| Authentication                     | 🔥 Core            |
| Validation                         | 🔥 Core            |
| Timing                             | 🔥 Core            |
| Caching                            | 🔥 Core            |
| Advanced decorator metaprogramming | ⚪ Optional for Now |

## Final Mental Model

```text
Decorator
   │
   ├── takes original function
   │
   ↓
decorator(func)
   │
   ├── defines wrapper
   │
   ├── wrapper adds extra behavior
   │
   └── returns wrapper
           │
           ↓
      decorated function
```

And the most important pattern to remember:

```python
from functools import wraps

def decorator(func):

    @wraps(func)
    def wrapper(*args, **kwargs):
        # Before
        result = func(*args, **kwargs)
        # After

        return result

    return wrapper
```

**If you understand this pattern deeply, you understand the foundation of most Python decorators.**
