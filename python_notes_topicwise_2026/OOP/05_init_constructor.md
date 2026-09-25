# 5. `__init__()` Constructor 🔥 Core

## 5.1 What is a Constructor?

A **constructor** is commonly understood as the method that runs when an object is created and prepares the object with its initial state.

In Python, the method commonly used for initialization is:

```python
__init__()
```

Example:

```python
class Student:

    def __init__(self):
        print("Object initialized")
```

When we create an object:

```python
student = Student()
```

Python automatically calls `__init__()` for that instance.

Output:

```text
Object initialized
```

### Simple mental model

```text
Student()
   ↓
object is created
   ↓
__init__() runs
   ↓
object gets initial state
```

---

# 5.2 Important Python Terminology

You will often hear:

> "`__init__()` is the constructor."

This is acceptable in everyday Python conversation, but technically Python separates **creation** and **initialization**.

### `__new__()`

Responsible for **creating/allocating the instance**.

### `__init__()`

Responsible for **initializing the already-created instance**.

Conceptually:

```text
Class()
  ↓
__new__()
  ↓
instance created
  ↓
__init__(instance, ...)
  ↓
instance initialized
```

For normal Python development, you will use `__init__()` far more often.

`__new__()` is an advanced topic and is usually not needed when learning basic OOP.

### Interview-safe answer

> **`__init__()` is the initializer method that Python automatically calls after an instance has been created. It is commonly called the constructor in Python, although technically `__new__()` creates the instance and `__init__()` initializes it.**

---

# 5.3 What Does `__init__()` Do?

Its main purpose is to give an object its **initial state**.

Example:

```python
class Student:

    def __init__(self):
        self.name = "Unknown"
        self.age = 0
```

Create:

```python
student = Student()
```

The object starts with:

```text
student
│
├── name → "Unknown"
└── age  → 0
```

So `__init__()` is commonly where we create and initialize instance attributes.

---

# 5.4 Why Do We Need `__init__()`?

Without `__init__()`, you could manually add attributes after creating the object:

```python
class Student:
    pass


student = Student()

student.name = "Abhijit"
student.age = 25
```

This works, but it is not a good way to guarantee that every `Student` starts with the required data.

With `__init__()`:

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age
```

Now:

```python
student = Student("Abhijit", 25)
```

Every `Student` created this way receives the required initial state.

---

# 5.5 Basic `__init__()` Syntax

```python
class ClassName:

    def __init__(self, parameters):
        self.attribute = value
```

Example:

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age
```

Here:

```text
__init__()
│
├── self → current object
├── name → input parameter
└── age  → input parameter
```

And:

```python
self.name = name
```

means:

> Store the received `name` value as an attribute on the current object.

---

# 5.6 Passing Values While Creating Objects

This is one of the most important uses of `__init__()`.

Consider:

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age
```

Now:

```python
student = Student("Abhijit", 25)
```

The values:

```text
"Abhijit"
25
```

are passed while creating the object.

Conceptually:

```text
Student("Abhijit", 25)
        │          │
        ↓          ↓
      name       age
```

Then:

```python
self.name = name
self.age = age
```

stores those values in the new instance.

---

# 5.7 Step-by-Step Flow

Consider:

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age


student = Student("Abhijit", 25)
```

Think about it in this sequence:

### Step 1 — Python has the class

```text
Student
   ↓
class object
```

### Step 2 — We call the class

```python
Student("Abhijit", 25)
```

This requests a new `Student` instance.

### Step 3 — Instance is created

Conceptually:

```text
new Student instance
```

### Step 4 — `__init__()` receives the instance and values

Conceptually:

```text
self → new Student instance
name → "Abhijit"
age  → 25
```

### Step 5 — Attributes are initialized

```python
self.name = name
self.age = age
```

Result:

```text
student
│
├── name → "Abhijit"
└── age  → 25
```

---

# 5.8 Why `self.name = name`?

This is one of the most common beginner confusions.

Look at:

```python
def __init__(self, name):
    self.name = name
```

There are two different `name`s.

### Right side

```python
name
```

is the parameter received by the method.

### Left side

```python
self.name
```

is the attribute belonging to the current object.

So:

```text
self.name = name
     │       │
     │       └── received value
     │
     └── object's attribute
```

Example:

```python
student = Student("Abhijit")
```

becomes conceptually:

