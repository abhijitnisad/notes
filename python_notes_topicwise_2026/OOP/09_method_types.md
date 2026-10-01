# 9. Method Types 🔥 Core

Python classes commonly use three types of methods:

1. **Instance methods**
2. **Class methods**
3. **Static methods**

The main difference is **what the method is connected to**:

```text
Instance method → Object / Instance
Class method    → Class
Static method   → Neither specifically
```

---

# 9.1 Instance Methods 🔥

An **instance method** is a method that operates on a particular object/instance.

It normally takes `self` as its first parameter.

```python
class Student:

    def __init__(self, name):
        self.name = name

    def introduce(self):
        print(f"My name is {self.name}")
```

Create an object:

```python
student = Student("Abhijit")

student.introduce()
```

Output:

```text
My name is Abhijit
```

Here:

```python
def introduce(self):
```

is an instance method.

`self` refers to the particular object calling the method.

---

# 9.2 How Instance Methods Work

When you write:

```python
student.introduce()
```

Python conceptually supplies `student` as the first argument:

```python
Student.introduce(student)
```

So:

```python
class Student:

    def introduce(self):
        print(self)
```

and:

```python
student = Student()
student.introduce()
```

`self` refers to:

```text
student
```

### Mental model

```text
student.introduce()
        ↓
instance method
        ↓
self = student
```

This is why instance methods can access:

```python
self.name
self.age
self.balance
self.model
```

etc.

---

# 9.3 When to Use Instance Methods

Use an instance method when the operation needs to work with the **state of a particular object**.

Example:

```python
class BankAccount:

    def __init__(self, balance):
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount
```

Here:

```python
account.deposit(500)
```

needs the specific account's:

```python
self.balance
```

So an instance method is appropriate.

### Rule

> **If the method needs `self` / object-specific state, use an instance method.**

---

# 9.4 Class Methods 🔥

A **class method** is a method that is bound to the **class**, rather than a particular instance.

It uses:

```python
@classmethod
```

and normally takes `cls` as its first parameter.

Example:

```python
class Student:

    college = "TIT Technocrats"

    @classmethod
    def show_college(cls):
        print(cls.college)
```

Call it:

```python
Student.show_college()
```

Output:

```text
TIT Technocrats
```

Here:

```python
cls
```

refers to the class:

```text
Student
```

---

# 9.5 `@classmethod`

`@classmethod` is a **decorator** that transforms a normal function defined inside a class into a method that receives the class as its first argument.

Example:

```python
class Student:

    college = "TIT Technocrats"

    @classmethod
    def show_college(cls):
        print(cls.college)
```

The important part is:

```python
@classmethod
```

Without it:

```python
class Student:

    def show_college(cls):
        print(cls.college)
```

this is just an ordinary function defined in the class namespace. It does **not** automatically receive the class as `cls` when called through the class.

`@classmethod` changes the binding behavior.

---

# 9.6 `cls` in Class Methods

Just like `self` is the conventional name for the instance parameter, `cls` is the conventional name for the class parameter.

```python
class Student:

    college = "TIT Technocrats"

    @classmethod
    def show_college(cls):
        print(cls.college)
```

Here:

```text
self → instance
cls  → class
```

Important:

> `cls` is not a keyword.

It is simply the conventional parameter name.

You could technically write:

```python
@classmethod
def show_college(x):
    print(x.college)
```

but using `cls` is the standard and readable convention.

---

# 9.7 Calling a Class Method

You can call a class method through the class:

```python
Student.show_college()
```

You can also call it through an instance:

```python
student = Student()

student.show_college()
```

In both cases, the method receives the relevant class as `cls`.

Conceptually:

```text
Student.show_college()
        ↓
cls = Student
```

and:

```text
student.show_college()
        ↓
cls = Student
```

The important point is that the method is **class-bound**, not instance-bound.

---

# 9.8 Class Method Example: Changing Class State

```python
class Student:

    college = "TIT Technocrats"

    @classmethod
    def change_college(cls, new_college):
        cls.college = new_college
```

Now:

```python
Student.change_college("RGPV")
```

This changes:

```python
Student.college
```

to:

```text
RGPV
```

Why use `cls` instead of `Student`?

Because `cls` refers to the class on which the method is being used.

This becomes especially useful with **inheritance**, where `cls` can refer to a subclass rather than always hard-coding the parent class name.

---

# 9.9 Class Methods as Alternative Constructors 🔥

One of the most useful real-world uses of `@classmethod` is creating **alternative constructors**.

Suppose:

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age
```

Normally:

```python
student = Student("Abhijit", 25)
```

But suppose the data comes as a string:

```text
"Abhijit,25"
```

We can create an alternative constructor:

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age

    @classmethod
    def from_string(cls, data):
        name, age = data.split(",")
        return cls(name, int(age))
```

