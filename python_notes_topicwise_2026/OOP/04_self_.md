# 4. `self` 🔥 Core

## 4.1 What is `self`?

`self` is the conventional name for the **first parameter of an instance method**.

It refers to the **current instance/object** on which the method was called.

Example:

```python
class Student:

    def introduce(self):
        print(self)
```

Create an object:

```python
student = Student()
student.introduce()
```

When `student.introduce()` is called, `self` refers to `student`.

Conceptually:

```text
student.introduce()
        ↓
self → student
```

So:

> **`self` gives a method access to the particular object that called the method.**

---

# 4.2 Why Do We Need `self`?

Suppose we have multiple objects:

```python
class Student:

    def __init__(self, name):
        self.name = name

    def introduce(self):
        print(f"My name is {self.name}")
```

Create two objects:

```python
student1 = Student("Abhijit")
student2 = Student("Rahul")
```

Now:

```python
student1.introduce()
student2.introduce()
```

Output:

```text
My name is Abhijit
My name is Rahul
```

How does the same `introduce()` method know whether it should use:

```text
student1.name
```

or:

```text
student2.name
```

Because Python supplies the object as the first argument, which we conventionally call `self`.

```text
student1.introduce()
        ↓
self → student1
        ↓
self.name → student1.name → "Abhijit"


student2.introduce()
        ↓
self → student2
        ↓
self.name → student2.name → "Rahul"
```

### Core idea

> **`self` connects the method to the specific object whose state it should work with.**

---

# 4.3 `self` Is Not a Keyword

An important Python detail:

`self` is **not a Python keyword**.

It is a naming convention.

You technically could write:

```python
class Student:

    def introduce(this):
        print(this)
```

and:

```python
student = Student()
student.introduce()
```

would work.

But you should **always use `self`** for the conventional first parameter of an instance method.

Why?

Because:

* Python community expects it
* code becomes easier to read
* tools and documentation commonly use it
* it makes the purpose immediately clear

So:

```python
def introduce(self):
```

is the correct conventional style.

---

# 4.4 How Python Passes `self`

Consider:

```python
class Student:

    def introduce(self):
        print("Hello")


student = Student()

student.introduce()
```

It is useful to understand this approximately as:

```python
Student.introduce(student)
```

In other words:

```text
student.introduce()
        ↓
Python supplies student
        ↓
Student.introduce(student)
        ↓
self = student
```

This is the key mechanism behind `self`.

### Very important

When you write:

```python
student.introduce()
```

you normally **do not explicitly pass `student`**.

Python supplies it automatically for an instance-method call.

---

# 4.5 `self` and Instance Attributes

The most common use of `self` is accessing an object's instance attributes.

Example:

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age
```

Here:

```python
self.name
```

means:

> the `name` attribute belonging to the current object.

And:

```python
self.age
```

means:

> the `age` attribute belonging to the current object.

If:

```python
student1 = Student("Abhijit", 25)
```

then conceptually:

```text
self → student1

self.name
   ↓
student1.name

self.age
   ↓
student1.age
```

If:

```python
student2 = Student("Rahul", 22)
```

then:

```text
self → student2

self.name
   ↓
student2.name
```

---

# 4.6 `self.attribute`

The general syntax is:

```python
self.attribute
```

Example:

```python
class BankAccount:

    def __init__(self, owner, balance):
        self.owner = owner
        self.balance = balance
```

Here:

```python
self.owner
self.balance
```

are instance attributes.

If:

```python
account = BankAccount("Abhijit", 5000)
```

then:

```text
self → account

self.owner
    ↓
account.owner

self.balance
    ↓
account.balance
```

---

# 4.7 Why Not Just Use `name`?

Consider:

```python
class Student:

    def __init__(self, name):
        self.name = name
```

There are two different things here:

```python
name
```

and:

```python
self.name
```

They are not the same.

### `name`

```python
def __init__(self, name):
```

`name` is a **local parameter** of `__init__`.

### `self.name`

```python
self.name = name
```

`self.name` is an **attribute stored on the object**.

So:

```text
name
 ↓
temporary parameter/local name


self.name
 ↓
attribute belonging to the object
```

This distinction is extremely important.

---

# 4.8 `self.attribute = attribute`

You will frequently see this pattern:

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age
```

