# 1. What is OOP? 🔥 Core

## 1.1 What is OOP?

**OOP (Object-Oriented Programming)** is a programming paradigm where we organize software around **objects** that contain:

* **Data** → attributes/state
* **Behavior** → methods/functions

### Simple idea

Instead of thinking only:

> “What steps should the program perform?”

OOP encourages us to think:

> “What objects exist, what data do they have, and what can they do?”

For example, in a banking application:

```text
BankAccount
    │
    ├── Data
    │    ├── account_number
    │    ├── owner
    │    └── balance
    │
    └── Behavior
         ├── deposit()
         ├── withdraw()
         └── check_balance()
```

An actual bank account would be an **object** created from this design.

---

# 1.2 Why OOP?

As programs become larger, keeping everything as independent variables and functions can become difficult to manage.

OOP helps organize related data and behavior together.

### Without OOP

You might have:

```python
account_name = "Abhijit"
balance = 5000

def deposit(balance, amount):
    return balance + amount

def withdraw(balance, amount):
    return balance - amount
```

As the application grows, you may end up managing many separate variables and functions.

### With OOP

We can model the account as an object:

```python
class BankAccount:

    def __init__(self, owner, balance):
        self.owner = owner
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount

    def withdraw(self, amount):
        self.balance -= amount
```

Then:

```python
account = BankAccount("Abhijit", 5000)

account.deposit(1000)
account.withdraw(500)
```

The account's **data and behavior are grouped together**.

---

# 1.3 Main Idea of OOP

A useful mental model is:

```text
             OBJECT
          ┌───────────┐
          │   Data    │
          │           │
          │ Attributes│
          ├───────────┤
          │ Behavior  │
          │           │
          │  Methods  │
          └───────────┘
```

For a `Car` object:

```text
Car
│
├── Data
│   ├── brand
│   ├── color
│   └── speed
│
└── Behavior
    ├── start()
    ├── accelerate()
    └── brake()
```

The exact design depends on the application.

---

# 1.4 Procedural Programming vs OOP

### Procedural approach

Procedural programming focuses primarily on:

> **Functions and the sequence of operations.**

Example:

```python
balance = 5000

def deposit(balance, amount):
    return balance + amount

balance = deposit(balance, 1000)
```

The data and functions are handled separately.

---

### OOP approach

OOP focuses on:

> **Objects that combine data and behavior.**

```python
class BankAccount:

    def __init__(self, balance):
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount
```

Now:

```python
account = BankAccount(5000)
account.deposit(1000)
```

### Comparison

| Procedural                              | OOP                                  |
| --------------------------------------- | ------------------------------------ |
| Focuses on functions/procedures         | Focuses on objects                   |
| Data and functions often separate       | Data and behavior are grouped        |
| Good for smaller/simple programs        | Useful for larger/complex systems    |
| Function-oriented organization          | Object-oriented organization         |
| State is often passed between functions | Objects can maintain their own state |

**Important:** This does not mean procedural programming is bad or that every program should use OOP. The appropriate approach depends on the problem.

---

# 1.5 Real-World Analogy

Think about a **Student**.

A student has:

### Attributes — what the student has/is

```text
name
age
college
roll_number
```

### Methods — what the student can do

```text
study()
attend_class()
submit_assignment()
```

So we can think:

```text
Student
│
├── Attributes
│   ├── name
│   ├── age
│   └── college
│
└── Methods
    ├── study()
    ├── attend_class()
    └── submit_assignment()
```

An individual student becomes an **object**:

```text
Student Class
      │
      ├── Object 1 → Abhijit
      ├── Object 2 → Rahul
      └── Object 3 → Priya
```

The class acts as the **blueprint/design**, while objects are actual instances created from it.

> We'll study **classes and objects** in the next OOP topic.

---

# 1.6 When is OOP Useful?

OOP becomes particularly useful when your application contains many entities with their own:

* state
* behavior
* relationships
* reusable logic

Examples:

### E-commerce

```text
User
Product
Cart
Order
Payment
```

### Banking

```text
Customer
BankAccount
Transaction
Loan
```

### College Management

```text
Student
Teacher
Course
Department
Exam
```

### Backend Applications

```text
User
Request
Response
Database
Service
Repository
```

### AI / GenAI Applications

OOP can be useful for modeling components such as:

```text
LLMClient
EmbeddingModel
Document
VectorStore
Retriever
Agent
Tool
```

For example:

```python
class LLMClient:

    def generate(self, prompt):
        ...
```

Then different parts of an application can interact with an object representing the LLM client.

---

# 1.7 Benefits of OOP

### 1. Organization

Related data and behavior can be grouped together.

