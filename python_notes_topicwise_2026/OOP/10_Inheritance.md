# 10. Inheritance 🔥🔥 Core

## 10.1 What is Inheritance?

**Inheritance** is a mechanism where a new class can **reuse and extend** the attributes and methods of an existing class.

The existing class is called the:

- **Parent class**
- **Base class**
- **Super class**

The new class is called the:

- **Child class**
- **Derived class**
- **Sub class**

Example:

```python
class Animal:
    def eat(self):
        print("Animal is eating")


class Dog(Animal):
    def bark(self):
        print("Dog is barking")
```

Here:

```text
Animal
  ↑
  │ inherits from
  │
Dog
```

`Dog` can use the inherited `eat()` method:

```python
dog = Dog()

dog.eat()
dog.bark()
```

Output:

```text
Animal is eating
Dog is barking
```

The important idea is:

> **A child class can reuse behavior from its parent class and add its own behavior.**

---

# 10.2 Basic Syntax

Inheritance is specified inside the class definition:

```python
class Child(Parent):
    pass
```

Example:

```python
class Animal:
    pass


class Dog(Animal):
    pass
```

The part:

```python
Dog(Animal)
```

means:

> `Dog` inherits from `Animal`.

---

# 10.3 Parent / Base Class

The class being inherited from is called the **parent** or **base class**.

```python
class Animal:

    def eat(self):
        print("Eating")
```

Here:

```text
Animal → Parent / Base class
```

It contains behavior that can be reused by child classes.

---

# 10.4 Child / Derived Class

The class that inherits from another class is called the **child** or **derived class**.

```python
class Dog(Animal):

    def bark(self):
        print("Barking")
```

Here:

```text
Dog → Child / Derived class
```

`Dog` receives access to inherited behavior from `Animal`.

---

# 10.5 What Does a Child Class Actually Get?

Consider:

```python
class Animal:

    def eat(self):
        print("Eating")


class Dog(Animal):

    def bark(self):
        print("Barking")
```

`Dog` does not need to redefine `eat()`.

So:

```python
dog = Dog()

dog.eat()
```

works.

Conceptually:

```text
Dog object
    │
    ↓
Dog class
    │
    │ eat() not found here
    ↓
Animal class
    │
    │ eat() found
    ↓
execute Animal.eat()
```

This is an important connection to the **attribute lookup** you studied earlier.

### Mental model

> **Inheritance allows a child class to participate in the parent's attribute/method lookup path.**

The complete lookup mechanism is more detailed and involves the **MRO**, which we'll study later.

---

# 10.6 Why Use Inheritance?

Inheritance is mainly useful for:

### 1. Code reuse

Common behavior can be written once in the parent.

```python
class Animal:

    def eat(self):
        print("Eating")
```

Multiple children can reuse it:

```python
class Dog(Animal):
    pass


class Cat(Animal):
    pass
```

Both can use:

```python
dog.eat()
cat.eat()
```

---

### 2. Extending existing behavior

The child can add new methods.

```python
class Dog(Animal):

    def bark(self):
        print("Barking")
```

Now `Dog` has:

```text
Inherited:
    eat()

Own:
    bark()
```

---

### 3. Specializing behavior

A general parent can represent common behavior, while children represent specialized versions.

```text
Animal
├── Dog
├── Cat
└── Cow
```

The parent contains common functionality.

Children can add or customize behavior.

---

### 4. Supporting polymorphism

Inheritance often provides the structure for **method overriding and polymorphism**, which we'll study later.

For example:

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

All three objects have a `sound()` method, but the behavior differs.

---

# 10.7 Important: Inheritance Is an "is-a" Relationship

Inheritance is generally appropriate when the child **is a type of** the parent.

For example:

```text
Dog is an Animal
Cat is an Animal
Car is a Vehicle
Student is a Person
```

This gives:

```text
Dog → Animal
```

because a dog is an animal.

But:

```text
Car → Engine
```

usually doesn't make sense as inheritance because:

> A car **has an** engine.

That's a **composition** relationship, not an inheritance relationship.

We'll study composition later.

### Memory rule

```text
"is-a"  → inheritance
"has-a" → composition
```

---

# 10.8 Single Inheritance 🔥

**Single inheritance** means one child class inherits from one parent class.

```text
Parent
   ↑
Child
```

Example:

```python
class Animal:

    def eat(self):
        print("Eating")


class Dog(Animal):

    def bark(self):
        print("Barking")
```

Here:

```text
Animal
  ↑
 Dog
```

`Dog` has access to:

```python
dog.eat()
dog.bark()
```

---

# 10.9 Single Inheritance Example

```python
class Vehicle:

    def start(self):
        print("Vehicle started")


class Car(Vehicle):

    def drive(self):
        print("Car is driving")
```

Now:

```python
car = Car()

car.start()
car.drive()
```

Output:

```text
Vehicle started
Car is driving
```

The child class has:

```text
Car
├── start() → inherited
└── drive() → its own method
```

---

# 10.10 Multilevel Inheritance 🔥

**Multilevel inheritance** occurs when inheritance forms a chain.

```text
Grandparent
     ↑
   Parent
     ↑
    Child
```

Example:

```python
class Animal:

    def eat(self):
        print("Eating")


class Dog(Animal):

    def bark(self):
        print("Barking")


class Puppy(Dog):

    def play(self):
        print("Playing")
```

Now:

```python
puppy = Puppy()
```

`Puppy` can access:

```python
puppy.eat()
puppy.bark()
puppy.play()
```

Why?

```text
Puppy
  ↓
Dog
  ↓
Animal
```

Python can search through this inheritance chain.

---

# 10.11 Multilevel Mental Model

```text
Animal
│
├── eat()
│
↓
Dog
│
├── bark()
│
↓
Puppy
│
└── play()
```

So a `Puppy` object can access:

```text
Puppy methods
      ↓
Dog methods
      ↓
Animal methods
```

### Important

The child doesn't literally copy all parent methods into its own namespace.

Instead, Python's attribute lookup can search through the inheritance hierarchy.

This is why understanding namespaces and attribute lookup from previous topics matters here.

---

# 10.12 Multiple Inheritance 🔥

**Multiple inheritance** means one child class inherits from **more than one parent class**.

Syntax:

```python
class Child(Parent1, Parent2):
    pass
```

Example:

```python
class Father:

    def skills(self):
        print("Driving")


class Mother:

    def hobbies(self):
        print("Painting")


class Child(Father, Mother):
    pass
```

Now:

```python
child = Child()

child.skills()
child.hobbies()
```

Output:

```text
Driving
Painting
```

The `Child` class inherits from both:

```text
Father
   ↘
     Child
   ↗
Mother
```

---

# 10.13 Why Multiple Inheritance Can Become Complicated

Suppose both parents have a method with the same name:

```python
class Father:

    def show(self):
        print("Father")


class Mother:

    def show(self):
        print("Mother")


class Child(Father, Mother):
    pass
```

Now:

```python
child = Child()

child.show()
```

Which `show()` should Python use?

Python needs a defined order for searching the inheritance hierarchy.

This is handled by the:

> **Method Resolution Order (MRO)**

For this example:

```python
print(Child.__mro__)
```

will show an order beginning conceptually like:

```text
Child → Father → Mother → object
```

Because the class was declared as:

```python
class Child(Father, Mother):
```

MRO is a major topic we'll study separately.

---

# 10.14 MRO Preview

For now, remember only the basic idea:

> **MRO determines the order in which Python searches classes for an attribute or method.**

Example:

```python
class A:
    x = 10


class B(A):
    pass
```

When:

```python
b = B()

print(b.x)
```

Python can search:

```text
B
 ↓
A
 ↓
object
```

For multiple inheritance, the order becomes more important:

```text
Child
  ↓
Parent 1
  ↓
Parent 2
  ↓
object
```

The actual MRO rules are more sophisticated than simply "left to right," especially with diamond inheritance.

We'll cover that later.

---

# 10.15 `object` — The Ultimate Base Class

There is one more important concept.

In Python 3, normal classes ultimately inherit from:

```python
object
```

For example:

```python
class Animal:
    pass
```

is conceptually part of an inheritance hierarchy ending in:

```text
Animal
   ↓
object
```

You can see it:

```python
print(Animal.__mro__)
```

Typical result:

```text
(<class '__main__.Animal'>, <class 'object'>)
```

For:

```python
class Dog(Animal):
    pass
```

the hierarchy becomes:

```text
Dog
 ↓
Animal
 ↓
object
```

