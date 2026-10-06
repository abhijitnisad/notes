# 13. MRO — Method Resolution Order 🟡 Know & Move On

## 13.1 What is MRO?

**MRO (Method Resolution Order)** stands for **Method Resolution Order**.

It is the order in which Python searches classes when looking for an attribute or method.

This becomes especially important with **inheritance and multiple inheritance**.

Example:

```python
class Animal:

    def sound(self):
        print("Animal")


class Dog(Animal):
    pass
```

For:

```python
dog = Dog()

dog.sound()
```

Python needs to find:

```python
sound
```

It can search:

```text
Dog
 ↓
Animal
 ↓
object
```

`Dog` doesn't define `sound()`, so Python finds it in `Animal`.

### Simple definition

> **MRO is the order Python follows to search the class hierarchy for an attribute or method.**

---

# 13.2 Why Do We Need MRO?

With simple inheritance, the lookup order is easy.

```text
Animal
  ↑
 Dog
```

Python can search:

```text
Dog → Animal → object
```

But multiple inheritance can look like:

```text
Father       Mother
    \         /
     \       /
      Child
```

Suppose both `Father` and `Mother` define:

```python
show()
```

Which one should `Child` use?

Python needs a **consistent and deterministic order**.

That's what MRO provides.

---

# 13.3 MRO With Single Inheritance

Consider:

```python
class Animal:

    def eat(self):
        print("Eating")


class Dog(Animal):
    pass
```

Now:

```python
dog = Dog()

dog.eat()
```

Simplified lookup:

```text
dog.eat()
   ↓
Dog
   ↓
eat not found
   ↓
Animal
   ↓
eat found
   ↓
Animal.eat()
```

The MRO is:

```text
Dog → Animal → object
```

---

# 13.4 Checking MRO With `__mro__`

Python provides the `__mro__` attribute.

Example:

```python
class Animal:
    pass


class Dog(Animal):
    pass


print(Dog.__mro__)
```

You will see something conceptually like:

```text
(<class '__main__.Dog'>,
 <class '__main__.Animal'>,
 <class 'object'>)
```

So:

```text
Dog → Animal → object
```

is the MRO.

---

# 13.5 `Class.mro()`

Python also provides:

```python
Class.mro()
```

Example:

```python
class Animal:
    pass


class Dog(Animal):
    pass


print(Dog.mro())
```

Output is conceptually:

```text
[
    Dog,
    Animal,
    object
]
```

### `mro()` vs `__mro__`

Both show the method resolution order.

```python
Dog.mro()
```

returns the MRO as a list.

```python
Dog.__mro__
```

exposes the MRO as a tuple.

For example:

```python
print(Dog.mro())
print(Dog.__mro__)
```

The information is essentially the same, but the container type differs.

### For your notes

Remember:

```text
Class.mro()
→ returns MRO as a list

Class.__mro__
→ MRO as a tuple
```

---

# 13.6 MRO With Method Overriding

Consider:

```python
class Animal:

    def sound(self):
        print("Animal")


class Dog(Animal):

    def sound(self):
        print("Dog")
```

Now:

```python
dog = Dog()

dog.sound()
```

Python searches:

```text
Dog
 ↓
sound found
 ↓
Dog.sound()
```

It doesn't continue to `Animal` because it already found the method.

Output:

```text
Dog
```

### Important connection

This is why method overriding works.

> **The child class appears before the parent class in the MRO.**

Therefore, the child's implementation is found first.

---

# 13.7 MRO With Multilevel Inheritance

Consider:

```python
class Animal:

    def sound(self):
        print("Animal")


class Dog(Animal):
    pass


class Puppy(Dog):
    pass
```

The MRO of `Puppy` is:

```text
Puppy
 ↓
Dog
 ↓
Animal
 ↓
object
```

So:

```python
puppy = Puppy()

puppy.sound()
```

searches:

```text
Puppy
 ↓
Dog
 ↓
Animal
 ↓
sound found
```

Output:

```text
Animal
```

If `Dog` overrides it:

```python
class Dog(Animal):

    def sound(self):
        print("Dog")
```

then:

```text
Puppy
 ↓
Dog
 ↓
sound found
```

Output:

```text
Dog
```

---

# 13.8 Multiple Inheritance

Now consider:

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

Here:

```text
Father      Mother
    \        /
     \      /
      Child
```

The child inherits from both classes.

Now:

```python
child = Child()

child.show()
```

Which method runs?

Python uses the MRO.

For this simple hierarchy, the MRO is:

```text
Child → Father → Mother → object
```

Therefore:

```python
child.show()
```

outputs:

```text
Father
```

because `Father` appears before `Mother`.

---

# 13.9 Checking Multiple-Inheritance MRO

