# Function Scope — 🔥 Core

## 1. What is Scope?

**Scope defines the region of a Python program where a variable or name can be accessed directly.**

In simple terms:

> **Scope determines where Python can find and use a variable.**

Python primarily follows four levels of scope:

```text
L → Local
E → Enclosing
G → Global
B → Built-in
```

These together form the **LEGB rule**.

---

# 2. Local Scope

A variable created **inside a function** normally has local scope.

```python
def greet():
    name = "Abhijit"
    print(name)

greet()
```

Here, `name` is a **local variable**.

It can be accessed inside `greet()`:

```python
def greet():
    name = "Abhijit"
    print(name)
```

But not directly outside:

```python
def greet():
    name = "Abhijit"

print(name)   # NameError
```

The variable exists within the local scope of the function.

---

# 3. Global Scope

A variable created at the **top level of a module/file**, outside functions and classes, has global scope within that module.

```python
name = "Abhijit"

def greet():
    print(name)

greet()
```

The function can read the global variable because Python can find it after checking the local scope.

However, assigning to a name inside a function normally creates a **local variable** rather than modifying the global variable.

```python
count = 10

def update():
    count = 20
    print(count)

update()

print(count)
```

Output:

```text
20
10
```

The `count` inside `update()` is local and is different from the global `count`.

---

# 4. Enclosing / Non-local Scope

An **enclosing scope** occurs when a function is defined inside another function.

```python
def outer():
    message = "Hello"

    def inner():
        print(message)

    inner()

outer()
```

Here:

```text
outer()
 └── message
      ↓
    inner()
```

`message` is not local to `inner()`, but it exists in the surrounding `outer()` function.

Therefore, it is in the **enclosing scope** of `inner()`.

This concept becomes particularly important when learning **closures and decorators**.

---

# 5. Built-in Scope

The **built-in scope** contains names provided by Python itself.

Examples:

```python
print()
len()
sum()
type()
range()
```

For example:

```python
numbers = [10, 20, 30]

print(len(numbers))
```

Python finds `len` in the built-in namespace if it isn't found in the local, enclosing, or global scopes.

---

# 6. LEGB Rule ⭐

When Python encounters a name, it searches for it in this order:

```text
Local
   ↓
Enclosing
   ↓
Global
   ↓
Built-in
```

This is called the **LEGB rule**.

Example:

```python
x = "global"

def outer():
    x = "enclosing"

    def inner():
        x = "local"
        print(x)

    inner()

outer()
```

Output:

```text
local
```

Python finds `x` immediately in the **local scope**, so it doesn't continue searching.

If the local `x` didn't exist:

```python
def outer():
    x = "enclosing"

    def inner():
        print(x)

    inner()

outer()
```

Python finds `x` in the **enclosing scope**.

---

# 7. `global` Keyword

The `global` keyword tells Python that a variable inside a function refers to a **global variable**, rather than creating a new local variable.

```python
count = 10

def update():
    global count
    count = 20

update()

print(count)
```

Output:

```text
20
```

Without `global`:

```python
count = 10

def update():
    count = 20
```

the assignment creates a **local `count`**.

### Important

You should generally avoid excessive use of `global` variables because they can make larger programs harder to maintain.

---

# 8. `nonlocal` Keyword

The `nonlocal` keyword is used inside a nested function when you want to modify a variable from its **enclosing function's scope**.

```python
def outer():
    count = 0

    def inner():
        nonlocal count
        count += 1

    inner()
    print(count)

outer()
```

Output:

```text
1
```

Without `nonlocal`, this:

```python
count += 1
```

would be treated as an assignment to a local variable inside `inner()`.

### Easy distinction

```text
global
   ↓
modify a variable from global scope

nonlocal
   ↓
modify a variable from enclosing function scope
```

---

# 9. Local vs Global vs Non-local

| Scope     | Where variable is defined   | Keyword                      |
| --------- | --------------------------- | ---------------------------- |
| Local     | Inside current function     | None                         |
| Enclosing | Inside an outer function    | `nonlocal` when modifying it |
| Global    | At module level             | `global` when modifying it   |
| Built-in  | Python's built-in namespace | None                         |

Example:

```python
x = "global"

def outer():
    y = "enclosing"

    def inner():
        z = "local"
```

Here:

* `z` → local to `inner()`
* `y` → enclosing for `inner()`
* `x` → global
* `print`, `len`, `type`, etc. → built-in

---

# 10. Variable Shadowing

**Variable shadowing** occurs when a variable in a more specific scope has the same name as a variable in an outer scope.

Example:

```python
name = "Global"

def greet():
    name = "Local"
    print(name)

greet()
print(name)
```

Output:

```text
Local
Global
```

The local `name` **shadows** the global `name` inside the function.

The global variable still exists; Python simply finds the local variable first because of LEGB.

### Shadowing built-ins ⚠️

You can also accidentally shadow built-in names:

```python
list = [1, 2, 3]
```

Now `list` refers to your variable instead of Python's built-in `list`.

This can cause problems:

```python
list = [1, 2, 3]

numbers = list((4, 5))  # TypeError
```

So avoid using important built-in names as variable names, such as:

```text
list
str
dict
set
sum
len
input
```

---

# ⭐ Important Points

* **Scope** determines where a name can be accessed.
* Python follows the **LEGB** lookup rule.
* **Local** → current function.
* **Enclosing** → outer function when functions are nested.
* **Global** → module-level scope.
* **Built-in** → Python-provided names.
* `global` allows a function to modify a global variable.
* `nonlocal` allows a nested function to modify a variable from its enclosing function.
* Assignment inside a function normally creates a local variable.
* A variable in an inner scope can **shadow** a variable with the same name in an outer scope.
* Scope is a major foundation for understanding **closures and decorators**.

---

# ⚠️ Common Interview Trap

Consider:

```python
x = 10

def test():
    print(x)
    x = 20

test()
```

You might think Python prints `10`.

❌ It doesn't.

Python treats `x` as a **local variable** in `test()` because there is an assignment to `x` inside that function.

Therefore, `print(x)` tries to access the local `x` before it has been assigned, resulting in:

```text
UnboundLocalError
```

This is an important consequence of Python's scope rules.

---

# 🎯 Interview Answer

**Q: What is the LEGB rule in Python?**

> **LEGB stands for Local, Enclosing, Global, and Built-in. It describes the order Python follows when looking up a name. Python first searches the local scope, then enclosing scopes of nested functions, then the global scope, and finally the built-in scope.**

**Q: What is the difference between `global` and `nonlocal`?**

> **`global` is used inside a function to refer to and modify a variable in the global scope, while `nonlocal` is used inside a nested function to refer to and modify a variable in an enclosing function's scope.**

---

# 🤖 Backend / AI Relevance

Scope is **🔥 Core** because it becomes the foundation for understanding:

* Closures
* Decorators
* Callbacks
* Factory functions
* Function-based configuration
* Larger Python applications

For example, many advanced Python patterns used in frameworks and AI libraries rely on functions retaining access to variables from their enclosing scope.

You don't need to memorize every scope edge case right now. **Understand LEGB, local/global/enclosing scope, `global`, `nonlocal`, and shadowing very well.** These will make later topics such as **closures and decorators** much easier.