It may look confusing initially because `name` appears twice.

Break it down:

```python
self.name = name
```

means:

```text
object's name attribute = parameter name
```

Similarly:

```python
self.age = age
```

means:

```text
object's age attribute = parameter age
```

After:

```python
student = Student("Abhijit", 25)
```

the object conceptually contains:

```text
student
│
├── name → "Abhijit"
└── age  → 25
```

---

# 4.9 `self.method()`

`self` is not only used for attributes.

It can also be used to call another method of the **same object**.

Example:

```python
class Student:

    def study(self):
        print("Studying...")

    def start_day(self):
        print("Starting day")
        self.study()
```

Create:

```python
student = Student()
student.start_day()
```

Output:

```text
Starting day
Studying...
```

Here:

```python
self.study()
```

means:

> Call the `study()` method on the current object.

Conceptually:

```text
student.start_day()
        ↓
self → student
        ↓
self.study()
        ↓
student.study()
```

---

# 4.10 Why Use `self.method()`?

Suppose an object has multiple related behaviors:

```python
class Car:

    def start_engine(self):
        print("Engine started")

    def drive(self):
        self.start_engine()
        print("Car is driving")
```

Calling:

```python
car = Car()
car.drive()
```

causes:

```text
car.drive()
    ↓
self → car
    ↓
self.start_engine()
    ↓
car.start_engine()
```

This lets one method use another method belonging to the **same object**.

---

# 4.11 `self.attribute` + `self.method()`

Together, these are the foundation of object behavior.

```python
class BankAccount:

    def __init__(self, balance):
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount

    def show_balance(self):
        print(self.balance)
```

Create:

```python
account = BankAccount(5000)
```

Call:

```python
account.deposit(1000)
account.show_balance()
```

Output:

```text
6000
```

What happened?

### `deposit()`

```python
self.balance += amount
```

means:

```text
account.balance += amount
```

### `show_balance()`

```python
print(self.balance)
```

means:

```text
print(account.balance)
```

So:

```text
self
 ↓
current object
 ↓
 ├── self.balance
 │       ↓
 │   object's data
 │
 └── self.show_balance()
         ↓
     object's behavior
```

---

# 4.12 A Very Important Example

Consider:

```python
class Person:

    def __init__(self, name):
        self.name = name

    def greet(self):
        print(f"Hello, I am {self.name}")

    def introduce(self):
        self.greet()
```

Now:

```python
person = Person("Abhijit")
person.introduce()
```

Flow:

```text
person.introduce()
        ↓
self = person
        ↓
self.greet()
        ↓
person.greet()
        ↓
self.name
        ↓
person.name
        ↓
"Abhijit"
```

Output:

```text
Hello, I am Abhijit
```

This is the mental model you should develop.

---

# 4.13 What Happens Under the Hood?

Consider:

```python
class Student:

    def introduce(self):
        print(self)


student = Student()

student.introduce()
```

The class stores the function:

```text
Student
  │
  └── introduce → function
```

When accessed through an instance:

```python
student.introduce
```

Python creates a **bound method** that remembers the instance.

Conceptually:

```text
student.introduce
       ↓
bound method
       ↓
Student.introduce + student
```

Then:

```python
student.introduce()
```

effectively calls:

```python
Student.introduce(student)
```

So inside:

```python
def introduce(self):
```

we have:

```text
self → student
```

### Important terminology

**Bound method** = a method accessed through an instance where the instance is bound to the method.

You don't need to memorize the implementation details yet, but understanding this explains why Python can automatically supply `self`.

---

# 4.14 `self` Is Not Magic

A common beginner misconception is:

> "`self` is a special Python keyword that automatically means the object."

More accurately:

* `self` is a normal parameter name by convention.
* The instance is passed as the first argument when an instance method is called through an object.
* The name `self` receives that object inside the method.

So:

```python
class Student:

    def greet(self):
        ...
```

is conceptually similar to:

```python
class Student:

    def greet(instance):
        ...
```

The convention is simply to call it `self`.

---

# 4.15 Common Mistakes

### Mistake 1: Forgetting `self`

Wrong:

```python
class Student:

    def greet():
        print("Hello")
```

Then:

```python
student = Student()
student.greet()
```

will raise a `TypeError` because the bound instance is supplied but the method doesn't have a parameter to receive it.

