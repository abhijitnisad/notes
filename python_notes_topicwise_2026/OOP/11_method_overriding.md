# 11. Method Overriding 🔥 Core

## 11.1 What is Method Overriding?

**Method overriding** occurs when a child class defines a method with the **same name** as a method already defined in its parent class.

The child provides its own implementation of that behavior.

Example:

```python
class Animal:

    def sound(self):
        print("Some generic sound")


class Dog(Animal):

    def sound(self):
        print("Bark")
```

Here both classes have:

```python
sound()
```

But `Dog` provides its own implementation.

Now:

```python
dog = Dog()

dog.sound()
```

Output:

```text
Bark
```

The child's `sound()` overrides the parent's `sound()` for `Dog` objects.

### Mental model

```text
Parent
└── sound() → "Some generic sound"

        ↓ overridden by

Child
└── sound() → "Bark"
```

---

# 11.2 Why Is It Called "Overriding"?

The parent already defines:

```python
sound()
```

The child defines another:

```python
sound()
```

with the same method name.

When Python looks up:

```python
dog.sound()
```

it finds the implementation associated with `Dog` before reaching `Animal`.

So the child's implementation **overrides** the inherited behavior.

---

# 11.3 Basic Example

```python
class Animal:

    def sound(self):
        print("Animal makes a sound")


class Dog(Animal):

    def sound(self):
        print("Dog barks")
```

Create objects:

```python
animal = Animal()
dog = Dog()
```

Now:

```python
animal.sound()
```

Output:

```text
Animal makes a sound
```

But:

```python
dog.sound()
```

Output:

```text
Dog barks
```

Why?

```text
animal
  ↓
Animal.sound()

dog
  ↓
Dog.sound()
  ↓
found immediately
```

The parent implementation still exists. It is simply not the one selected for normal `dog.sound()` lookup.

---

# 11.4 Overriding Does Not Delete the Parent Method

This is an important point.

Suppose:

```python
class Parent:

    def show(self):
        print("Parent")


class Child(Parent):

    def show(self):
        print("Child")
```

The parent method still exists:

```python
Parent.show
```

The child has its own:

```python
Child.show
```

Conceptually:

```text
Parent
└── show() → "Parent"

Child
└── show() → "Child"
```

So overriding means:

> **The child provides a different implementation for the same inherited behavior.**

It does not physically remove the parent method.

---

# 11.5 How Python Finds the Overridden Method

This connects directly to the attribute lookup and inheritance concepts you've already learned.

Consider:

```python
class Animal:

    def sound(self):
        print("Animal")


class Dog(Animal):

    def sound(self):
        print("Dog")
```

When:

```python
dog.sound()
```

is executed, Python looks through the object's class hierarchy.

Simplified:

```text
dog
 ↓
Dog
 ↓
sound found
 ↓
Dog.sound()
```

Python doesn't need to continue to:

```text
Animal
```

because it already found `sound()` in `Dog`.

### Simplified rule

> **During method lookup, the child class is searched before its parent according to the inheritance hierarchy/MRO.**

---

# 11.6 Overriding and `self`

The overriding method normally has the same instance-method structure:

```python
class Parent:

    def show(self):
        print("Parent")


class Child(Parent):

    def show(self):
        print("Child")
```

Both receive:

```python
self
```

because both are instance methods.

The important difference is the implementation.

```text
Parent.show()
    ↓
Parent behavior

Child.show()
    ↓
Child behavior
```

---

# 11.7 Method Overriding vs Method Overloading

These are different concepts.

### Overriding

Child replaces/redefines inherited behavior:

```python
class Parent:
    def show(self):
        print("Parent")


class Child(Parent):
    def show(self):
        print("Child")
```

### Overloading

Traditionally means defining multiple methods with the same name but different parameter lists.

Python does **not** support traditional method overloading in the same way as languages such as Java.

For example, this does not create two overloads:

```python
class Calculator:

    def add(self, a):
        pass

    def add(self, a, b):
        pass
```

The second `add()` replaces the first definition in the class namespace.

For now, remember:

```text
Overriding
→ inheritance relationship
→ child redefines parent method

Traditional overloading
→ multiple signatures
→ not directly supported like Java/C++
```

---

# 11.8 Runtime Polymorphism 🔥

Method overriding is closely connected to **runtime polymorphism**.

Let's break the term down.

### Poly

Means:

> many

### Morph

Means:

> forms

So polymorphism means:

> **The same interface/method call can produce different behavior depending on the object involved.**

Example:

```python
class Animal:

    def sound(self):
        print("Some sound")


class Dog(Animal):

    def sound(self):
        print("Bark")


class Cat(Animal):

    def sound(self):
        print("Meow")
```

Now:

```python
dog = Dog()
cat = Cat()

dog.sound()
cat.sound()
```

Output:

```text
Bark
Meow
```

The method call is the same:

```python
object.sound()
```

but the behavior depends on the actual object.

---

# 11.9 Why Is This "Runtime" Polymorphism?

The important part is **runtime**.

Consider:

