# 12. `super()` 🔥 Core

## 12.1 What is `super()`?

`super()` is a built-in function used to access methods or attributes from a **parent class / next class in the Method Resolution Order (MRO)**.

It is commonly used when a child class wants to:

- call the parent's `__init__()`
- call a parent's overridden method
- extend parent behavior instead of completely replacing it
- write inheritance code that works better with multiple inheritance

Example:

```python
class Parent:

    def show(self):
        print("Parent")


class Child(Parent):

    def show(self):
        super().show()
        print("Child")
```

Now:

```python
child = Child()
child.show()
```

Output:

```text
Parent
Child
```

The child calls the parent implementation using:

```python
super().show()
```

---

# 12.2 Why Do We Need `super()`?

Consider:

```python
class Parent:

    def __init__(self):
        print("Parent initialized")


class Child(Parent):

    def __init__(self):
        print("Child initialized")
```

Create:

```python
child = Child()
```

Output:

```text
Child initialized
```

The parent's `__init__()` does not automatically execute when the child defines its own `__init__()`.

If the child also needs the parent's initialization:

```python
class Child(Parent):

    def __init__(self):
        super().__init__()
        print("Child initialized")
```

Now:

```python
child = Child()
```

Output:

```text
Parent initialized
Child initialized
```

So:

```text
Child.__init__()
      ↓
super().__init__()
      ↓
Parent.__init__()
      ↓
Child continues
```

---

# 12.3 Calling the Parent Constructor

This is one of the most common uses of `super()`.

Example:

```python
class Person:

    def __init__(self, name):
        self.name = name


class Student(Person):

    def __init__(self, name, roll_number):
        super().__init__(name)
        self.roll_number = roll_number
```

Create:

```python
student = Student("Abhijit", 101)
```

Now:

```python
print(student.name)
print(student.roll_number)
```

Output:

```text
Abhijit
101
```

### What happened?

The child receives:

```text
name
roll_number
```

Then:

```python
super().__init__(name)
```

calls the parent initialization.

The parent handles:

```python
self.name = name
```

The child handles:

```python
self.roll_number = roll_number
```

So responsibilities are separated:

```text
Person
└── initializes name

Student
└── initializes roll_number
```

---

# 12.4 Why Not Just Write `Parent.__init__(self, ...)`?

You technically can.

For example:

```python
class Person:

    def __init__(self, name):
        self.name = name


class Student(Person):

    def __init__(self, name, roll_number):
        Person.__init__(self, name)
        self.roll_number = roll_number
```

This can work.

But it has an important limitation:

> You have explicitly hard-coded the parent class.

With:

```python
super().__init__(name)
```

the code is less tightly coupled to a specific parent.

This becomes especially important with **multiple inheritance**.

---

# 12.5 `super()` Is Not Exactly "Parent"

This is a very important technical point.

Beginners often memorize:

> "`super()` calls the parent class."

That's useful initially, but technically incomplete.

A better definition is:

> **`super()` provides access to the next class in the current class's MRO.**

For simple single inheritance:

```text
Child
  ↓
Parent
  ↓
object
```

`super()` from `Child` usually reaches `Parent`.

So it feels like:

```text
super() → parent
```

But with multiple inheritance, the behavior is based on the **MRO**, not simply "go to my parent."

---

# 12.6 Simple MRO Example

```python
class A:
    def show(self):
        print("A")


class B(A):
    def show(self):
        print("B")
        super().show()
```

Create:

```python
b = B()
b.show()
```

Output:

```text
B
A
```

Why?

MRO:

```python
print(B.__mro__)
```

Conceptually:

```text
B → A → object
```

Inside `B.show()`:

```python
super().show()
```

finds the next `show()` according to the MRO:

```text
B
 ↓
A
 ↓
show() found
```

---

# 12.7 Calling Parent Methods

`super()` isn't limited to `__init__()`.

Suppose:

```python
class Animal:

    def sound(self):
        print("Animal sound")


class Dog(Animal):

    def sound(self):
        super().sound()
        print("Dog barks")
```

