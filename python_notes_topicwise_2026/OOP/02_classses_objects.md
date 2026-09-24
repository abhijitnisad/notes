# 2. Class & Object 🔥 Core

## 2.1 What is a Class?

A **class** is a definition that describes the **data and behavior** that objects of that type can have.

It defines things such as:

* what data an object can contain
* what operations/behavior it can perform

Example:

```python
class Student:
    pass
```

Here, `Student` is a **class**.

But no actual student object has been created yet.

### Mental Model

Think of a class as a **design/template**:

```text
Class: Student
       │
       ├── Data
       │   ├── name
       │   ├── age
       │   └── branch
       │
       └── Behavior
           ├── study()
           └── attend_class()
```

The class describes what a `Student` object can have and do.

---

# 2.2 What is an Object?

An **object** is an actual instance of a class.

If `Student` is the class:

```python
class Student:
    pass
```

we can create objects:

```python
student1 = Student()
student2 = Student()
```

Here:

```text
Student    → class
student1   → object
student2   → object
```

Both objects are instances of `Student`.

### Important

A class is the **definition**.

An object is an **actual instance** created from that definition.

---

# 2.3 Class as a Blueprint

A common analogy is:

> **Class = blueprint**
>
> **Object = actual thing built from the blueprint**

For example, imagine a house blueprint:

```text
House Blueprint
      │
      ├── rooms
      ├── doors
      ├── windows
      └── structure
```

The blueprint isn't an actual house.

It describes how houses can be constructed.

From the same blueprint, you can build multiple houses:

```text
             House Class
             (Blueprint)
                  │
       ┌──────────┼──────────┐
       ↓          ↓          ↓
    House 1    House 2    House 3
```

Similarly:

```text
             Student Class
              (Definition)
                   │
       ┌───────────┼───────────┐
       ↓           ↓           ↓
   student1     student2     student3
    object        object       object
```

Each object can have its own data.

---

# 2.4 Creating a Class

Basic syntax:

```python
class ClassName:
    # attributes
    # methods
    pass
```

Example:

```python
class Student:
    pass
```

### Naming Convention

Python convention is:

```python
class Student:
    pass

class BankAccount:
    pass

class LLMClient:
    pass
```

Use **PascalCase / CapWords** for class names.

---

# 2.5 Creating Objects

Objects are created by **calling the class**:

```python
student1 = Student()
```

This may look like calling a function, but `Student` is a class.

```text
Student()
   ↓
creates an object
   ↓
object assigned to student1
```

Example:

```python
class Student:
    pass


student1 = Student()
student2 = Student()
```

Now:

```python
print(student1)
print(student2)
```

will show representations containing their object identities, typically something like:

```text
<__main__.Student object at 0x...>
<__main__.Student object at 0x...>
```

The exact representation/address is not something you should rely on.

---

# 2.6 What Actually Happens When We Create an Object?

Consider:

```python
class Student:
    pass

student1 = Student()
```

Conceptually:

### Step 1 — Python has the class

```text
Student
   ↓
class object
```

The `class` statement creates a class object and binds the name `Student` to it.

### Step 2 — We call the class

```python
Student()
```

Python creates an instance of that class.

### Step 3 — Reference is stored

```python
student1 = Student()
```

The name `student1` now refers to that object.

Conceptually:

```text
Student
   │
   │ creates
   ↓
Student object
   ↑
   │
student1
```

This distinction is important:

> `student1` is a **name/reference** pointing to an object; the object itself is the instance of `Student`.

---

# 2.7 Multiple Objects from One Class

One class can create many independent objects.

```python
class Student:
    pass


student1 = Student()
student2 = Student()
student3 = Student()
```

Conceptually:

```text
             Student
               Class
                │
       ┌────────┼────────┐
       ↓        ↓        ↓
   Object 1  Object 2  Object 3
      ↑          ↑         ↑
  student1   student2  student3
```

The objects are separate instances.

This is one of the major advantages of classes.

---

# 2.8 Objects Can Have Different State

Let's give each object some data.

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

Both objects belong to the same class:

```python
type(student1)
type(student2)
```

but they contain different instance data.

Conceptually:

```text
student1
┌─────────────────┐
│ name = Abhijit  │
└─────────────────┘

student2
┌─────────────────┐
│ name = Rahul    │
└─────────────────┘
```

This **state** is stored separately for each object.

> We'll study proper attribute creation and `self` in the next topics.

---

# 2.9 Class vs Object

| Class                             | Object                                |
| --------------------------------- | ------------------------------------- |
| Definition/blueprint              | Actual instance                       |
| Describes structure/behavior      | Contains actual state                 |
| Used to create objects            | Created from a class                  |
| Example: `Student`                | Example: `student1`                   |
| One class can create many objects | Each object is an individual instance |

### Simple analogy

```text
Class  → Student
Object → Abhijit
Object → Rahul
Object → Priya
```

---

# 2.10 Class vs Object in Code

```python
class Car:
    pass

car1 = Car()
car2 = Car()
```

Here:

```text
Car
 ↓
Class

car1
 ↓
Object / Instance

car2
 ↓
Object / Instance
```

You can verify this:

```python
print(type(car1))
```

Conceptually:

```text
<class '__main__.Car'>
```

And:

```python
print(isinstance(car1, Car))
```

gives:

```text
True
```

`isinstance()` checks whether an object is an instance of a particular class (including relevant inheritance relationships).