This is why every normal Python class participates in an inheritance hierarchy.

---

# 10.16 Inherited Attributes vs Own Attributes

Consider:

```python
class Animal:

    species = "Animal"


class Dog(Animal):

    breed = "Labrador"
```

Create:

```python
dog = Dog()
```

Conceptually:

```text
Dog class
└── breed → Labrador

Animal class
└── species → Animal
```

Now:

```python
dog.breed
```

finds:

```text
Dog
```

while:

```python
dog.species
```

can find:

```text
Animal
```

through inheritance.

Again:

> The child can access attributes from the parent, but those attributes are not necessarily stored in the child's namespace.

---

# 10.17 Inheritance + `__init__()`

A common beginner question is:

> "If a child inherits from a parent, does the parent's `__init__()` automatically run?"

Not always.

Example:

```python
class Parent:

    def __init__(self):
        print("Parent initialized")


class Child(Parent):

    def __init__(self):
        print("Child initialized")
```

Now:

```python
child = Child()
```

Output:

```text
Child initialized
```

The child's `__init__()` overrides the inherited one.

The parent initialization does not automatically execute just because inheritance exists.

Later, we use:

```python
super()
```

to explicitly cooperate with the parent implementation.

For example:

```python
class Child(Parent):

    def __init__(self):
        super().__init__()
        print("Child initialized")
```

`super()` is a separate important topic.

---

# 10.18 Inheritance Does Not Mean Everything Is Copied

This is an important mental model.

Suppose:

```python
class Animal:

    def eat(self):
        print("Eating")


class Dog(Animal):
    pass
```

It is tempting to imagine:

```text
Dog
├── eat()   ← copied from Animal
```

But a better mental model is:

```text
Dog
  ↓
attribute lookup
  ↓
Animal
  ↓
eat() found
```

The method remains defined in the parent class.

This is why inheritance is closely connected to **attribute lookup and MRO**.

---

# 10.19 `isinstance()` and Inheritance

Inheritance also affects `isinstance()`.

```python
class Animal:
    pass


class Dog(Animal):
    pass


dog = Dog()
```

Now:

```python
isinstance(dog, Dog)
```

returns:

```text
True
```

And:

```python
isinstance(dog, Animal)
```

also returns:

```text
True
```

because a `Dog` is an `Animal` through inheritance.

Conceptually:

```text
Dog object
    ↓
Dog
    ↓
Animal
```

This is useful when checking whether an object belongs to a class or its inheritance hierarchy.

---

# 10.20 `issubclass()`

Python also provides:

```python
issubclass()
```

which checks relationships between classes.

Example:

```python
class Animal:
    pass


class Dog(Animal):
    pass
```

Then:

```python
issubclass(Dog, Animal)
```

returns:

```text
True
```

But:

```python
issubclass(Animal, Dog)
```

returns:

```text
False
```

### Remember

```text
isinstance()
    ↓
object relationship

issubclass()
    ↓
class relationship
```

---

# 10.21 Types of Inheritance

At your current level, remember these three:

### 1. Single inheritance

```text
A
↑
B
```

One parent → one child.

---

### 2. Multilevel inheritance

```text
A
↑
B
↑
C
```

Inheritance chain.

---

### 3. Multiple inheritance

```text
A   B
 \ /
  C
```

One child → multiple parents.

---

# 10.22 Comparison

| Type | Structure | Example |
|---|---|---|
| Single | `A → B` | `Dog(Animal)` |
| Multilevel | `A → B → C` | `Puppy(Dog)`, `Dog(Animal)` |
| Multiple | `A + B → C` | `Child(Father, Mother)` |

---

# 10.23 Real-World Example

Consider a backend application:

```python
class APIClient:

    def connect(self):
        print("Connecting to API")


class OpenAIClient(APIClient):

    def generate(self):
        print("Generating response")
```

Now:

```python
client = OpenAIClient()

client.connect()
client.generate()
```

The specialized client reuses common API behavior.

Conceptually:

```text
APIClient
│
├── connect()
│
↓
OpenAIClient
│
└── generate()
```

This pattern can be useful when several related classes share a genuine common abstraction.

However, in real Python applications, don't automatically use inheritance just to reuse a few lines of code. **Composition** is often a better choice when the relationship is "has-a" rather than "is-a."

---

