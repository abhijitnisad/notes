# 6. Instance Attributes vs Class Attributes 🔥 Core

## 6.1 The Big Idea

Python classes can have two important kinds of attributes:

1. **Instance attributes** → belong to a particular object
2. **Class attributes** → belong to the class and are shared through the class

Example:

```python
class Student:

    college = "TIT Technocrats"   # class attribute

    def __init__(self, name):
        self.name = name           # instance attribute
```

Here:

```text
Student
│
├── college → "TIT Technocrats"   ← class attribute
│
├── student1
│   └── name → "Abhijit"          ← instance attribute
│
└── student2
    └── name → "Rahul"            ← instance attribute
```

### Mental model

> **Instance attribute = specific to one object**
> **Class attribute = associated with the class and shared by instances unless overridden**

---

# 6.2 Instance Attributes

An **instance attribute** belongs to a particular instance/object.

They are usually created using `self` inside `__init__()`.

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age
```

Create two objects:

```python
student1 = Student("Abhijit", 25)
student2 = Student("Rahul", 22)
```

Now:

```text
student1
├── name → "Abhijit"
└── age  → 25

student2
├── name → "Rahul"
└── age  → 22
```

Each object has its **own values**.

### Access

```python
student1.name
student2.name
```

### Key pattern

```python
self.attribute = value
```

means:

> Store an attribute on the current instance.

---

# 6.3 Class Attributes

A **class attribute** is defined directly inside the class body, outside instance methods.

Example:

```python
class Student:

    college = "TIT Technocrats"
```

Here:

```python
college
```

is a class attribute.

It is associated with the `Student` class.

You can access it through the class:

```python
print(Student.college)
```

Output:

```text
TIT Technocrats
```

Instances can also access it:

```python
student = Student()

print(student.college)
```

Python can find the attribute through the class.

---

# 6.4 Complete Example

```python
class Student:

    college = "TIT Technocrats"

    def __init__(self, name, age):
        self.name = name
        self.age = age
```

Create objects:

```python
student1 = Student("Abhijit", 25)
student2 = Student("Rahul", 22)
```

We have:

```text
Class:
Student
└── college → "TIT Technocrats"

Instance 1:
student1
├── name → "Abhijit"
└── age  → 25

Instance 2:
student2
├── name → "Rahul"
└── age  → 22
```

Notice:

* `name` is different for each object.
* `age` is different for each object.
* `college` is common through the class.

---

# 6.5 Instance vs Class Attribute

| Instance Attribute                       | Class Attribute                                  |
| ---------------------------------------- | ------------------------------------------------ |
| Belongs to a particular instance         | Defined on the class                             |
| Usually created with `self`              | Defined directly in class body                   |
| Each object can have its own value       | One class-level value can be shared              |
| `self.name`                              | `Student.college`                                |
| Usually represents object-specific state | Usually represents class-wide/shared information |

### Simple memory trick

```text
Instance → individual
Class → common
```

---

# 6.6 Where Are Instance Attributes Stored?

This is an important Python-under-the-hood concept.

For ordinary Python objects, instance attributes are commonly stored in the instance's `__dict__`.

Example:

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age


student = Student("Abhijit", 25)
```

You can inspect:

```python
print(student.__dict__)
```

Typical output:

```text
{'name': 'Abhijit', 'age': 25}
```

This shows the attributes stored directly on that instance.

Conceptually:

```text
student.__dict__
│
├── name → "Abhijit"
└── age  → 25
```

### Important nuance

This is the normal mechanism for ordinary Python objects, but not every Python object necessarily has a `__dict__`. For example, classes using `__slots__` can store instance attributes differently.

For your current OOP level:

> **Instance attributes are normally stored in the instance's `__dict__`.**

---

# 6.7 Where Are Class Attributes Stored?

Class attributes are stored on the **class object**.

For example:

```python
class Student:

    college = "TIT Technocrats"
```

You can inspect:

```python
print(Student.__dict__)
```

The class namespace contains entries including:

```text
college → "TIT Technocrats"
```

Conceptually:

```text
Student.__dict__
│
├── college
├── ...
└── methods
```

So:

```text
Instance attributes
        ↓
instance.__dict__

Class attributes
        ↓
class namespace / class.__dict__
```

This distinction is extremely useful for understanding attribute lookup.

---

# 6.8 Attribute Lookup

Suppose:

```python
class Student:

    college = "TIT Technocrats"

    def __init__(self, name):
        self.name = name


student = Student("Abhijit")
```

Now:

```python
student.name
```

Python finds:

```text
student
  ↓
instance attributes
  ↓
name found
```

But:

```python
student.college
```

doesn't find `college` on the instance.

Python can then look at the class:

```text
student
  ↓
instance attributes
  ↓
college not found
  ↓
Student class
  ↓
college found
```

So the object can access class attributes through the class.

### Simplified lookup model

For normal attribute access:

```text
object.attribute
      ↓
look at object's attributes
      ↓
if not found
      ↓
look through its class / inheritance hierarchy
```

The actual attribute lookup rules involve descriptors and the MRO, which we'll study later. This simplified model is enough for now.

---

# 6.9 Accessing a Class Attribute

You can access a class attribute directly through the class:

```python
class Student:
    college = "TIT Technocrats"


print(Student.college)
```

Output:

```text
TIT Technocrats
```

You can also access it through an instance:

```python
student = Student()

print(student.college)
```

Output:

```text
TIT Technocrats
```

But these two expressions have an important conceptual difference:

```python
Student.college
```

means:

> Look up `college` on the class.

While:

```python
student.college
```

means:

> Look up `college` starting from the instance; if appropriate, Python can find it through the class.

---

# 6.10 Changing an Instance Attribute

Suppose:

```python
class Student:

    college = "TIT Technocrats"

    def __init__(self, name):
        self.name = name
```

Create:

```python
student1 = Student("Abhijit")
student2 = Student("Rahul")
```

Now:

```python
student1.name = "Amit"
```

Only `student1` changes.

```text
student1.name → Amit
student2.name → Rahul
```

This is because `name` is an instance attribute.

---

# 6.11 Changing a Class Attribute

Suppose:

```python
class Student:

    college = "TIT Technocrats"
```

Now:

```python
Student.college = "RGPV"
```

The class attribute changes.

Instances that haven't overridden `college` can observe the new class value:

```python
student1 = Student()
student2 = Student()

print(student1.college)
print(student2.college)
```

After:

```python
Student.college = "RGPV"
```

both can resolve `college` to:

```text
RGPV
```

because they find it through the class.

---

# 6.12 The Important Trap: Assigning Through an Instance

This is one of the most important distinctions.

Suppose:

```python
class Student:

    college = "TIT Technocrats"
```

Create:

```python
student1 = Student()
student2 = Student()
```

Now do:

```python
student1.college = "RGPV"
```

Many beginners think:

> "I changed the class attribute."

But that's **not what happened**.

You created/assigned an **instance attribute** named `college` on `student1`.

Conceptually:

```text
Student
└── college → "TIT Technocrats"

student1
└── college → "RGPV"

student2
└── no own college
```

Now:

```python
print(student1.college)
```

gives:

```text
RGPV
```

while:

```python
print(student2.college)
```

still gives:

```text
TIT Technocrats
```

### Why?

Because `student1` now has its own `college` attribute, which takes precedence during normal lookup.

---

# 6.13 Class Attribute vs Instance Attribute With Same Name

Example:

```python
class Student:

    college = "TIT Technocrats"
```

Initially:

```text
Student.college
        ↓
TIT Technocrats
```

For an instance:

```python
student = Student()
```

there is no own `college` attribute.

So:

```python
student.college
```

finds the class value.

Now:

```python
student.college = "RGPV"
```

creates an instance-level value.

Now:

```text
Student.college
        ↓
TIT Technocrats

student.college
        ↓
RGPV
```

This is called **shadowing**: the instance attribute shadows the class attribute for that instance.

---

# 6.14 When Should You Use Instance Attributes?

Use an **instance attribute** when the value can be different for different objects.

Examples:

```text
Student:
    name
    age
    roll_number

BankAccount:
    account_number
    balance
    owner

LLMClient:
    api_key
    model
    base_url
```