---

# 2.11 A More Realistic Example

Let's model a bank account:

```python
class BankAccount:
    pass


account1 = BankAccount()
account2 = BankAccount()
```

We have:

```text
BankAccount
     │
     ├── account1
     └── account2
```

Later, the class can define:

```text
Data:
    owner
    balance

Behavior:
    deposit()
    withdraw()
```

So eventually:

```python
class BankAccount:

    def deposit(self):
        ...

    def withdraw(self):
        ...
```

The class defines what a bank-account object **can do**.

---

# 2.12 A Class Is Also an Object in Python

This is an important Python concept.

In Python, **classes themselves are objects too**.

For example:

```python
class Student:
    pass
```

`Student` refers to a class object.

You can inspect it:

```python
print(type(Student))
```

For normal Python classes, you'll typically see:

```text
<class 'type'>
```

So conceptually:

```text
Student
   ↓
class object
   ↓
instance of type
```

While:

```python
student1 = Student()
```

gives:

```text
student1
   ↓
Student instance
```

Don't worry about `type`/metaclasses deeply yet. Just remember:

> **In Python, classes are objects too.**

We'll return to this when discussing Python's object model.

---

# 2.13 Important Terminology

| Term           | Meaning                                             |
| -------------- | --------------------------------------------------- |
| Class          | Definition describing a type of object              |
| Object         | An actual instance of a class                       |
| Instance       | Another term for an object created from a class     |
| Instance state | Data belonging to a particular object               |
| Class object   | The object created by executing a `class` statement |
| Instantiate    | Create an object from a class                       |

For example:

```python
student = Student()
```

means:

> We **instantiate** the `Student` class to create a `Student` object.

---

# 2.14 Common Confusions

### Confusion 1: Class and object are the same

They are not.

```text
Student → class
student1 → object
```

---

### Confusion 2: Class is just a blueprint

"Blueprint" is a useful beginner mental model, but Python classes are more than passive blueprints.

A class is an actual Python **class object** that defines behavior and participates in Python's object model.

For learning OOP initially, however:

> **Class = definition/template**
>
> **Object = instance created from it**

is a good mental model.

---

### Confusion 3: `Student` vs `Student()`

Very important:

```python
Student
```

refers to the **class object**.

```python
Student()
```

creates an **instance** of that class.

Similar to the function distinction:

```python
func      # function object
func()    # call the function
```

You can think:

```text
Student    → class
Student()  → create an instance
```

---

### Confusion 4: `student1` is the object itself

At the beginner level we commonly say:

> "`student1` is an object."

More precisely:

> `student1` is a **name/reference bound to a Student instance**.

This distinction becomes important when learning Python's object model, mutability, and references.

---

# 2.15 Interview Questions

### What is a class?

> **A class is a Python definition that describes the structure and behavior of objects. It defines attributes and methods that instances of the class can have.**

### What is an object?

> **An object is an instance of a class. It is a concrete entity with its own state and access to the behavior defined by its class.**

### What is the difference between a class and an object?

> **A class defines what an object should look like and what it can do, while an object is an actual instance created from that class. A single class can be used to create many independent objects.**

### What does `Student()` do?

> **`Student()` creates an instance of the `Student` class. The resulting object can then be assigned to a name such as `student1`.**

### Can a class create multiple objects?

> **Yes. A single class can be instantiated multiple times, and each instance can maintain its own state.**

---

# 2.16 Backend / GenAI Relevance

Classes become particularly useful as your Python applications become larger.

For example, a GenAI application might have:

```text
Application
│
├── LLMClient
├── Document
├── EmbeddingModel
├── VectorStore
├── Retriever
├── Tool
└── Agent
```

Each can be represented by a class when that design makes sense.

For example:

```python
class LLMClient:

    def generate(self, prompt):
        # call an LLM
        ...
```

Then:

```python
client = LLMClient()
```

Here:

```text
LLMClient → class
client    → object
```

Later, you'll learn how `__init__`, `self`, methods, inheritance, composition, and other OOP concepts make these objects useful in real applications.

---

# 2.17 Final Mental Model

Remember this chain:

```text
CLASS
  │
  │ defines
  ↓
Structure + Behavior
  │
  │ instantiate
  ↓
OBJECT
  │
  ├── State / Data
  └── Behavior
```

Example:

```python
class Student:
    pass

student1 = Student()
student2 = Student()
```

Think:

```text
             Student
               │
             CLASS
               │
       ┌───────┴───────┐
       ↓               ↓
   student1         student2
    OBJECT            OBJECT
```

### One-line memory trick

> **Class = definition; Object = instance.**

And an even more precise Python mental model:

> **A class defines a type; an object is an instance of that type.**

---

## Priority

| Concept                         | Priority           |
| ------------------------------- | ------------------ |
| What is a class?                | 🔥 Core            |
| What is an object?              | 🔥 Core            |
| Class vs object                 | 🔥 Core            |
| Creating a class                | 🔥 Core            |
| Creating objects                | 🔥 Core            |
| Class as a blueprint            | 🔥 Core            |
| Multiple objects from one class | 🔥 Core            |
| Object state                    | 🔥 Core            |
| `Student` vs `Student()`        | 🔥 Core            |
| `isinstance()`                  | 🟡 Know & Move On  |
| Classes are objects / `type`    | 🟡 Know & Move On  |
| Metaclasses                     | ⚪ Optional for Now |