```python
def make_sound(animal):
    animal.sound()
```

Now:

```python
dog = Dog()
cat = Cat()

make_sound(dog)
make_sound(cat)
```

The function doesn't need to know:

```text
"If dog → call Dog.sound()
 If cat → call Cat.sound()"
```

It simply calls:

```python
animal.sound()
```

The actual object's implementation is selected at runtime.

Conceptually:

```text
make_sound(dog)
      ↓
animal = Dog object
      ↓
animal.sound()
      ↓
Dog.sound()

make_sound(cat)
      ↓
animal = Cat object
      ↓
animal.sound()
      ↓
Cat.sound()
```

That's the core idea behind runtime polymorphism.

---

# 11.10 The Same Method Call, Different Behavior

This is the easiest way to understand polymorphism:

```python
dog.sound()
cat.sound()
```

Both use:

```python
.sound()
```

but:

```text
Dog → Bark
Cat → Meow
```

So:

```text
Same interface
      ↓
Different implementation
      ↓
Different behavior
```

---

# 11.11 A Better Example

```python
class Payment:

    def pay(self):
        print("Processing payment")


class UPI(Payment):

    def pay(self):
        print("Processing UPI payment")


class Card(Payment):

    def pay(self):
        print("Processing card payment")


class Cash(Payment):

    def pay(self):
        print("Processing cash payment")
```

Now:

```python
def process_payment(payment):
    payment.pay()
```

Call:

```python
process_payment(UPI())
process_payment(Card())
process_payment(Cash())
```

Output:

```text
Processing UPI payment
Processing card payment
Processing cash payment
```

The function:

```python
process_payment()
```

doesn't need separate logic for every payment type.

It simply expects an object that provides:

```python
pay()
```

This is polymorphism in action.

---

# 11.12 Important: Python Is Dynamically Typed

Python doesn't require:

```python
def process_payment(payment: Payment):
```

for this to work.

Even this can work:

```python
class Bitcoin:

    def pay(self):
        print("Processing Bitcoin payment")
```

Then:

```python
process_payment(Bitcoin())
```

works because the object provides the required behavior.

This leads to another important Python concept:

> **Duck typing**

We'll study duck typing later.

So Python can achieve polymorphic behavior through both:

- inheritance
- compatible behavior/interfaces

---

# 11.13 Method Overriding With Different Arguments

For a clean override, the child method should generally preserve a compatible calling interface.

Example:

```python
class Animal:

    def sound(self):
        print("Animal sound")


class Dog(Animal):

    def sound(self):
        print("Bark")
```

This is straightforward.

Be careful with:

```python
class Animal:

    def sound(self, volume):
        print("Animal sound")


class Dog(Animal):

    def sound(self):
        print("Bark")
```

Now the child has changed the expected arguments.

This can cause problems if code expects every `Animal` to support:

```python
animal.sound(volume)
```

A good overriding design should preserve the behavioral expectations of the parent interface.

This idea becomes important when we study **polymorphism and the Liskov Substitution Principle** later.

---

# 11.14 Calling the Parent Implementation

Sometimes the child wants to add behavior rather than completely replace the parent behavior.

Example:

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

Here:

```python
super().sound()
```

calls the parent implementation according to Python's method-resolution mechanism.

### Mental model

```text
Dog.sound()
    ↓
super().sound()
    ↓
parent implementation
    ↓
continue child implementation
```

`super()` is an important topic of its own, so we'll cover it separately.

---

# 11.15 Complete Example: Parent + Child + Polymorphism

```python
class Animal:

    def sound(self):
        print("Some animal sound")


class Dog(Animal):

    def sound(self):
        print("Bark")


class Cat(Animal):

    def sound(self):
        print("Meow")


def make_sound(animal):
    animal.sound()
```

Now:

```python
make_sound(Dog())
make_sound(Cat())
make_sound(Animal())
```

Output:

```text
Bark
Meow
Some animal sound
```

Notice that `make_sound()` has only one line of important logic:

```python
animal.sound()
```

It doesn't need to check the object's class manually.

That's one of the major benefits of polymorphism.

---

# 11.16 Without Polymorphism

You could write:

```python
def make_sound(animal):

    if isinstance(animal, Dog):
        print("Bark")

    elif isinstance(animal, Cat):
        print("Meow")

    elif isinstance(animal, Animal):
        print("Some sound")
```

This approach becomes difficult to maintain as more animal types are added.

With polymorphism:

```python
def make_sound(animal):
    animal.sound()
```

Each class owns its own behavior.

### Design idea

Instead of:

```text
Central function decides behavior
```

we can use:

```text
Each object provides its own behavior
```

This is a major OOP design principle.

---

# 11.17 Overriding vs Shadowing

These two concepts are easy to confuse.

### Attribute shadowing

You learned earlier:

```python
class Student:
    college = "TIT"


student = Student()

student.college = "RGPV"
```

Here the **instance attribute** shadows the class attribute.

### Method overriding

With inheritance:

```python
class Animal:
    def sound(self):
        print("Animal")


class Dog(Animal):
    def sound(self):
        print("Dog")
```