Now:

```python
student = Student.from_string("Abhijit,25")
```

This creates a `Student` object.

### Flow

```text
Student.from_string(...)
        ↓
class method
        ↓
parse input
        ↓
cls(name, age)
        ↓
create Student object
```

This pattern is very common in Python.

### Interview point

> **A class method is often used as an alternative constructor because it receives `cls` and can create an instance of the class.**

---

# 9.10 Static Methods 🔥

A **static method** is a method defined inside a class that does not automatically receive either:

```text
self
```

or:

```text
cls
```

It uses:

```python
@staticmethod
```

Example:

```python
class MathUtils:

    @staticmethod
    def add(a, b):
        return a + b
```

Call it:

```python
print(MathUtils.add(10, 20))
```

Output:

```text
30
```

The method doesn't need an object or class state.

---

# 9.11 `@staticmethod`

`@staticmethod` is a decorator that prevents Python from automatically binding the function to an instance or class.

Example:

```python
class MathUtils:

    @staticmethod
    def add(a, b):
        return a + b
```

There is no:

```python
self
```

and no:

```python
cls
```

because the function doesn't need either.

---

# 9.12 When to Use Static Methods

Use a static method when the function:

- logically belongs to the class
- doesn't need instance state
- doesn't need class state

Example:

```python
class User:

    @staticmethod
    def is_valid_age(age):
        return age >= 18
```

Call:

```python
print(User.is_valid_age(20))
```

Output:

```text
True
```

The method doesn't need:

```python
self
```

because it doesn't need a particular `User` object.

It doesn't need:

```python
cls
```

because it doesn't need information from the `User` class.

---

# 9.13 Why Not Just Use a Normal Function?

Good question.

You could write:

```python
def is_valid_age(age):
    return age >= 18
```

instead of:

```python
class User:

    @staticmethod
    def is_valid_age(age):
        return age >= 18
```

The logic is the same.

The reason to use a static method can be **organization and conceptual grouping**.

If the function is closely related to `User`, keeping it inside `User` communicates:

> "This utility operation belongs conceptually to User."

But don't put every unrelated helper function inside a class just because Python allows it.

---

# 9.14 All Three Method Types Together

This example shows the difference clearly:

```python
class Student:

    college = "TIT Technocrats"

    def __init__(self, name, age):
        self.name = name
        self.age = age

    # Instance method
    def introduce(self):
        print(f"I am {self.name}")

    # Class method
    @classmethod
    def show_college(cls):
        print(cls.college)

    # Static method
    @staticmethod
    def is_adult(age):
        return age >= 18
```

Now:

```python
student = Student("Abhijit", 25)
```

### Instance method

```python
student.introduce()
```

Uses:

```text
self → student
```

### Class method

```python
Student.show_college()
```

Uses:

```text
cls → Student
```

### Static method

```python
Student.is_adult(25)
```

Uses:

```text
neither self nor cls
```

---

# 9.15 The Most Important Comparison

| Method | Decorator | First parameter | Bound to | Uses |
|---|---|---|---|---|
| Instance method | None | `self` | Instance | Object state |
| Class method | `@classmethod` | `cls` | Class | Class state / alternative constructors |
| Static method | `@staticmethod` | None automatically | Neither | Utility logic |

### Memory trick

```text
self → this object
cls  → this class
static → independent function inside class
```

---

# 9.16 Calling All Three

Consider:

```python
class Student:

    college = "TIT"

    def __init__(self, name):
        self.name = name

    def show_name(self):
        print(self.name)

    @classmethod
    def show_college(cls):
        print(cls.college)

    @staticmethod
    def greet():
        print("Hello")
```

### Instance method

```python
student = Student("Abhijit")

student.show_name()
```

Conceptually:

```python
Student.show_name(student)
```

---

### Class method

```python
Student.show_college()
```

Conceptually:

```text
cls = Student
```

---

### Static method

```python
Student.greet()
```

No automatic:

```text
self
```

or:

```text
cls
```

is supplied.

---

# 9.17 A Critical Difference: `self` vs `cls`

This distinction is worth remembering:

### `self`

Refers to:

```text
one particular object
```

Example:

```python
student1
```

So:

```python
self.name
```

means:

> Get the `name` belonging to this particular object.

### `cls`

Refers to:

```text
the class
```

So:

```python
cls.college
```

means:

> Get the `college` associated with this class.

---

# 9.18 Class Method vs Static Method

This is a common interview question.

### Class method

Use when the method needs information about the class.

```python
class Student:

    college = "TIT"

    @classmethod
    def show_college(cls):
        return cls.college
```