```text
self → student
name → "Abhijit"

self.name = name
     ↓
student.name = "Abhijit"
```

---

# 5.9 Multiple Objects, Different Initial State

The same class can create many objects with different values.

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age


student1 = Student("Abhijit", 25)
student2 = Student("Rahul", 22)
```

Now:

```text
student1
├── name → Abhijit
└── age  → 25

student2
├── name → Rahul
└── age  → 22
```

The class provides the structure, while each object's `__init__()` call establishes its own state.

---

# 5.10 `__init__()` Is Automatically Called

You normally don't call it directly.

Use:

```python
student = Student("Abhijit", 25)
```

rather than:

```python
student.__init__("Abhijit", 25)
```

The first form is the normal way to create and initialize the object.

Python handles the initialization call as part of instance creation.

---

# 5.11 `__init__()` Does Not Return the Object

A common mistake is:

```python
class Student:

    def __init__(self, name):
        self.name = name
        return self
```

This is wrong.

`__init__()` must return `None`.

Normally:

```python
class Student:

    def __init__(self, name):
        self.name = name
```

There is no explicit return, so it returns `None`.

### Important distinction

```text
__new__()
   ↓
creates/returns instance

__init__()
   ↓
initializes instance
   ↓
returns None
```

This is another reason why technically calling `__init__()` a constructor can be slightly imprecise.

---

# 5.12 Default Values in `__init__()`

Parameters can have default values.

```python
class Student:

    def __init__(self, name, age=18):
        self.name = name
        self.age = age
```

Now:

```python
student1 = Student("Abhijit", 25)
student2 = Student("Rahul")
```

Results:

```text
student1.age → 25
student2.age → 18
```

This follows the same default-parameter rules you learned earlier.

---

# 5.13 Keyword Arguments While Creating Objects

You can also pass values by parameter name.

```python
student = Student(
    name="Abhijit",
    age=25
)
```

This works because the arguments are passed to `__init__()`.

You can also change the order:

```python
student = Student(
    age=25,
    name="Abhijit"
)
```

because these are keyword arguments.

---

# 5.14 `__init__()` Can Perform Validation

Initialization can also validate input.

Example:

```python
class BankAccount:

    def __init__(self, balance):
        if balance < 0:
            raise ValueError("Balance cannot be negative")

        self.balance = balance
```

Now:

```python
account = BankAccount(5000)
```

works.

But:

```python
account = BankAccount(-100)
```

raises an error.

This ensures the object doesn't start with an invalid state.

---

# 5.15 `__init__()` Can Initialize More Complex State

An object doesn't have to contain only primitive values.

Example:

```python
class ShoppingCart:

    def __init__(self):
        self.items = []
```

Create:

```python
cart = ShoppingCart()
```

Now:

```text
cart
└── items → []
```

Then:

```python
cart.items.append("Laptop")
```

The object's state becomes:

```text
cart
└── items → ["Laptop"]
```

This pattern is very common in real applications.

---

# 5.16 `__init__()` vs Normal Method

Both are methods defined inside a class, but their roles differ.

| `__init__()`                                        | Normal method                |
| --------------------------------------------------- | ---------------------------- |
| Called automatically during instance initialization | Called explicitly            |
| Establishes initial state                           | Performs behavior/operations |
| Usually runs once per instance initialization       | Can run many times           |
| Commonly initializes attributes                     | Usually reads/modifies state |
| Special method                                      | Regular instance method      |

Example:

```python
class Student:

    def __init__(self, name):
        self.name = name

    def study(self):
        print(f"{self.name} is studying")
```

Here:

```text
__init__()
   ↓
initializes object

study()
   ↓
performs behavior
```

---

# 5.17 `__init__()` + `self` Together

These two concepts are closely connected.

```python
class Student:

    def __init__(self, name):
        self.name = name
```

Break it down:

```text
__init__
  │
  ├── self
  │    ↓
  │  current object
  │
  └── name
       ↓
     value passed during creation
```

Then:

```python
student = Student("Abhijit")
```

results conceptually in:

```text
self → student
name → "Abhijit"

self.name = name

       ↓

student.name = "Abhijit"
```

This is the connection you should understand deeply.

---

# 5.18 What If We Don't Define `__init__()`?

You don't have to define it.

Example:

```python
class Student:
    pass