Now:

```python
dog = Dog()
dog.sound()
```

Output:

```text
Animal sound
Dog barks
```

The child method extends the parent's behavior.

---

# 12.8 Replace vs Extend

This gives us two common overriding patterns.

### Replace parent behavior

```python
class Dog(Animal):

    def sound(self):
        print("Dog barks")
```

The child provides its own behavior.

```text
Parent behavior
      ↓
not called
```

### Extend parent behavior

```python
class Dog(Animal):

    def sound(self):
        super().sound()
        print("Dog barks")
```

Now:

```text
Parent behavior
      ↓
Child behavior
```

### Mental model

```text
Override without super()
→ Replace / completely customize

Override with super()
→ Extend / reuse + customize
```

This isn't an absolute rule—`super()` can be used in other patterns too—but it's a very useful mental model.

---

# 12.9 Passing Arguments Through `super()`

Suppose:

```python
class Person:

    def __init__(self, name, age):
        self.name = name
        self.age = age


class Student(Person):

    def __init__(self, name, age, roll_number):
        super().__init__(name, age)
        self.roll_number = roll_number
```

Here:

```python
super().__init__(name, age)
```

passes the required arguments to the next implementation.

Now:

```python
student = Student("Abhijit", 25, 101)
```

creates:

```text
student
├── name → Abhijit
├── age → 25
└── roll_number → 101
```

---

# 12.10 `super()` With Methods Other Than `__init__()`

Example:

```python
class Employee:

    def introduce(self):
        print("I am an employee")


class Developer(Employee):

    def introduce(self):
        super().introduce()
        print("I am a developer")
```

Now:

```python
developer = Developer()
developer.introduce()
```

Output:

```text
I am an employee
I am a developer
```

So remember:

> `super()` is not specifically for constructors.

It can be used to access another implementation in the MRO.

---

# 12.11 `super()` Without Arguments

Inside a normal method, you will commonly see:

```python
super()
```

instead of:

```python
super(Child, self)
```

Example:

```python
class Child(Parent):

    def __init__(self):
        super().__init__()
```

Modern Python automatically determines the relevant context for zero-argument `super()` in normal class methods.

This is the preferred modern style.

---

# 12.12 The Older Explicit Form

You may sometimes encounter:

```python
super(Child, self).__init__()
```

This explicitly specifies:

```text
Child → current class
self  → current instance
```

For example:

```python
class Student(Person):

    def __init__(self, name):
        super(Student, self).__init__(name)
```

In modern Python 3 code, prefer:

```python
super().__init__(name)
```

It is shorter and easier to maintain.

---

# 12.13 Why `super()` Is Preferred

There are several reasons.

## 1. Less hard-coding

Instead of:

```python
Person.__init__(self, name)
```

use:

```python
super().__init__(name)
```

The child doesn't explicitly name its parent.

---

## 2. Better support for multiple inheritance

This is the **most important advanced reason**.

Suppose:

```python
class A:
    def show(self):
        print("A")


class B(A):
    def show(self):
        print("B")
        super().show()


class C(A):
    def show(self):
        print("C")
        super().show()


class D(B, C):
    def show(self):
        print("D")
        super().show()
```

The MRO of `D` is approximately:

```text
D → B → C → A → object
```

So:

```python
d = D()
d.show()
```

produces:

```text
D
B
C
A
```

Each class uses:

```python
super().show()
```

to cooperate with the next class in the MRO.

This is called **cooperative multiple inheritance**.

We'll study this much more deeply when we reach MRO.

---

# 12.14 Why Explicit Parent Calls Can Cause Problems

Consider:

```python
class A:

    def show(self):
        print("A")


class B(A):

    def show(self):
        A.show(self)
        print("B")
```

This explicitly says:

```python
A.show(self)
```

Now imagine the class hierarchy changes or multiple inheritance is introduced.

The code is still specifically tied to `A`.

With:

```python
super().show()
```

the lookup follows the MRO.