You can verify it:

```python
print(Child.mro())
```

Conceptually:

```text
[
    Child,
    Father,
    Mother,
    object
]
```

So when Python searches for `show()`:

```text
Child
 ↓
show not found
 ↓
Father
 ↓
show found
 ↓
stop
```

It doesn't reach `Mother`.

---

# 13.10 Changing the Parent Order

Consider:

```python
class Child(Mother, Father):
    pass
```

Now the MRO changes.

Conceptually:

```text
Child → Mother → Father → object
```

Therefore:

```python
child.show()
```

will use:

```text
Mother.show()
```

Output:

```text
Mother
```

This shows why the order in:

```python
class Child(Father, Mother):
```

matters.

---

# 13.11 MRO and `super()`

This is one of the most important connections.

You learned:

```python
super()
```

doesn't technically mean:

> "Call my parent."

It means:

> **Continue method lookup according to the MRO.**

Example:

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

The MRO is:

```text
D → B → C → A → object
```

Now:

```python
d = D()

d.show()
```

Output:

```text
D
B
C
A
```

Why?

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

This is why `super()` and MRO are closely connected.

---

# 13.12 The Diamond Problem

Multiple inheritance can create a structure called the **diamond problem**.

Example:

```text
        A
       / \
      B   C
       \ /
        D
```

In Python:

```python
class A:
    pass


class B(A):
    pass


class C(A):
    pass


class D(B, C):
    pass
```

There are two paths from `D` to `A`:

```text
D → B → A

D → C → A
```

Python must determine a single consistent order.

A simplified MRO is:

```text
D → B → C → A → object
```

The MRO ensures that the hierarchy is searched in a consistent way and that common ancestors such as `A` are handled appropriately.

---

# 13.13 Why Doesn't Python Simply Use "Left to Right"?

For simple multiple inheritance:

```python
class Child(Father, Mother):
    pass
```

the result often looks like:

```text
Child → Father → Mother → object
```

So beginners may think:

> "MRO is just left to right."

That's **not always correct**.

Python uses a formal algorithm called **C3 linearization** to calculate the MRO.

You don't need to memorize the algorithm right now.

For your level, remember:

> **Python uses C3 linearization to produce a consistent MRO for multiple inheritance.**

---

# 13.14 C3 Linearization — Only What You Need

You don't need to learn the full mathematical algorithm now.

Just understand what it guarantees:

- a class appears before its parents
- parent ordering is respected
- the same class isn't unnecessarily repeated
- the resulting order is consistent with Python's inheritance rules

Example:

```text
D → B → C → A → object
```

is one valid MRO generated by these rules.

### Priority

```text
Understand MRO → 🔥 useful
Memorize C3 algorithm → ⚪ not needed now
```

---

# 13.15 MRO and Attribute Lookup

MRO isn't only about methods.

It is used for normal class attribute lookup as well.

Example:

```python
class A:
    value = "A"


class B(A):
    pass


class C(B):
    pass
```

Now:

```python
c = C()

print(c.value)
```

Python can search:

```text
C
 ↓
B
 ↓
A
 ↓
value found
```

So although the name says **Method Resolution Order**, it is involved in the broader process of resolving attributes through the class hierarchy.

### Simple mental model

```text
object.attribute
      ↓
instance / descriptor rules
      ↓
class hierarchy according to MRO
```

For your current level, remember that MRO determines the class search order.

---

# 13.16 MRO and `super()` — Side-by-Side

Consider:

```python
class A:

    def show(self):
        print("A")


class B(A):

    def show(self):
        print("B")
        super().show()
```

MRO:

```text
B → A → object
```

Inside:

```python
super().show()
```

Python continues after `B`:

```text
B
 ↓
A
 ↓
show()
```

So it calls:

```python
A.show()
```

This gives:

```text
B
A
```

### Key idea

> **`super()` follows the MRO; it doesn't simply search for a hard-coded parent.**

---

# 13.17 `mro()` Example You Should Know

```python
class A:
    pass


class B(A):
    pass


class C(A):
    pass


class D(B, C):
    pass


print(D.mro())
```

Conceptually:

```text
[
    D,
    B,
    C,
    A,
    object
]
```

Visualize it:

```text
        A
       / \
      B   C
       \ /
        D

MRO:

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

---

# 13.18 What Happens When a Method Isn't Found?

Suppose:

```python
class A:

    def show(self):
        print("A")


class B(A):
    pass


class C(B):
    pass
```

And:

```python
c = C()
```

When:

```python
c.show()
```

is called:

```text
C
 ↓
show not found

B
 ↓
show not found

A
 ↓
show found