Example:

```python
class Student:

    def __init__(self, name, roll_number):
        self.name = name
        self.roll_number = roll_number
```

Each student needs its own values.

### Rule

> **If the data describes an individual object's state, use an instance attribute.**

---

# 6.15 When Should You Use Class Attributes?

Use a class attribute when the value is conceptually associated with the class and can be shared across instances.

Examples:

```text
Student:
    college_name

BankAccount:
    minimum_balance

Employee:
    company_name
```

Example:

```python
class Student:

    college = "TIT Technocrats"
```

Every student in this model belongs to the same college.

Another common example:

```python
class Circle:

    PI = 3.14159
```

`PI` is not specific to one circle object.

---

# 6.16 Class Attributes as Shared State

Class attributes can also represent shared state.

Example:

```python
class Student:

    count = 0

    def __init__(self, name):
        self.name = name
        Student.count += 1
```

Create:

```python
student1 = Student("Abhijit")
student2 = Student("Rahul")
student3 = Student("Priya")
```

Now:

```python
print(Student.count)
```

Output:

```text
3
```

Here `count` belongs to the class and tracks a value shared across the class's instances.

### Important

If you are intentionally maintaining shared mutable state, be careful. Shared class-level state can cause unexpected interactions between objects.

---

# 6.17 Mutable Class Attribute Trap

This is a very important interview trap.

Consider:

```python
class Student:

    subjects = []
```

Now:

```python
student1 = Student()
student2 = Student()

student1.subjects.append("Python")
```

You might expect only `student1` to have `"Python"`.

But:

```python
print(student2.subjects)
```

may also show:

```text
['Python']
```

Why?

Because both instances are resolving `subjects` to the **same class-level list**.

Conceptually:

```text
Student
└── subjects ──→ []

student1 ──────┐
                ├──→ same list
student2 ──────┘
```

### Usually correct approach

If each object needs its own list:

```python
class Student:

    def __init__(self):
        self.subjects = []
```

Now:

```text
student1.subjects → separate list
student2.subjects → separate list
```

### Important rule

> **Don't use a mutable class attribute when every instance is supposed to have its own independent mutable state.**

---

# 6.18 Complete Example

```python
class Student:

    college = "TIT Technocrats"   # class attribute
    student_count = 0             # class attribute

    def __init__(self, name, age):
        self.name = name           # instance attribute
        self.age = age             # instance attribute

        Student.student_count += 1
```

Create:

```python
student1 = Student("Abhijit", 25)
student2 = Student("Rahul", 22)
```

State conceptually:

```text
Student class
│
├── college → "TIT Technocrats"
└── student_count → 2


student1
├── name → "Abhijit"
└── age  → 25


student2
├── name → "Rahul"
└── age  → 22
```

Notice:

* `name` differs → instance attribute
* `age` differs → instance attribute
* `college` is common → class attribute
* `student_count` is shared → class attribute

---

# 6.19 Instance Attribute vs Class Attribute

|                  | Instance Attribute    | Class Attribute               |
| ---------------- | --------------------- | ----------------------------- |
| Belongs to       | Individual instance   | Class                         |
| Typical syntax   | `self.name`           | `ClassName.name`              |
| Defined commonly | `__init__()`          | Directly in class body        |
| Stored normally  | Instance `__dict__`   | Class namespace               |
| Values           | Can differ per object | Shared unless overridden      |
| Use for          | Object-specific state | Class-wide/shared information |
| Example          | `self.name`           | `Student.college`             |

---

# 6.20 Important Lookup Mental Model

When you write:

```python
student.name
```

think:

```text
student
  ↓
Does instance have "name"?
  │
  ├── Yes → use it
  │
  └── No → continue lookup through class/inheritance
```

For:

```python
student.college
```

if:

```python
student.__dict__
```

doesn't contain `college`, Python can find:

```python
Student.college
```

So:

```text
object.attribute
       ↓
instance lookup
       ↓
class lookup
       ↓
base classes / MRO
```

This is a simplified model; Python's complete lookup mechanism also involves descriptors.

---

# 6.21 Common Mistakes & Traps

