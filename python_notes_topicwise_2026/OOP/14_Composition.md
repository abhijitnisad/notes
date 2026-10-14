# 14. Composition 🔥🔥 Core

## 14.1 What Is Composition?

**Composition** is an OOP technique in which one object contains or owns another object to provide its functionality.

It represents a **"Has-a" relationship**.

For example:

- A `Car` has an `Engine`.
- A `Computer` has a `CPU`.
- A `House` has `Room` objects.
- An AI application has an `LLMClient`.

### Real-world mental model

```text
Car
 ├── Engine
 ├── Battery
 └── Transmission
```

A car is not an engine. It *has* an engine.

In Python, we can represent this relationship by creating an object of one class inside another class.

## 14.2 Creating an Object Inside Another Object

Example:

```python
class Engine:

    def start(self):
        print("Engine started")


class Car:

    def __init__(self):
        self.engine = Engine()

    def start(self):
        self.engine.start()
        print("Car is ready")
```

Create and use the car:

```python
car = Car()
car.start()
```

Output:

```text
Engine started
Car is ready
```

### How it works

1. `Engine` defines engine-related behavior.
2. `Car` creates an `Engine` object inside `__init__()`.
3. `self.engine` stores a reference to that engine object.
4. `Car.start()` delegates engine startup to `self.engine.start()`.

Object relationship:

```text
car ──references──> Car object
                       │
                       └── engine ──> Engine object
```

**Important:** `self.engine = Engine()` stores a reference to the created engine object. It doesn't copy the `Engine` class into the `Car` object.

## 14.3 Composition With Dependency Injection

The component doesn't always need to create its own dependency. We can pass the object into the constructor instead.

```python
class Engine:

    def start(self):
        print("Engine started")


class Car:

    def __init__(self, engine):
        self.engine = engine

    def start(self):
        self.engine.start()
        print("Car is ready")


engine = Engine()
car = Car(engine)

car.start()
```

Output:

```text
Engine started
Car is ready
```

Here:

```python
car = Car(engine)
```

passes an existing object into `Car`.

This pattern is called **dependency injection**: an object receives the dependency it needs rather than creating it internally.

Why is this useful?

- You can replace the engine implementation.
- You can test `Car` with a fake engine.
- You can configure dependencies independently.
- The `Car` class becomes less tightly coupled to a specific engine implementation.

Composition and dependency injection are closely related, but they are not identical: composition is the object relationship; dependency injection is one way to supply the related object.

## 14.4 Composition vs Inheritance

This is the most important distinction.

### Inheritance — "Is-a"

```python
class Animal:

    def eat(self):
        print("Eating")


class Dog(Animal):
    pass


dog = Dog()
dog.eat()
```

Relationship:

```text
Dog IS AN Animal
```

`Dog` inherits the behavior of `Animal`.

### Composition — "Has-a"

```python
class Battery:

    def charge(self):
        print("Battery charging")


class Phone:

    def __init__(self):
        self.battery = Battery()

    def charge(self):
        self.battery.charge()
```

Relationship:

```text
Phone HAS A Battery
```

`Phone` uses a separate `Battery` object.

### Comparison

| Inheritance | Composition |
|---|---|
| Represents an "is-a" relationship | Represents a "has-a" relationship |
| Child inherits from a parent | Object holds references to other objects |
| Reuses or specializes inherited behavior | Combines functionality from separate objects |
| Can create tight coupling to a base class | Often allows more flexible component replacement |
| Useful for genuine subtype relationships | Useful for assembling objects with different responsibilities |

## 14.5 Why Is Composition Useful?

### 1. Reusability

One component can be used by several different classes.

```python
class Logger:

    def log(self, message):
        print(f"LOG: {message}")


class UserService:

    def __init__(self, logger):
        self.logger = logger

    def create_user(self):
        self.logger.log("User created")


class PaymentService:

    def __init__(self, logger):
        self.logger = logger

    def process_payment(self):
        self.logger.log("Payment processed")
```

Both services reuse `Logger` without inheriting from it.

### 2. Flexibility

You can change the object that provides a service without redesigning the class hierarchy.

```python
class EmailNotifier:

    def send(self, message):
        print(f"Email: {message}")


class SMSNotifier:

    def send(self, message):
        print(f"SMS: {message}")


class AlertService:

    def __init__(self, notifier):
        self.notifier = notifier

    def alert(self, message):
        self.notifier.send(message)
```

Usage:

```python
email_alert = AlertService(EmailNotifier())
sms_alert = AlertService(SMSNotifier())

email_alert.alert("Server is down")
sms_alert.alert("Server is down")
```

The `AlertService` doesn't need to inherit from `EmailNotifier` or `SMSNotifier`. It uses whichever compatible object it receives.

### 3. Easier testing

You can pass a fake dependency during testing.

