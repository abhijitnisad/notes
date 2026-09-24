# 3. Attributes & Methods 🔥 Core

## 3.1 What are Attributes?

An **attribute** is a value/data associated with an object or class.

For example, a student object may have:

```text
name
age
branch
roll_number
```

These are attributes.

Think:

> **Attribute = data/state associated with an object.**

Example:

```python
class Student:
    pass


student = Student()

student.name = "Abhijit"
student.age = 25
student.branch = "CSE"
```

Now the object has:

```text
student
│
├── name   → "Abhijit"
├── age    → 25
└── branch → "CSE"
```

---

# 3.2 Instance Attributes

An **instance attribute** is an attribute that belongs to a particular object/instance.

Example:

```python
class Student:
    pass


student1 = Student()
student2 = Student()

student1.name = "Abhijit"
student2.name = "Rahul"
```

Now:

```python
print(student1.name)
print(student2.name)
```

Output:

```text
Abhijit
Rahul
```

Both objects are instances of the same class, but their `name` attributes contain different values.

### Mental Model

```text
Student class
      │
      ├───────────────┐
      ↓               ↓
  student1         student2
      │               │
 name="Abhijit"    name="Rahul"
```

Each instance has its **own state**.

---

# 3.3 Where are Instance Attributes Usually Created?

Although Python allows:

```python
student.name = "Abhijit"
```

the normal and recommended way to initialize instance attributes is inside `__init__()`.

Example:

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age
```

Then:

```python
student = Student("Abhijit", 25)
```

The object now has:

```text
name → "Abhijit"
age  → 25
```

We'll study `__init__()` and `self` in detail in the next topic.

For now, understand:

```python
self.name = name
```

creates/stores an attribute on the particular instance.

---

# 3.4 Accessing Attributes

Use the **dot (`.`) operator** to access an attribute.

```python
student.name
student.age
```

Example:

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age


student = Student("Abhijit", 25)

print(student.name)
print(student.age)
```

Output:

```text
Abhijit
25
```

### General syntax

```text
object.attribute
```

Examples:

```python
student.name
student.age
account.balance
car.speed
```

---

# 3.5 Updating an Attribute

Attributes can usually be changed by assigning a new value.

```python
student.age = 26
```

Now:

```python
print(student.age)
```

Output:

```text
26
```

Conceptually:

```text
Before:
student
  └── age → 25

After:
student
  └── age → 26
```

This is one way an object's **state changes**.

---

# 3.6 What are Methods?

A **method** is a function defined inside a class that represents behavior associated with that class/object.

Example:

```python
class Student:

    def study(self):
        print("Student is studying")
```

Here:

```text
study()
   ↓
method
```

Create an object:

```python
student = Student()
```

Call the method:

```python
student.study()
```

Output:

```text
Student is studying
```

### Mental Model

```text
Object
│
├── Attributes → Data / State
│
└── Methods → Behavior
```

For example:

```text
Student
│
├── name       → "Abhijit"
├── age        → 25
│
├── study()
└── attend_class()
```

---

# 3.7 Method vs Normal Function

A method is essentially a function defined within a class and used as part of the class's behavior.

### Normal function

```python
def add(a, b):
    return a + b
```

Called as:

```python
add(10, 20)
```

### Method

```python
class Calculator:

    def add(self, a, b):
        return a + b
```

Called through an object:

```python
calculator = Calculator()

calculator.add(10, 20)
```

The important difference is that a method is associated with a class/object and can interact with the object's state.

---

# 3.8 Calling Methods

Use:

```text
object.method()
```

Example:

```python
class Student:

    def study(self):
        print("Studying...")


student = Student()

student.study()
```

Output:

```text
Studying...
```

Here:

```text
student
   ↓
.study()
   ↓
calls Student.study()
```

---

# 3.9 Methods Can Work With Object Attributes

This is where attributes and methods become powerful.

```python
class Student:

    def __init__(self, name):
        self.name = name

    def introduce(self):
        print(f"My name is {self.name}")
```

Create object:

```python
student = Student("Abhijit")
```

Call:

```python
student.introduce()
```

Output:

```text
My name is Abhijit
```

The method accesses the object's attribute:

```python
self.name
```

So:

```text
Object
│
├── name → "Abhijit"
│
└── introduce()
        │
        └── uses self.name
```

This is a fundamental OOP idea:

> **Methods operate on or with the object's state.**

---

# 3.10 Why `self` Appears in Methods

Consider:

```python
class Student:

    def introduce(self):
        print(self.name)
```

`self` refers to the **current instance** when the method is called as an instance method.

Example:

```python
student1.introduce()
```

Inside the method, `self` refers to `student1`.

If:

```python
student2.introduce()
```

then `self` refers to `student2`.

Conceptually:

```text
student1.introduce()
        ↓
self = student1


student2.introduce()
        ↓
self = student2
```

This allows the same method definition to work with different objects.

> We'll study `self` deeply in the next topic.

---

# 3.11 Example: Two Objects, Different State

```python
class Student:

    def __init__(self, name):
        self.name = name

    def introduce(self):
        print(f"My name is {self.name}")


student1 = Student("Abhijit")
student2 = Student("Rahul")

student1.introduce()
student2.introduce()
```

Output:

```text
My name is Abhijit
My name is Rahul
```

The method is the **same method definition**, but it operates on different object state.

```text
Student.introduce()
       │
       ├── student1 → self.name = Abhijit
       │
       └── student2 → self.name = Rahul
```