It receives:

```text
cls
```

automatically.

### Static method

Use when the method doesn't need class or instance information.

```python
class Student:

    @staticmethod
    def is_valid_age(age):
        return age >= 18
```

It receives neither automatically.

### Simple rule

```text
Needs instance data? → Instance method

Needs class data?    → Class method

Needs neither?       → Static method
```

---

# 9.19 A Deeper Mental Model

Think of a class as a container with different kinds of behavior:

```text
                 Student
                    │
       ┌────────────┼────────────┐
       ↓            ↓            ↓
   Instance       Class        Static
    Method        Method        Method
       │            │            │
       ↓            ↓            ↓
     self          cls       nothing
       │            │            │
       ↓            ↓            ↓
   one object     class       independent
```

This is the easiest mental model to remember.

---

# 9.20 Common Mistakes

### Mistake 1: Forgetting `self`

Incorrect:

```python
class Student:

    def show_name():
        print(self.name)
```

Correct:

```python
class Student:

    def show_name(self):
        print(self.name)
```

---

### Mistake 2: Using `self` in a class method

Incorrect:

```python
@classmethod
def show_college(self):
    print(self.college)
```

Technically the parameter could be named `self`, but that is confusing and violates the normal convention.

Prefer:

```python
@classmethod
def show_college(cls):
    print(cls.college)
```

Remember:

```text
self → instance
cls  → class
```

---

### Mistake 3: Expecting `self` in a static method

Incorrect:

```python
@staticmethod
def greet(self):
    print("Hello")
```

If the method doesn't need instance state, simply:

```python
@staticmethod
def greet():
    print("Hello")
```

---

### Mistake 4: Thinking `@staticmethod` makes a method "more static"

The important meaning is:

> Python does not automatically bind an instance or class to the function.

It is still a function stored in the class namespace and accessed through the class or an instance.

---

### Mistake 5: Thinking `@classmethod` must always modify class data

No.

A class method can:

- read class data
- modify class data
- create instances
- implement alternative constructors
- perform class-level operations

---

# 9.21 Backend / GenAI Relevance

You will encounter these patterns in libraries and application code.

For example:

```python
class APIClient:

    default_timeout = 30

    def __init__(self, api_key):
        self.api_key = api_key

    def request(self, url):
        # Uses this client's API key
        pass

    @classmethod
    def from_environment(cls):
        # Create client using environment configuration
        pass

    @staticmethod
    def validate_url(url):
        return url.startswith("https://")
```

The design naturally maps to:

```text
request()
    ↓
needs this object's API key
    ↓
instance method

from_environment()
    ↓
creates/configures class instances
    ↓
class method

validate_url()
    ↓
needs neither object nor class state
    ↓
static method
```

This kind of separation is useful in backend and GenAI SDK/application design.

---

# 9.22 Interview Questions

### Q1. What is an instance method?

> An instance method is a method that operates on a particular object and normally receives the instance as its first parameter, conventionally named `self`.

### Q2. What is a class method?

> A class method is a method bound to the class rather than a particular instance. It is created using `@classmethod` and normally receives the class as its first parameter, conventionally named `cls`.

### Q3. What is a static method?

> A static method is a method that doesn't automatically receive either the instance or the class. It is created using `@staticmethod` and is useful for logic that is related to the class but doesn't require object or class state.

### Q4. Difference between `@classmethod` and `@staticmethod`?

> A class method receives `cls` and can work with class-level state, while a static method receives neither `self` nor `cls` automatically.

### Q5. Why use `@classmethod`?

> Common uses include class-level operations and alternative constructors.

### Q6. Why use `@staticmethod`?

> To keep a logically related utility function inside a class when it doesn't need instance or class state.

---

# 9.23 Final Mental Model

Remember these three lines:

```python
def method(self):
```

```text
→ Instance method
→ Works with one object
```

```python
@classmethod
def method(cls):
```

```text
→ Class method
→ Works with the class
```

```python
@staticmethod
def method(...):
```

```text
→ Static method
→ Needs neither object nor class automatically
```

### One-line memory trick

> **`self` = object, `cls` = class, static = neither.**

---

## Priority

| Concept | Priority |
|---|---|
| Instance methods | 🔥 Core |
| `self` in instance methods | 🔥 Core |
| Class methods | 🔥 Core |
| `@classmethod` | 🔥 Core |
| `cls` | 🔥 Core |
| Static methods | 🔥 Core |
| `@staticmethod` | 🔥 Core |
| Instance vs class vs static | 🔥 Core |
| Alternative constructors | 🟡 Know & Move On |
| Descriptor/binding internals | ⚪ Optional for Now |