This makes the code more cooperative with Python's inheritance mechanism.

---

# 12.15 `super()` and Multiple Inheritance

This is where `super()` becomes especially powerful.

Consider:

```python
class A:

    def show(self):
        print("A")


class B(A):

    def show(self):
        print("B")
        super().show()


class C(A):

    def show(self):
        print("C")
        super().show()


class D(B, C):

    def show(self):
        print("D")
        super().show()
```

Check MRO:

```python
print(D.__mro__)
```

Conceptually:

```text
D
↓
B
↓
C
↓
A
↓
object
```

Calling:

```python
D().show()
```

causes:

```text
D.show()
  ↓
super()
  ↓
B.show()
  ↓
super()
  ↓
C.show()
  ↓
super()
  ↓
A.show()
```

Output:

```text
D
B
C
A
```

This demonstrates why the statement:

> "`super()` means parent"

is incomplete.

In this example, inside `B`, `super()` leads to `C`, not directly to `A`.

---

# 12.16 `super()` and the Diamond Problem

Multiple inheritance can produce a diamond:

```text
       A
      / \
     B   C
      \ /
       D
```

Here:

```python
class B(A):
    ...


class C(A):
    ...


class D(B, C):
    ...
```

Without a proper method-resolution system, `A` could potentially be called multiple times.

Python's MRO and cooperative use of `super()` help manage this inheritance structure.

The exact rules are based on **C3 linearization**, which determines a consistent MRO.

You do not need to memorize C3 linearization yet.

For now:

> **MRO + `super()` allow multiple-inheritance classes to cooperate without manually hard-coding every parent call.**

---

# 12.17 Important: `super()` Returns a Proxy

Technically:

```python
super()
```

does not directly return the parent class.

It returns a **super object** that provides access to attributes/methods found after the current class in the MRO.

For example:

```python
super().show()
```

means roughly:

```text
Create super context
      ↓
Look after current class in MRO
      ↓
Find show()
      ↓
Call it
```

You don't need to work with the `super` object directly at your current level, but knowing this prevents the misconception that:

```text
super() = parent class
```

---

# 12.18 `super()` With Class Methods

`super()` can also be used with class methods.

Example:

```python
class Parent:

    @classmethod
    def show(cls):
        print("Parent")


class Child(Parent):

    @classmethod
    def show(cls):
        super().show()
        print("Child")
```

Now:

```python
Child.show()
```

Output:

```text
Parent
Child
```

The exact binding behavior follows the class/MRO context.

The key point is:

> `super()` is about accessing the next implementation in the MRO, not specifically about instance methods.

---

# 12.19 Common Mistakes

### Mistake 1: Thinking `super()` automatically calls `__init__()`

It doesn't.

You explicitly call:

```python
super().__init__()
```

if you want the next implementation of `__init__()` to execute.

---

### Mistake 2: Thinking `super()` always means direct parent

Simplified:

```text
super() ≈ parent
```

Technically:

```text
super() → next implementation according to MRO
```

The second explanation is the correct one.

---

### Mistake 3: Forgetting to pass required arguments

If:

```python
class Parent:

    def __init__(self, name):
        self.name = name
```

then:

```python
super().__init__()
```

will fail because `name` is required.

Use:

```python
super().__init__(name)
```

---

### Mistake 4: Calling the parent manually everywhere

This works:

```python
Parent.method(self)
```

but can tightly couple the child to a specific parent.

Prefer:

```python
super().method()
```

when cooperative inheritance is intended.

---

### Mistake 5: Thinking `super()` is only for constructors

It can be used with:

```text
__init__()
ordinary methods
class methods
other inherited attributes
```

when appropriate.

---

# 12.20 Interview Questions

### What is `super()`?

> **`super()` provides access to methods and attributes from the next class in the Method Resolution Order. It is commonly used to call inherited implementations from a child class.**

### Why is `super()` used in `__init__()`?

> **It allows a child class to invoke the parent or next MRO implementation's initialization so that inherited state can also be initialized.**

