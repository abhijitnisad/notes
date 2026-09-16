# 2. Parameters & Arguments — 🔥 Core

Parameters and arguments are used to **pass data into a function**.

---

## 1. What is a Parameter?

A **parameter** is a variable defined in a function's definition that receives a value when the function is called.

```python
def greet(name):
    print(f"Hello, {name}")
```

Here, `name` is a **parameter**.

Think:

```text
Function definition
       ↓
def greet(name):
           ↑
       parameter
```

---

## 2. What is an Argument?

An **argument** is the actual value that we pass to a function when calling it.

```python
greet("Abhijit")
```

Here, `"Abhijit"` is an **argument**.

Think:

```text
Function call
       ↓
greet("Abhijit")
       ↑
    argument
```

### Easy way to remember

> **Parameter = variable in the function definition**
> **Argument = actual value passed during the function call**

---

## 3. Parameter vs Argument

| Parameter                          | Argument                        |
| ---------------------------------- | ------------------------------- |
| Defined in the function definition | Passed during the function call |
| Acts as a receiving variable       | Provides the actual value       |
| Example: `name`                    | Example: `"Abhijit"`            |

```python
def greet(name):       # name → parameter
    print(name)

greet("Abhijit")       # "Abhijit" → argument
```

---

## 4. Multiple Parameters

A function can have multiple parameters.

```python
def add(a, b):
    return a + b

result = add(10, 20)
print(result)
```

Here:

* `a` and `b` → parameters
* `10` and `20` → arguments

The values are matched according to the argument-passing rules.

---

## 5. Positional Arguments

A **positional argument** is matched to a parameter based on its **position/order**.

```python
def introduce(name, age):
    print(name, age)

introduce("Abhijit", 21)
```

Mapping:

```text
name ← "Abhijit"
age  ← 21
```

The first argument goes to the first parameter, and the second argument goes to the second parameter.

### Important

Order matters:

```python
introduce(21, "Abhijit")
```

This is syntactically valid, but the values are assigned to the wrong parameters.

---

## 6. Keyword Arguments

A **keyword argument** passes a value by explicitly specifying the parameter name.

```python
def introduce(name, age):
    print(name, age)

introduce(age=21, name="Abhijit")
```

Here, Python matches the values by **parameter name**, not position.

Therefore, the order can be changed:

```python
introduce(age=21, name="Abhijit")
```

is equivalent to:

```python
introduce(name="Abhijit", age=21)
```

---

## 7. Positional vs Keyword Arguments

| Positional          | Keyword                        |
| ------------------- | ------------------------------ |
| Matched by position | Matched by parameter name      |
| Order matters       | Order generally doesn't matter |
| `func(10, 20)`      | `func(a=10, b=20)`             |
| More concise        | More explicit/readable         |

Example:

```python
def create_user(name, age):
    print(name, age)

create_user("Abhijit", 21)                 # positional
create_user(name="Abhijit", age=21)        # keyword
```

Keyword arguments are particularly useful when a function has several parameters because they make the call easier to understand.

---

## 8. Default Parameters

A **default parameter** has a default value that Python uses when the caller does not provide an argument for that parameter.

```python
def greet(name, message="Hello"):
    print(f"{message}, {name}")

greet("Abhijit")
```

Output:

```text
Hello, Abhijit
```

Here:

```python
message="Hello"
```

is the default value.

If an argument is provided, it overrides the default:

```python
greet("Abhijit", "Good morning")
```

Output:

```text
Good morning, Abhijit
```

### Important rule

A parameter with a default value cannot normally be followed by a required parameter.

❌ Incorrect:

```python
def func(a=10, b):
    pass
```

✅ Correct:

```python
def func(a, b=10):
    pass
```

---

## 9. Passing Different Types of Values

Python functions can receive values of different types because Python is **dynamically typed**.

```python
def display(value):
    print(value)

display(10)              # int
display(3.14)            # float
display("Hello")         # str
display([1, 2, 3])       # list
display({"a": 1})        # dict
```

The parameter doesn't have to be declared with a specific type:

```python
def display(value):
    ...
```

The actual object passed determines what `value` refers to during that call.

### Type hints

You can optionally document the expected type:

```python
def greet(name: str, age: int):
    print(name, age)
```

Type hints improve readability and tooling, but Python generally **does not enforce them at runtime by itself**.

---

## ⭐ Important Points

* **Parameter** → variable in the function definition.
* **Argument** → actual value passed during the function call.
* Positional arguments are matched according to **order**.
* Keyword arguments are matched according to **parameter name**.
* Keyword arguments can generally be supplied in a different order.
* Default parameters provide a value when an argument isn't supplied.
* A required parameter should not come after a default parameter.
* Python functions can receive objects of different types.
* Type hints can communicate expected types but are not runtime type enforcement by default.

---

## ⚠️ Common Confusion

Don't say:

> "`name` is an argument in `def greet(name)`."

❌ Technically, `name` is a **parameter**.

```python
def greet(name):
    ...
```

`name` → parameter

```python
greet("Abhijit")
```

`"Abhijit"` → argument

---

## 🎯 Interview Answer

**Q: What is the difference between a parameter and an argument?**

> **A parameter is a variable defined in a function's definition, while an argument is the actual value passed to the function when it is called. For example, in ****`def greet(name)`****, ****`name`**** is a parameter, and in ****`greet("Abhijit")`****, ****`"Abhijit"`**** is an argument.**

---

## 🤖 Backend / AI Relevance

This concept is **very important** for your future development work.

You'll frequently encounter functions such as:

```python
def generate_response(prompt, model="default"):
    ...
```

and calls like:

```python
generate_response(
    prompt="Explain Python",
    model="some-model"
)
```

Understanding positional arguments, keyword arguments, and defaults will become especially useful when working with **APIs, SDKs, FastAPI, AI libraries, and configuration-heavy functions**.
