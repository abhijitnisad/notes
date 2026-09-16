# Mutable & Immutable Arguments — 🔥 Core

## 1. Mutable vs Immutable Objects

**Mutable** means an object can be changed **after it is created**.

**Immutable** means an object **cannot be changed after it is created**.

### Common mutable objects

```text
list
dict
set
```

Example:

```python
numbers = [1, 2, 3]

numbers.append(4)

print(numbers)
```

Output:

```text
[1, 2, 3, 4]
```

The existing list was modified.

### Common immutable objects

```text
int
float
bool
str
tuple
frozenset
```

Example:

```python
name = "Abhijit"

name = name + " Kumar"
```

A string itself wasn't modified. Python created a **new string object** and `name` was made to refer to it.

### Easy mental model

> **Mutable → object can be changed.**
> **Immutable → object cannot be changed.**

---

# 2. Passing Objects to Functions

Python does **not** pass variables themselves to functions.

Python passes a **reference to an object** (more precisely, the function receives a reference to the same object).

Example:

```python
def show(value):
    print(value)

x = [1, 2, 3]

show(x)
```

The parameter `value` refers to the **same list object** that `x` refers to.

Conceptually:

```text
x ───────┐
         ↓
      [1, 2, 3]
         ↑
         │
       value
```

This becomes important when the object is mutable.

### Important terminology

You may hear:

> "Python is pass-by-reference."

or:

> "Python is pass-by-value."

Both descriptions can be misleading.

A better explanation is:

> **Python uses call-by-sharing (object reference semantics): the function receives a reference to the same object.**

What happens next depends on whether the object is mutable and whether the function **mutates the object or reassigns the parameter**.

---

# 3. List as a Function Argument

Lists are mutable.

Therefore, if a function modifies the list, the change can be seen outside the function.

```python
def add_item(items):
    items.append("Python")

languages = ["JavaScript"]

add_item(languages)

print(languages)
```

Output:

```text
['JavaScript', 'Python']
```

Why?

Both `items` and `languages` refer to the **same list object**.

```text
languages ──┐
            ↓
     ["JavaScript"]
            ↑
            │
         items
```

`append()` modifies that existing list.

---

## ⚠️ Mutation vs Reassignment

This distinction is extremely important.

### Mutation

```python
def modify(items):
    items.append(4)

numbers = [1, 2, 3]

modify(numbers)

print(numbers)
```

Output:

```text
[1, 2, 3, 4]
```

The original object was modified.

### Reassignment

```python
def modify(items):
    items = [10, 20]

numbers = [1, 2, 3]

modify(numbers)

print(numbers)
```

Output:

```text
[1, 2, 3]
```

Why?

Inside the function, `items` was simply made to refer to a **different list**.

The original `numbers` reference was not changed.

### Remember

> **Mutation changes the object.**
> **Reassignment changes what the local parameter refers to.**

---

# 4. Dictionary as a Function Argument

Dictionaries are also mutable.

```python
def update_user(user):
    user["age"] = 21

person = {"name": "Abhijit"}

update_user(person)

print(person)
```

Output:

```text
{'name': 'Abhijit', 'age': 21}
```

The function modified the same dictionary object.

This pattern is common in real Python applications because dictionaries are frequently used to represent structured data and configuration.

---

# 5. Why Mutable Default Arguments Can Cause Problems

This is a **very common Python interview question**.

Consider:

```python
def add_item(item, items=[]):
    items.append(item)
    return items
```

You might expect:

```python
print(add_item("A"))
print(add_item("B"))
```

to produce:

```text
['A']
['B']
```

But the actual result is:

```text
['A']
['A', 'B']
```

### Why?

The default list:

```python
items=[]
```

is created **once when the function is defined**, not every time the function is called.

So the same list is reused across calls where no `items` argument is provided.

Conceptually:

```text
Function definition
       ↓
   items = []
       ↓
Call 1 → append("A")
       ↓
   ["A"]
       ↓
Call 2 → append("B")
       ↓
   ["A", "B"]
```

This can create unexpected shared state.

---

# 6. `None` as a Safe Default Value

A common and safer pattern is to use `None` as the default.

```python
def add_item(item, items=None):
    if items is None:
        items = []

    items.append(item)
    return items
```

Now:

```python
print(add_item("A"))
print(add_item("B"))
```

Output:

```text
['A']
['B']
```

A new list is created **inside the function for each call** when no list is provided.

### Why `None`?

`None` is immutable and commonly used to represent **"no value was provided."**

Then we explicitly create the mutable object when needed.

---

## ⭐ Important Points

* **Mutable objects** can be modified after creation.
* **Immutable objects** cannot be modified after creation.
* Lists, dictionaries, and sets are common mutable objects.
* Strings, numbers, booleans, and tuples are common immutable objects.
* Function parameters are references to objects.
* Mutating a mutable object inside a function can affect the original object outside the function.
* Reassigning a parameter does **not** reassign the caller's variable.
* Mutable default arguments are dangerous because the default object is created once and reused.
* Use `None` as the default when you need a fresh mutable object for each call.

---

# ⚠️ Most Important Distinction

Understand this very well:

```python
def change(data):
    data.append(10)
```

Here:

**Mutation → original object changes.**

But:

```python
def change(data):
    data = [10]
```

Here:

**Reassignment → only the local parameter changes.**

This distinction is more important than simply memorizing:

> "Lists are passed by reference."

---

# 🎯 Interview Answer

**Q: How are objects passed to functions in Python?**

> **Python uses call-by-sharing, where a function receives a reference to the same object. If the object is mutable and the function modifies it, the change can be visible outside the function. However, if the parameter is reassigned to a different object, the caller's variable is not changed.**

**Q: Why should we avoid mutable default arguments?**

> **Mutable default arguments are created once when the function is defined and are reused across calls. This can cause unexpected state to be shared between calls. A common solution is to use `None` as the default and create a new mutable object inside the function.**

---

# 🤖 Backend / AI Relevance

This is **🔥 Core** for real Python development.

You'll frequently pass around:

```python
dict
list
```

when working with:

* JSON data
* API requests/responses
* configuration
* database results
* LLM messages
* agent state
* tool arguments
* RAG pipelines

Understanding **mutation vs reassignment** will help prevent subtle bugs when multiple parts of an application work with the same data.

### Priority

🔥 **Deeply understand**

* Mutable vs immutable
* Object references
* Mutation vs reassignment
* Mutable default arguments
* `None` as a default

You don't need to memorize the phrase **"call-by-sharing"** as much as you need to understand **what object the parameter refers to and whether you're mutating it or reassigning it**.