### Can `super()` call methods other than `__init__()`?

> **Yes. It can be used to access other inherited method implementations as well.**

### Why is `super()` preferred over `Parent.method(self)`?

> **It avoids hard-coding a specific parent and works with Python's MRO, making inheritance code more maintainable and suitable for cooperative multiple inheritance.**

### Does `super()` always mean the parent class?

> **Not technically. It accesses the next appropriate class in the MRO. In simple single inheritance, that is usually the parent class.**

### What is cooperative multiple inheritance?

> **It is a design where classes in a multiple-inheritance hierarchy use `super()` to delegate to the next class in the MRO, allowing the classes to work together.**

---

# 12.21 Complete Example

```python
class Person:

    def __init__(self, name):
        self.name = name

    def introduce(self):
        print(f"My name is {self.name}")


class Student(Person):

    def __init__(self, name, roll_number):
        super().__init__(name)
        self.roll_number = roll_number

    def introduce(self):
        super().introduce()
        print(f"My roll number is {self.roll_number}")
```

Create:

```python
student = Student("Abhijit", 101)
```

When the object is created:

```text
Student.__init__()
       ↓
super().__init__(name)
       ↓
Person.__init__(name)
       ↓
self.name = "Abhijit"
       ↓
Student continues
       ↓
self.roll_number = 101
```

When:

```python
student.introduce()
```

runs:

```text
Student.introduce()
       ↓
super().introduce()
       ↓
Person.introduce()
       ↓
Student continues
```

Output:

```text
My name is Abhijit
My roll number is 101
```

This single example demonstrates both major uses:

```python
super().__init__(name)
```

and:

```python
super().introduce()
```

---

# 12.22 Backend / GenAI Relevance

`super()` becomes useful when you create specialized classes that extend common infrastructure.

Example:

```python
class BaseClient:

    def __init__(self, timeout):
        self.timeout = timeout


class LLMClient(BaseClient):

    def __init__(self, timeout, model):
        super().__init__(timeout)
        self.model = model
```

The base class manages:

```text
timeout
```

while the specialized class manages:

```text
model
```

This keeps initialization responsibilities separated.

A more realistic hierarchy might contain:

```text
BaseClient
    ↓
LLMClient
    ↓
OpenAIClient
```

and each layer can initialize or extend behavior without duplicating the parent's code.

In larger Python applications, though, don't create deep inheritance hierarchies unnecessarily. Composition and dependency injection are often preferable when the relationship is not genuinely "is-a."

---

# 12.23 Final Mental Model

Remember:

```text
                 Parent
                    ↑
                    │
                 Child
                    │
              super()
                    │
                    ↓
       next implementation in MRO
```

For simple inheritance:

```text
Child
  ↓
Parent
```

so:

```python
super().method()
```

usually reaches:

```python
Parent.method()
```

For multiple inheritance:

```text
D → B → C → A → object
```

`super()` follows the MRO:

```text
D.super()
   ↓
B

B.super()
   ↓
C

C.super()
   ↓
A
```

### The core mental model

```text
super()
   ↓
"Continue the method lookup from here."
```

Not merely:

```text
super()
   ↓
"Call my parent."
```

---

## One-Line Interview Memory

> **`super()` lets a class access the next implementation in the MRO, commonly allowing a child to reuse or extend parent initialization and methods without hard-coding a specific parent class.**

---

## Priority

| Concept | Priority |
|---|---|
| What `super()` does | 🔥🔥 Core |
| `super().__init__()` | 🔥🔥 Core |
| Calling parent methods with `super()` | 🔥🔥 Core |
| Extending vs replacing parent behavior | 🔥 Core |
| Why `super()` is preferred | 🔥🔥 Core |
| `super()` and MRO | 🔥🔥 Core |
| `super()` in multiple inheritance | 🔥 Core |
| Cooperative multiple inheritance | 🟡 Know & Move On |
| `super()` as a proxy object | 🟡 Know & Move On |
| C3 linearization internals | ⚪ Optional for Now |