---

# 3.12 Accessing Attributes Through Methods

Methods can also modify attributes.

```python
class BankAccount:

    def __init__(self, balance):
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount
```

Create:

```python
account = BankAccount(5000)
```

Current state:

```text
balance → 5000
```

Call:

```python
account.deposit(1000)
```

Now:

```text
balance → 6000
```

The method changed the object's state.

This is a key OOP pattern:

```text
Method
  ↓
reads/modifies
  ↓
Object's state
```

---

# 3.13 Attributes Can Be Different for Different Objects

Example:

```python
class Car:

    def __init__(self, brand, speed):
        self.brand = brand
        self.speed = speed
```

Create:

```python
car1 = Car("BMW", 120)
car2 = Car("Toyota", 100)
```

Now:

```text
car1
├── brand → BMW
└── speed → 120

car2
├── brand → Toyota
└── speed → 100
```

The class defines the structure, but each instance stores its own values.

---

# 3.14 Attribute Access vs Method Call

This distinction is important.

### Attribute

```python
student.name
```

You are **accessing data**.

### Method

```python
student.study()
```

You are **calling behavior**.

Notice the parentheses:

```text
student.name
     ↑
attribute


student.study()
     ↑
method
```

A method is also an attribute lookup that gives you a callable method object when accessed through an instance, but at your current level, the practical distinction is:

> **Attribute → data**
>
> **Method → behavior**

---

# 3.15 Common Mistakes

### Mistake 1: Forgetting `self`

Wrong:

```python
class Student:

    def introduce():
        print("Hello")
```

For an ordinary instance method, you need the instance parameter:

```python
class Student:

    def introduce(self):
        print("Hello")
```

---

### Mistake 2: Using `name` instead of `self.name`

Wrong:

```python
class Student:

    def __init__(self, name):
        self.name = name

    def introduce(self):
        print(name)
```

`name` is not automatically available inside `introduce()`.

Correct:

```python
print(self.name)
```

because the attribute belongs to the current object.

---

### Mistake 3: Forgetting parentheses when calling a method

```python
student.study
```

This accesses the method attribute.

```python
student.study()
```

This calls the method.

Similar to:

```python
func
```

vs

```python
func()
```

---

### Mistake 4: Thinking all objects share instance attributes

```python
student1.name = "Abhijit"
student2.name = "Rahul"
```

These are separate instance attributes.

Changing:

```python
student1.name = "Amit"
```

doesn't automatically change:

```python
student2.name
```

---

# 3.16 Important Terminology

| Term               | Meaning                                                 |
| ------------------ | ------------------------------------------------------- |
| Attribute          | Data associated with an object/class                    |
| Instance attribute | Attribute belonging to a particular object              |
| Method             | Function defined in a class representing behavior       |
| Object state       | Current values stored in an object                      |
| Dot operator `.`   | Used to access attributes/methods                       |
| `self`             | Reference to the current instance in an instance method |

---

# 3.17 Interview Answer

### What is an attribute?

> **An attribute is data associated with an object or class. An instance attribute represents state belonging to a particular object.**

### What is a method?

> **A method is a function defined inside a class that represents behavior associated with its objects.**

### How do you access an attribute?

> Using the dot operator:
>
> ```python
> object.attribute
> ```

### How do you call a method?

> Using the dot operator followed by parentheses:
>
> ```python
> object.method()
> ```

### What is the relationship between attributes and methods?

> **Attributes represent an object's state, while methods represent behavior that can read or modify that state.**

---

# 3.18 Backend / GenAI Relevance

This pattern appears everywhere in larger Python applications.

For example:

```python
class LLMClient:

    def __init__(self, model):
        self.model = model

    def generate(self, prompt):
        # use self.model
        ...
```

Here:

```text
LLMClient
│
├── Attribute
│   └── model
│
└── Method
    └── generate()
```

A particular object can maintain configuration/state:

```python
client = LLMClient("some-model")
```

and methods can operate using that state:

```python
client.generate("Explain Python")
```

This same pattern appears in:

* API clients
* database connections
* vector stores
* retrievers
* agents
* tools
* services
* repositories

---

# 3.19 Final Mental Model

The most important model to remember:

```text
                 OBJECT
                   │
          ┌────────┴────────┐
          ↓                 ↓
      ATTRIBUTES          METHODS
       (State)           (Behavior)
          │                 │
          ↓                 ↓
      name = ...         study()
      age = ...          introduce()
      balance = ...      deposit()
```

Example:

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def introduce(self):
        print(f"My name is {self.name}")


student = Student("Abhijit", 25)

print(student.name)       # attribute
student.introduce()       # method
```

### One-line memory trick

> **Attributes = what an object has; Methods = what an object does.**

And:

```text
object.attribute  → access data
object.method()   → perform behavior
```

---

## Priority

| Concept                        | Priority           |
| ------------------------------ | ------------------ |
| Instance attributes            | 🔥 Core            |
| What is a method?              | 🔥 Core            |
| Accessing attributes           | 🔥 Core            |
| Calling methods                | 🔥 Core            |
| Attributes vs methods          | 🔥 Core            |
| Object state                   | 🔥 Core            |
| Methods accessing object state | 🔥 Core            |
| Basic `self` idea              | 🔥 Core            |
| Dynamic attribute creation     | 🟡 Know & Move On  |
| Advanced attribute lookup      | ⚪ Optional for Now |