### Trap 1: Assuming class attributes are copied into every object

If:

```python
class Student:
    college = "TIT"
```

Python does not simply copy `"TIT"` into every instance's `__dict__`.

Instances can resolve the attribute through the class.

---

### Trap 2: Changing an instance attribute changes the class

```python
student1.college = "RGPV"
```

does **not** normally change:

```python
Student.college
```

It creates/changes an instance attribute.

To change the class attribute:

```python
Student.college = "RGPV"
```

---

### Trap 3: Mutable class attributes

Avoid:

```python
class Student:
    subjects = []
```

when every student needs an independent list.

Prefer:

```python
class Student:

    def __init__(self):
        self.subjects = []
```

---

### Trap 4: Using class attributes for object-specific data

Bad design:

```python
class Student:
    name = "Unknown"
```

if every student should have a different name.

Better:

```python
class Student:

    def __init__(self, name):
        self.name = name
```

---

# 6.22 Interview Questions

### What is the difference between instance and class attributes?

> **Instance attributes belong to individual objects and can have different values for each instance. Class attributes are defined on the class and can be shared by instances unless an instance provides its own attribute with the same name.**

### Where are instance attributes stored?

> **For ordinary Python objects, instance attributes are commonly stored in the object's `__dict__`.**

### Where are class attributes stored?

> **Class attributes are stored in the class's namespace, accessible through the class object's `__dict__`.**

### Can an instance access a class attribute?

> **Yes. If the attribute isn't found on the instance, Python can find it on the class or its inheritance hierarchy.**

### What happens if an instance assigns to a class attribute name?

For example:

```python
student.college = "RGPV"
```

> **It normally creates an instance attribute named `college` rather than modifying the class attribute. That instance-level value shadows the class-level value for that object.**

### When should you use a class attribute?

> **Use a class attribute when the value is conceptually associated with the class or intentionally shared across instances.**

---

# 6.23 Backend / GenAI Relevance

This distinction becomes useful when designing real applications.

For example:

```python
class LLMClient:

    provider = "OpenAI"      # class-level information

    def __init__(self, model, api_key):
        self.model = model   # instance-specific
        self.api_key = api_key
```

Conceptually:

```text
LLMClient
│
└── provider → common class information

client1
├── model
└── api_key

client2
├── model
└── api_key
```

Each client can have different configuration, while some information can be shared at the class level.

In backend/GenAI development, however, **don't put sensitive or mutable request-specific state into class attributes just because it is convenient**. Shared state can unintentionally affect multiple requests or objects.

---

# 6.24 Final Mental Model

Remember this picture:

```text
                 CLASS
        ┌─────────────────────┐
        │ college = "TIT"     │  ← Class Attribute
        │ count = 100         │  ← Class Attribute
        └─────────────────────┘
                  │
        ┌─────────┴─────────┐
        ↓                   ↓
    OBJECT 1            OBJECT 2
 ┌──────────────┐    ┌──────────────┐
 │ name = A     │    │ name = R     │
 │ age = 25     │    │ age = 22     │
 └──────────────┘    └──────────────┘
    ↑ Instance          ↑ Instance
      Attributes          Attributes
```

### The two rules to remember

```python
self.name
```

> **Instance attribute → belongs to this particular object.**

```python
Student.college
```

> **Class attribute → belongs to the class and can be shared through instances.**

### One-line memory trick

> **Instance attributes describe “this object”; class attributes describe something associated with “the class.”**

---

## Priority

| Concept                                 | Priority           |
| --------------------------------------- | ------------------ |
| Instance attributes                     | 🔥 Core            |
| Class attributes                        | 🔥 Core            |
| Instance vs class attributes            | 🔥 Core            |
| Where instance attributes are stored    | 🔥 Core            |
| Where class attributes are stored       | 🔥 Core            |
| Attribute lookup                        | 🔥 Core            |
| Instance shadowing class attribute      | 🔥 Core            |
| When to use each                        | 🔥 Core            |
| Mutable class attribute trap            | 🔥 Core            |
| `__dict__` details                      | 🟡 Know & Move On  |
| Descriptors / advanced lookup internals | ⚪ Optional for Now |