student = Student()
```

This can still create a `Student` instance.

If you need initialization logic or required initial state, you define your own `__init__()`.

So:

```text
No __init__()
   ↓
object can still be created

Custom __init__()
   ↓
object gets your specified initial state
```

---

# 5.19 Common Mistakes

### Mistake 1: Forgetting `self`

Wrong:

```python
class Student:

    def __init__(name, age):
        ...
```

For a normal instance initializer, the instance parameter comes first:

```python
def __init__(self, name, age):
```

---

### Mistake 2: Confusing `name` with `self.name`

```python
def __init__(self, name):
    self.name = name
```

Remember:

```text
name
   ↓
parameter

self.name
   ↓
object attribute
```

---

### Mistake 3: Trying to return an object from `__init__()`

Wrong:

```python
def __init__(self):
    return some_object
```

`__init__()` must return `None`.

---

### Mistake 4: Calling `__init__()` manually

Usually avoid:

```python
student.__init__("Abhijit")
```

Use:

```python
student = Student("Abhijit")
```

The normal object-creation mechanism handles initialization.

---

### Mistake 5: Thinking `__init__()` creates the object

Technically:

```text
__new__() → creates instance
__init__() → initializes instance
```

For normal OOP development, you mostly work with `__init__()`.

---

# 5.20 Interview Questions

### What is `__init__()` in Python?

> **`__init__()` is a special instance method that Python automatically calls to initialize a newly created object. It is commonly referred to as the constructor, although technically `__new__()` creates the instance and `__init__()` initializes it.**

### Why do we use `__init__()`?

> **We use `__init__()` to establish the initial state of an object, usually by initializing instance attributes from values provided during object creation.**

### How do you pass values while creating an object?

```python
student = Student("Abhijit", 25)
```

> Those values are passed to the class's `__init__()` method.

### What is the difference between `__new__()` and `__init__()`?

> **`__new__()` is responsible for creating and returning the instance, while `__init__()` initializes that instance after it has been created.**

### Can `__init__()` return a value?

> **No. `__init__()` must return `None`. It is used for initialization, not for returning the newly created object.**

---

# 5.21 Backend / GenAI Relevance

`__init__()` is extremely common in real Python applications.

For example:

```python
class LLMClient:

    def __init__(self, model, api_key):
        self.model = model
        self.api_key = api_key
```

Then:

```python
client = LLMClient(
    model="some-model",
    api_key="..."
)
```

The object now stores its configuration:

```text
client
│
├── model
└── api_key
```

Methods can then use that state:

```python
class LLMClient:

    def __init__(self, model):
        self.model = model

    def generate(self, prompt):
        # use self.model
        ...
```

This pattern appears frequently in:

* API clients
* database classes
* service classes
* repositories
* vector stores
* embedding clients
* LLM clients
* agents
* tools

---

# 5.22 Final Mental Model

Remember this complete flow:

```text
class Student:
        │
        ↓
Student("Abhijit", 25)
        │
        ↓
instance is created
        │
        ↓
__init__(self, name, age)
        │
        ├── self → new instance
        ├── name → "Abhijit"
        └── age  → 25
        │
        ↓
self.name = name
self.age = age
        │
        ↓
Student object initialized
```

Result:

```text
student
│
├── name → "Abhijit"
└── age  → 25
```

### The most important pattern

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age
```

```python
student = Student("Abhijit", 25)
```

Think:

> **`__init__()` receives the values needed to initialize the object, and `self.attribute = value` stores those values in that particular instance.**

### One-line memory trick

> **`__init__()` initializes an object's initial state.**

---

## Priority

| Concept                                       | Priority           |
| --------------------------------------------- | ------------------ |
| Constructor/initializer concept               | 🔥 Core            |
| Object initialization                         | 🔥 Core            |
| `__init__()`                                  | 🔥 Core            |
| Passing values during object creation         | 🔥 Core            |
| `self.attribute = value`                      | 🔥 Core            |
| `__init__()` + `self` relationship            | 🔥 Core            |
| Multiple objects with different initial state | 🔥 Core            |
| Default/keyword arguments in `__init__()`     | 🟡 Know & Move On  |
| Validation in `__init__()`                    | 🟡 Know & Move On  |
| `__new__()` vs `__init__()`                   | 🟡 Know & Move On  |
| Custom `__new__()`                            | ⚪ Optional for Now |