Correct:

```python
def greet(self):
    print("Hello")
```

---

### Mistake 2: Using `self` outside the class method context

`self` isn't a globally available variable.

This is wrong:

```python
print(self.name)
```

outside a method where `self` has been defined as a parameter.

`self` is simply the name used for the current instance inside an instance method.

---

### Mistake 3: Confusing `self.name` and `name`

```python
class Student:

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

### Mistake 4: Forgetting `self` when accessing another method

Inside a class:

```python
class Student:

    def study(self):
        ...

    def start(self):
        study()
```

This is not the normal way to call the instance method.

Use:

```python
self.study()
```

because you want the `study()` method associated with the current instance.

---

# 4.16 `self` vs Object Name

You might wonder:

> Why don't we write `student.name` inside the class instead of `self.name`?

Because the class doesn't know that the object's external variable will be called `student`.

For example:

```python
student1 = Student("Abhijit")
student2 = Student("Rahul")
```

Inside the same method:

```python
def greet(self):
    print(self.name)
```

`self` automatically refers to whichever object called it.

So the method remains reusable:

```text
student1.greet()
     ↓
self → student1


student2.greet()
     ↓
self → student2
```

That's the whole purpose.

---

# 4.17 `self` Is Per Method Call

Suppose:

```python
class Student:

    def greet(self):
        print(self)
```

Then:

```python
student1.greet()
student2.greet()
```

For the first call:

```text
self → student1
```

For the second:

```text
self → student2
```

So `self` isn't permanently tied to one object.

> **`self` refers to the instance associated with the current method call.**

---

# 4.18 Interview Questions

### What is `self` in Python?

> **`self` is the conventional name for the first parameter of an instance method. It refers to the current instance on which the method is called and allows the method to access that object's attributes and other instance methods.**

### Why is `self` required?

> **It allows an instance method to know which object it should operate on. When a method is called through an instance, Python passes that instance as the first argument, which the method conventionally receives as `self`.**

### Is `self` a keyword?

> **No. `self` is not a Python keyword. It is a strong naming convention for the first parameter of an instance method.**

### What is `self.name`?

> **`self.name` refers to the `name` attribute of the current instance.**

### What does `self.method()` mean?

> **It calls another instance method on the current object.**

---

# 4.19 Backend / GenAI Relevance

Understanding `self` is essential for reading real Python libraries and frameworks.

For example:

```python
class LLMClient:

    def __init__(self, model, api_key):
        self.model = model
        self.api_key = api_key

    def generate(self, prompt):
        return self.call_api(prompt)

    def call_api(self, prompt):
        ...
```

Here:

```text
self.model
    ↓
configuration belonging to this client

self.api_key
    ↓
state belonging to this client

self.call_api()
    ↓
another method belonging to this client
```

When you eventually work with:

* FastAPI
* database clients
* API SDKs
* vector stores
* retrievers
* agents
* AI service classes

you will constantly encounter this pattern.

---

# 4.20 Final Mental Model

Memorize this flow:

```text
class Student:

    def introduce(self):
        print(self.name)
```

When:

```python
student.introduce()
```

is called:

```text
student.introduce()
        ↓
Python binds student to the method
        ↓
self → student
        ↓
self.name
        ↓
student.name
```

For another object:

```python
rahul.introduce()
```

the same method works as:

```text
self → rahul
```

### The two most important patterns

```python
self.attribute
```

means:

> **Access data/state of the current object.**

```python
self.method()
```

means:

> **Call behavior on the current object.**

### One-line memory trick

> **`self` = the current instance that this method is operating on.**

---

## Priority

| Concept                                   | Priority           |
| ----------------------------------------- | ------------------ |
| What `self` represents                    | 🔥 Core            |
| Why `self` is required                    | 🔥 Core            |
| `self.attribute`                          | 🔥 Core            |
| `self.method()`                           | 🔥 Core            |
| `self` vs local parameter                 | 🔥 Core            |
| How Python passes the instance            | 🔥 Core            |
| `Student` vs `student` vs `self`          | 🔥 Core            |
| `self` is not a keyword                   | 🟡 Know & Move On  |
| Bound methods                             | 🟡 Know & Move On  |
| Descriptor internals behind bound methods | ⚪ Optional for Now |