### 2. Reusability

Classes can be reused to create multiple objects.

### 3. Maintainability

Large applications can be divided into logical components.

### 4. Encapsulation

An object's internal state and implementation can be controlled through its interface.

### 5. Extensibility

Existing designs can be extended using concepts such as inheritance and composition.

### 6. Abstraction

Complex implementation details can be hidden behind simpler interfaces.

These concepts will become clearer as we study:

```text
Class & Object
      ↓
Attributes & Methods
      ↓
Constructor
      ↓
Encapsulation
      ↓
Inheritance
      ↓
Polymorphism
      ↓
Abstraction
      ↓
Composition
```

---

# 1.8 Important Terminology

| Term          | Meaning                                               |
| ------------- | ----------------------------------------------------- |
| OOP           | Object-Oriented Programming                           |
| Object        | An instance containing state/data and behavior        |
| Class         | A blueprint/definition used to create objects         |
| Attribute     | Data/state associated with an object or class         |
| Method        | Function defined as part of a class                   |
| Instance      | A particular object created from a class              |
| Encapsulation | Bundling data/behavior and controlling access         |
| Inheritance   | Creating a class based on another class               |
| Polymorphism  | Same interface/operation working with different types |
| Abstraction   | Hiding unnecessary implementation details             |

Don't try to memorize all of these yet. We'll study each one separately.

---

# 1.9 OOP Is a Programming Paradigm

OOP is a **programming paradigm**, meaning a way of structuring and thinking about programs.

Python supports multiple programming styles, including:

```text
Procedural
Functional
Object-Oriented
```

Python is therefore **multi-paradigm**.

You are not required to make every Python program fully object-oriented.

For a small script:

```python
name = "Abhijit"
print(f"Hello {name}")
```

using a class would often be unnecessary.

For a larger application with many interacting entities, OOP may provide useful structure.

---

# 1.10 Common Confusion

### OOP ≠ Just Classes

Classes are an important mechanism for OOP, but OOP is broader than simply writing classes.

It involves concepts such as:

* objects
* encapsulation
* abstraction
* inheritance
* polymorphism
* composition

---

### Class ≠ Object

A class is the definition/blueprint.

An object is an actual instance.

```python
class Car:
    pass

car1 = Car()
car2 = Car()
```

Here:

```text
Car   → class
car1  → object
car2  → object
```

---

### OOP ≠ Always Better

OOP is a tool, not a rule.

Use the programming style that makes the particular problem easier to understand, maintain, and extend.

---

# 1.11 Interview Answer

### What is OOP?

> **OOP, or Object-Oriented Programming, is a programming paradigm that organizes software around objects containing data and behavior. In Python, classes are used to define the structure and behavior of objects. OOP helps organize larger programs through concepts such as encapsulation, inheritance, polymorphism, abstraction, and composition.**

### Why is OOP used?

> **OOP helps structure complex applications by grouping related state and behavior, improving organization, reuse, maintainability, and extensibility.**

### Is Python purely object-oriented?

> **No. Python is a multi-paradigm language. It supports object-oriented, procedural, and functional programming styles.**

---

# 1.12 Backend / GenAI Relevance

For your **Python → Backend → GenAI/Agentic AI** path, OOP is worth understanding deeply because real applications often contain many interacting components.

For example:

```text
GenAI Application
│
├── LLM Client
├── Prompt Manager
├── Document Loader
├── Embedding Model
├── Vector Store
├── Retriever
├── Tool
└── Agent
```

Each component can potentially be represented by a class/object with its own state and behavior.

You don't need to force everything into classes, but understanding OOP will make larger Python codebases and frameworks much easier to read.

---

# 1.13 Final Mental Model

Remember this:

```text
OOP
│
├── Organize program around OBJECTS
│
├── Object
│   ├── Data → Attributes
│   └── Behavior → Methods
│
├── Class
│   └── Defines how objects are structured
│
└── Main Concepts
    ├── Encapsulation
    ├── Abstraction
    ├── Inheritance
    ├── Polymorphism
    └── Composition
```

### One-line memory trick

> **Object = data + behavior**

---

## Priority

| Concept                      | Priority          |
| ---------------------------- | ----------------- |
| What is OOP?                 | 🔥 Core           |
| Why OOP?                     | 🔥 Core           |
| Object-oriented thinking     | 🔥 Core           |
| Procedural vs OOP            | 🔥 Core           |
| Real-world analogy           | 🔥 Core           |
| When OOP is useful           | 🔥 Core           |
| OOP terminology              | 🔥 Core           |
| Benefits of OOP              | 🟡 Know & Move On |
| Detailed paradigm comparison | 🟡 Know & Move On |