```python
class FakeNotifier:

    def send(self, message):
        print(f"Test notification: {message}")


service = AlertService(FakeNotifier())
service.alert("Test message")
```

This makes it easier to test application logic without sending real emails or SMS messages.

### 4. Separation of responsibilities

Each class can focus on one job:

```text
AlertService
    └── decides when to alert

EmailNotifier
    └── sends email

SMSNotifier
    └── sends SMS
```

This is easier to maintain than putting every responsibility into one large class.

## 14.6 Composition in Backend and GenAI

Suppose you're building a Python application that generates answers using an LLM.

```python
class Retriever:

    def retrieve(self, question):
        return ["Relevant document"]


class LLMClient:

    def generate(self, question, context):
        return f"Answer to: {question}"


class RAGApplication:

    def __init__(self, retriever, llm):
        self.retriever = retriever
        self.llm = llm

    def answer(self, question):
        context = self.retriever.retrieve(question)
        return self.llm.generate(question, context)


retriever = Retriever()
llm = LLMClient()

app = RAGApplication(retriever, llm)

print(app.answer("What is Python?"))
```

Output:

```text
Answer to: What is Python?
```

Object structure:

```text
RAGApplication
 ├── Retriever
 │    └── retrieve()
 └── LLMClient
      └── generate()
```

The `RAGApplication` coordinates the work while the other objects handle their individual responsibilities.

This is a simplified example, but the same design idea appears in backend services, API clients, retrieval systems, and agentic AI applications.

## 14.7 Composition vs Inheritance: How to Choose

Ask yourself:

**Question 1: Is the child genuinely a specialized type of the parent?**

Example: `Dog` is an `Animal`.

Inheritance may be appropriate.

**Question 2: Does the class need another object's functionality?**

Example: `Car` uses an `Engine`.

Composition is usually more appropriate.

**Question 3: Might the dependency need to be replaced or mocked?**

Composition with dependency injection often makes that easier.

### General guideline

> **Favor composition over inheritance when you want to combine functionality without creating a genuine "is-a" relationship.**

This is a design guideline, not an absolute rule. Inheritance remains useful when the subtype relationship is meaningful and the child can honor the parent's expected behavior.

## 14.8 Does Composition Mean the Objects Must Have the Same Lifetime?

Not necessarily.

Composition is commonly described as a strong "has-a" relationship, but Python itself does not enforce ownership or lifetime rules.

For example:

```python
engine = Engine()
car = Car(engine)
```

The same `engine` object could also be referenced by another variable or object.

If a component can exist independently and is shared, the relationship may be better described as **aggregation** in some OOP models. The terminology varies between languages and design methods.

For your current level, focus on the practical idea: one object holds a reference to and uses another object.

## 14.9 Common Mistakes

**Mistake 1: Confusing "is-a" and "has-a"**

- `Dog` is an `Animal` → inheritance.
- `Car` has an `Engine` → composition.

**Mistake 2: Assuming composition requires creating the dependency inside `__init__()`**

You can create it internally:

```python
self.engine = Engine()
```

Or receive it from outside:

```python
self.engine = engine
```

Both can form a composition-style design.

**Mistake 3: Thinking composition always means ownership**

An object reference alone doesn't guarantee exclusive ownership or a shared lifetime.

**Mistake 4: Using inheritance just to reuse a few methods**

If the relationship isn't genuinely "is-a," composition may produce a cleaner design.

## 14.10 Interview Questions

**What is composition in Python?**

Composition is an OOP technique in which an object contains references to other objects and uses their functionality to perform its work.

**What is a "has-a" relationship?**

It describes a relationship where one object contains or uses another object, such as a `Car` having an `Engine`.

**What is the difference between composition and inheritance?**

Inheritance models an "is-a" relationship and reuses or specializes parent behavior. Composition models a "has-a" relationship by combining objects.

**Why is composition often preferred over inheritance?**

It can reduce tight coupling to base classes, improve flexibility, separate responsibilities, and make dependencies easier to replace and test.

**What is dependency injection?**

Dependency injection is a technique in which an object receives the dependencies it needs from outside instead of creating them internally.

## Final Mental Model

```text
Inheritance
    ↓
"Is-a"
    ↓
Dog inherits from Animal


Composition
    ↓
"Has-a"
    ↓
Car uses an Engine
```

**One-line interview memory:**

> Composition is an OOP technique in which an object uses other objects to build its functionality, representing a "has-a" relationship and often improving flexibility and testability.

## Priority

| Concept | Priority |
|---|---|
| "Has-a" relationship | 🔥🔥 Core |
| Object inside another object | 🔥🔥 Core |
| Composition vs inheritance | 🔥🔥 Core |
| Why composition is useful | 🔥🔥 Core |
| Dependency injection | 🔥 Core |
| Composition in backend/GenAI | 🔥 Core |
| Aggregation and object lifetime terminology | ⚪ Optional for Now |