# 10.24 Inheritance vs Composition — Early Preview

### Inheritance

```text
Dog is an Animal
```

```python
class Dog(Animal):
    pass
```

### Composition

```text
Car has an Engine
```

```python
class Car:

    def __init__(self):
        self.engine = Engine()
```

So:

```text
"is-a"  → inheritance
"has-a" → composition
```

We'll study composition separately.

---

# 10.25 Common Mistakes

### Mistake 1: Thinking inheritance copies methods

Incorrect mental model:

```text
Parent method copied into child
```

Better:

```text
Child lookup
    ↓
Parent
    ↓
method found
```

---

### Mistake 2: Thinking parent's `__init__()` always runs

It doesn't automatically run when the child defines its own `__init__()`.

Use `super().__init__()` when parent initialization needs to be performed.

---

### Mistake 3: Using inheritance only for code reuse

Inheritance should usually represent a meaningful **is-a relationship**.

Don't create:

```text
Class B inherits Class A
```

only because A contains one convenient function.

Composition or a standalone helper may be more appropriate.

---

### Mistake 4: Ignoring MRO in multiple inheritance

When multiple parents contain the same method, Python needs a deterministic resolution order.

That's what MRO handles.

---

# 10.26 Interview Questions

### What is inheritance?

> **Inheritance is an OOP mechanism where a child class derives from a parent class and can reuse, extend, or override its attributes and methods.**

### What is a parent/base class?

> **The class from which another class inherits is called the parent or base class.**

### What is a child/derived class?

> **A class that inherits from another class is called a child or derived class.**

### Why is inheritance used?

> **It allows related classes to share common behavior and enables specialization, extension, and polymorphism.**

### What is single inheritance?

> **When a class inherits from one parent class.**

### What is multilevel inheritance?

> **When inheritance forms a chain, such as `A → B → C`.**

### What is multiple inheritance?

> **When a class inherits from more than one parent class, such as `class C(A, B)`.**

### What is MRO?

> **Method Resolution Order is the order Python follows when searching the inheritance hierarchy for an attribute or method.**

### Does Python support multiple inheritance?

> **Yes. Python supports multiple inheritance and uses MRO to determine method lookup order.**

### What is the difference between `isinstance()` and `issubclass()`?

> **`isinstance()` checks an object's relationship with a class, while `issubclass()` checks the relationship between two classes.**

---

# 10.27 Final Mental Model

Think of inheritance as a **lookup relationship**, not simply copying.

```text
                    object
                       ↑
                    Animal
                       ↑
                      Dog
                       ↑
                    Puppy
```

If you have:

```python
puppy = Puppy()
```

and write:

```python
puppy.eat()
```

Python can conceptually search:

```text
puppy
  ↓
Puppy
  ↓
Dog
  ↓
Animal
  ↓
object
```

until it finds `eat()`.

For multiple inheritance:

```text
        Father      Mother
           \         /
            \       /
             Child
               ↑
             object
```

Python uses **MRO** to determine the exact search order.

---

# 10.28 The 5 Things to Remember

```text
1. Parent/Base class
   → class being inherited from

2. Child/Derived class
   → class that inherits

3. Single inheritance
   → one parent

4. Multilevel inheritance
   → inheritance chain

5. Multiple inheritance
   → multiple parents
```

And the most important mental model:

> **Inheritance allows a child class to reuse and extend a parent's behavior, while Python's attribute lookup searches through the inheritance hierarchy when necessary.**

---

## Priority

| Concept | Priority |
|---|---|
| Meaning of inheritance | 🔥🔥 Core |
| Parent / base class | 🔥🔥 Core |
| Child / derived class | 🔥🔥 Core |
| Why inheritance | 🔥 Core |
| `class Child(Parent)` syntax | 🔥🔥 Core |
| Inherited methods/attributes | 🔥🔥 Core |
| Single inheritance | 🔥 Core |
| Multilevel inheritance | 🔥 Core |
| Multiple inheritance | 🔥 Core |
| `isinstance()` with inheritance | 🟡 Know & Move On |
| `issubclass()` | 🟡 Know & Move On |
| `object` as base class | 🟡 Know & Move On |
| MRO details | 🟡 Know & Move On — deeper later |
| `super()` | 🔥 Core — next related topic |
| Composition vs inheritance | 🟡 Know & Move On — deeper later |