execute A.show()
```

If Python reaches the end of the relevant lookup hierarchy without finding the attribute, an `AttributeError` is raised for normal attribute access.

---

# 13.19 Method Lookup Mental Model

For:

```python
obj.method()
```

think:

```text
1. Start with the object/class lookup machinery
          ↓
2. Search the object's class
          ↓
3. Follow the class's MRO
          ↓
4. Find the first appropriate implementation
          ↓
5. Bind/call it as appropriate
```

This is simplified. Python's actual attribute lookup also involves **descriptors**, which you don't need to study deeply yet.

---

# 13.20 Common Mistakes

### Mistake 1: Thinking MRO means only parent order

MRO is the **complete class search order**, including the class itself and ultimately `object`.

Example:

```text
D → B → C → A → object
```

---

### Mistake 2: Thinking `super()` always means direct parent

Better:

```text
super()
→ next class according to MRO
```

---

### Mistake 3: Assuming multiple inheritance is always left-to-right

Simple cases often look that way, but Python actually uses **C3 linearization** to calculate the MRO.

---

### Mistake 4: Ignoring the order in the class definition

These are not necessarily equivalent:

```python
class Child(A, B):
    pass
```

and:

```python
class Child(B, A):
    pass
```

The MRO can change, which can change which implementation is found first.

---

### Mistake 5: Trying to memorize C3 too early

For your current Python + GenAI goal, you should understand:

```text
MRO → what it is
mro() → how to inspect it
multiple inheritance → why MRO matters
super() → follows MRO
```

You don't need to manually calculate complicated C3 linearization yet.

---

# 13.21 Interview Questions

### What is MRO?

> **MRO, or Method Resolution Order, is the order in which Python searches classes in an inheritance hierarchy when resolving an attribute or method.**

### Why is MRO important?

> **It determines which implementation Python uses, especially when multiple inheritance creates multiple possible sources for the same method.**

### How can you see a class's MRO?

Using:

```python
Class.mro()
```

or:

```python
Class.__mro__
```

### What is the difference?

```python
Class.mro()
```

returns the MRO as a list, while:

```python
Class.__mro__
```

provides it as a tuple.

### What is the MRO of:

```python
class C(A, B):
    pass
```

in a simple compatible hierarchy?

> It begins with `C` and then follows Python's MRO rules through `A`, `B`, their ancestors, and finally `object`. The exact complete order should be checked with `C.mro()` rather than guessed.

### What is the connection between `super()` and MRO?

> **`super()` follows the MRO to access the next appropriate implementation, which is why it works correctly with cooperative multiple inheritance.**

### What is C3 linearization?

> **C3 linearization is the algorithm Python uses to calculate a consistent MRO for multiple inheritance.**

---

# 13.22 Backend / GenAI Relevance

You are unlikely to manually design complicated multiple-inheritance hierarchies in most backend or GenAI applications.

But you **will encounter MRO indirectly** when working with Python frameworks and libraries that use inheritance.

Understanding MRO helps you understand code such as:

```python
class CustomClient(BaseClient, SomeMixin):
    ...
```

and:

```python
super().__init__(...)
```

especially when frameworks use mixins and cooperative inheritance.

For your learning goal, the practical knowledge is:

```text
Inheritance
    ↓
Method overriding
    ↓
super()
    ↓
MRO
```

These four concepts form one connected chain.

---

# 13.23 Final Mental Model

Remember this hierarchy:

```text
                    object
                      ↑
                      A
                    /   \
                   B     C
                    \   /
                      D
```

For:

```python
class D(B, C):
    pass
```

Python determines an MRO such as:

```text
D → B → C → A → object
```

When you call:

```python
d.some_method()
```

Python searches according to that order.

If:

```python
B.some_method()
```

exists:

```text
D
 ↓
B
 ↓
found
```

it uses `B`'s implementation.

And if `B.some_method()` contains:

```python
super().some_method()
```

Python continues according to the MRO:

```text
B
 ↓
C
 ↓
A
 ↓
...
```

### One-line memory trick

> **MRO = the order Python follows through the inheritance hierarchy to find the appropriate implementation.**

---

## Priority

| Concept | Priority |
|---|---|
| Meaning of MRO | 🟡 Know & Move On |
| Why MRO is needed | 🔥 Core |
| MRO with single inheritance | 🔥 Core |
| MRO with multiple inheritance | 🔥 Core |
| `Class.mro()` | 🔥 Core |
| `Class.__mro__` | 🟡 Know & Move On |
| MRO + method overriding | 🔥 Core |
| MRO + `super()` | 🔥🔥 Core |
| Diamond inheritance | 🟡 Know & Move On |
| C3 linearization concept | 🟡 Know & Move On |
| Calculating complex C3 manually | ⚪ Optional for Now |
| Descriptor-level lookup | ⚪ Optional for Now |