Here the **child class method** overrides the parent method.

So:

```text
Shadowing
→ usually instance attribute hides class attribute

Overriding
→ child class redefines inherited method
```

---

# 11.18 Method Overriding vs Method Reuse

Without overriding:

```python
class Dog(Animal):
    pass
```

`Dog` simply inherits the parent's implementation.

```text
Dog
 ↓
Animal.sound()
```

With overriding:

```python
class Dog(Animal):

    def sound(self):
        print("Bark")
```

Now:

```text
Dog
 ↓
Dog.sound()
```

The parent's version is still available through the parent class and can potentially be reached explicitly using mechanisms such as `super()`.

---

# 11.19 Real-World Backend / GenAI Example

Suppose you have different LLM providers.

```python
class LLMProvider:

    def generate(self, prompt):
        print("Generating response")
```

Different providers can implement their own behavior:

```python
class OpenAIProvider(LLMProvider):

    def generate(self, prompt):
        print("Generating using OpenAI")


class GeminiProvider(LLMProvider):

    def generate(self, prompt):
        print("Generating using Gemini")
```

Now:

```python
def generate_response(provider, prompt):
    provider.generate(prompt)
```

You can pass:

```python
generate_response(
    OpenAIProvider(),
    "Explain Python"
)

generate_response(
    GeminiProvider(),
    "Explain Python"
)
```

The caller doesn't need to know the internal implementation of each provider.

It only needs the common operation:

```python
generate()
```

This is the kind of abstraction that becomes useful when building systems that can work with multiple implementations.

---

# 11.20 Common Mistakes

### Mistake 1: Thinking overriding deletes the parent method

It doesn't.

Both methods still exist:

```text
Parent.sound()
Child.sound()
```

The child's method is selected for child objects through normal lookup.

---

### Mistake 2: Thinking overriding requires a special decorator

Python does **not** require an `@override` decorator to override a method.

Simply defining the same method name in the child class is enough.

```python
class Child(Parent):

    def show(self):
        print("Child")
```

Some projects may use optional tooling/decorators to explicitly mark overrides, but Python's basic overriding mechanism does not require one.

---

### Mistake 3: Confusing overriding with overloading

```text
Overriding
→ parent-child relationship

Overloading
→ same method name with different signatures
```

Python does not provide traditional method overloading like Java.

---

### Mistake 4: Manually checking every subclass

Avoid unnecessary code like:

```python
if isinstance(obj, Dog):
    ...
elif isinstance(obj, Cat):
    ...
```

when each class can implement the same method interface.

Polymorphism often lets the object determine its own behavior.

---

# 11.21 Interview Questions

### What is method overriding?

> **Method overriding occurs when a child class provides its own implementation of a method that is already defined in its parent class.**

### Why is method overriding used?

> **It allows a child class to specialize or replace inherited behavior while keeping the same method interface.**

### What is runtime polymorphism?

> **Runtime polymorphism is the ability to use the same method call/interface with different objects and have the appropriate implementation execute based on the actual object at runtime.**

### How is overriding related to polymorphism?

> **Overriding allows different child classes to provide different implementations of the same parent method, which enables runtime polymorphic behavior.**

### Does Python require an override keyword?

> **No. Python does not require a special keyword to override a method. Defining a method with the same name in the child class is sufficient.**

### Can a child still call the parent's overridden method?

> **Yes. The parent implementation can be explicitly accessed, commonly using `super()` when appropriate.**

---

# 11.22 Final Mental Model

Think of inheritance + overriding like this:

```text
                 Animal
                    │
                 sound()
              "Some sound"
                    │
          ┌─────────┴─────────┐
          ↓                   ↓
         Dog                 Cat
       sound()              sound()
        "Bark"               "Meow"
```

Then:

```python
def make_sound(animal):
    animal.sound()
```

The caller doesn't need to know whether `animal` is:

```text
Dog
Cat
Animal
```

It simply says:

```python
animal.sound()
```

and the appropriate implementation runs.

### The core relationship

```text
Inheritance
    ↓
Child gets parent's behavior
    ↓
Overriding
    ↓
Child changes that behavior
    ↓
Polymorphism
    ↓
Same method call → different behavior
```

---

## One-Line Interview Memory

> **Method overriding is when a child class redefines a parent method, allowing the same method call to produce behavior specific to the actual object at runtime.**

---

## Priority

| Concept | Priority |
|---|---|
| What is method overriding | 🔥 Core |
| Child replacing parent behavior | 🔥 Core |
| Overriding with inheritance | 🔥 Core |
| Method lookup in child before parent | 🔥 Core |
| Runtime polymorphism | 🔥🔥 Core |
| Same interface, different behavior | 🔥 Core |
| `super()` preview | 🟡 Know & Move On |
| Overriding vs overloading | 🟡 Know & Move On |
| Duck typing | 🟡 Know & Move On — later |
| MRO internals | 🟡 Know & Move On — later |
| Liskov Substitution Principle | ⚪ Optional for Now |