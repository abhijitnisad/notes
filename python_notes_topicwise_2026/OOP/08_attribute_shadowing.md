# 8. Attribute Shadowing 🟡 Know & Move On

## 8.1 What is Attribute Shadowing?

**Attribute shadowing** happens when an instance and its class have an attribute with the **same name**.

The instance-level attribute **shadows** (hides for that instance) the class-level attribute during normal attribute lookup.

Example:

```python
class Student:
    college = "TIT Technocrats"


student = Student()

print(student.college)
```

Output:

```text
TIT Technocrats
```

At this point, `student` does not have its own `college`, so Python finds it on the class.

Now:

```python
student.college = "RGPV"
```

This creates an instance attribute:

```text
Student
└── college → "TIT Technocrats"

student
└── college → "RGPV"
```

Now:

```python
print(student.college)
```

Output:

```text
RGPV
```

But:

```python
print(Student.college)
```

still gives:

```text
TIT Technocrats
```

The instance attribute is **shadowing** the class attribute.

---

# 8.2 Why Does the Instance Attribute Take Precedence?

When Python evaluates:

```python
student.college
```

it needs to determine which `college` you mean.

A simplified mental model is:

```text
student.college
      ↓
Check instance
      ↓
Is "college" there?
      │
   ┌──┴──┐
  Yes    No
   ↓      ↓
Use it   Check class
```

So if the instance already has an attribute named `college`, Python can use that value instead of the class's value.

Example:

```python
class Student:
    college = "TIT Technocrats"


student = Student()

student.college = "RGPV"

print(student.__dict__)
```

Output:

```text
{'college': 'RGPV'}
```

Now:

```python
student.college
```

finds:

```text
student.__dict__
        ↓
college = "RGPV"
```

before needing to obtain the class-level value.

---

# 8.3 Step-by-Step Example

Consider:

```python
class Student:

    college = "TIT Technocrats"

    def __init__(self, name):
        self.name = name


student = Student("Abhijit")
```

Initially:

```text
Student class
└── college → "TIT Technocrats"

student
└── name → "Abhijit"
```

Now:

```python
print(student.college)
```

Python conceptually does:

```text
student
  ↓
Does student have college?
  ↓
No
  ↓
Check Student
  ↓
college found
  ↓
"TIT Technocrats"
```

Now execute:

```python
student.college = "RGPV"
```

The state becomes:

```text
Student class
└── college → "TIT Technocrats"

student
├── name → "Abhijit"
└── college → "RGPV"
```

Now:

```python
print(student.college)
```

conceptually:

```text
student
  ↓
Does student have college?
  ↓
Yes
  ↓
"RGPV"
```

The class attribute is still there; the instance value simply **shadows** it for this object.

---

# 8.4 Shadowing Does NOT Change the Class Attribute

This is the most important point.

```python
class Student:
    college = "TIT Technocrats"


student = Student()

student.college = "RGPV"
```

It is incorrect to think:

```text
student.college = "RGPV"
        ↓
Student.college changes
```

Instead:

```text
student.college = "RGPV"
        ↓
create/update student-level attribute
```

Therefore:

```python
print(student.college)
```

gives:

```text
RGPV
```

while:

```python
print(Student.college)
```

gives:

```text
TIT Technocrats
```

---

# 8.5 Multiple Objects Can Shadow Independently

This is where the concept becomes very clear.

```python
class Student:
    college = "TIT Technocrats"


student1 = Student()
student2 = Student()

student1.college = "RGPV"
```

Now:

```python
print(student1.college)
print(student2.college)
print(Student.college)
```

Output:

```text
RGPV
TIT Technocrats
TIT Technocrats
```

Why?

```text
                 Student
                    │
          college = "TIT Technocrats"
                    │
          ┌─────────┴─────────┐
          ↓                   ↓
      student1            student2
      college=RGPV         no college
          │                   │
          ↓                   ↓
        RGPV            finds class value
```

Only `student1` has its own `college`.

---

# 8.6 Shadowing vs Changing the Class Attribute

Compare these two statements:

### Instance assignment

```python
student.college = "RGPV"
```

Creates/updates the instance attribute.

Result:

```text
student.college → RGPV
Student.college → TIT Technocrats
```

### Class assignment

```python
Student.college = "RGPV"
```

Changes the class attribute.

Result for instances without their own `college`:

```text
student1.college → RGPV
student2.college → RGPV
Student.college → RGPV
```

So remember:

```python
student.college
```

and

```python
Student.college
```

are not equivalent when the instance has shadowed the attribute.

---

# 8.7 How `__dict__` Makes Shadowing Visible

This is an excellent way to understand what actually happened.

```python
class Student:
    college = "TIT Technocrats"


student = Student()

print(student.__dict__)
```

Initially:

```text
{}
```

There is no instance-level `college`.

Now:

```python
student.college = "RGPV"
```

Check again:

```python
print(student.__dict__)
```

Output:

```text
{'college': 'RGPV'}
```

Meanwhile:

```python
print(Student.__dict__["college"])
```

still represents the class-level value:

```text
TIT Technocrats
```

Conceptually:

```text
student.__dict__
└── college → "RGPV"

Student.__dict__
└── college → "TIT Technocrats"
```

This makes the shadowing relationship very clear.

---

# 8.8 Shadowing Is Not Copying

A common misconception is:

> "When I create an instance attribute, Python copies the class attribute and replaces it."

That's not what happens.

Suppose:

```python
class Student:
    college = "TIT Technocrats"
```

and:

```python
student = Student()
```

The instance does not automatically receive:

```python
student.__dict__["college"]
```

Instead, if the instance doesn't have `college`, attribute lookup can find:

```python
Student.college
```

After:

```python
student.college = "RGPV"
```

the instance gets its **own** attribute.

So:

```text
Before shadowing:

student.__dict__
└── {}

Student.__dict__
└── college → "TIT Technocrats"


After shadowing:

student.__dict__
└── college → "RGPV"

Student.__dict__
└── college → "TIT Technocrats"
```

---

# 8.9 Why Is Instance Lookup Designed This Way?

The practical reason is that objects often need to **override a general class-level value for themselves**.

Imagine:

```python
class Employee:
    company = "ABC"


employee1 = Employee()
employee2 = Employee()
```

Normally both use:

```text
ABC
```

But suppose one employee changes company information for their own object:

```python
employee1.company = "XYZ"
```

Now:

```text
employee1.company → XYZ
employee2.company → ABC
```

The class still provides the default/shared value, while an individual instance can have its own value.

This gives Python a useful model:

> **Class-level value can act as a shared/default value, while an instance can provide its own value when needed.**

---

# 8.10 A Very Important Distinction

Don't confuse **shadowing** with **modifying a mutable class attribute**.

Consider:

```python
class Student:
    subjects = []
```

Then:

```python
student1 = Student()
student2 = Student()

student1.subjects.append("Python")
```

This does **not** create:

```python
student1.subjects
```

Instead, both instances may still resolve `subjects` to the same class-level list.

So:

```text
Student
└── subjects → ["Python"]
       ↑
       │
  ┌────┴────┐
  │         │
student1  student2
```

This is **not ordinary shadowing**.

Shadowing would happen if you did:

```python
student1.subjects = ["Python"]
```

Now `student1` has its own `subjects` attribute:

```text
Student
└── subjects → []

student1
└── subjects → ["Python"]

student2
└── no subjects
```

Therefore:

```python
student1.subjects.append("Python")
```

and:

```python
student1.subjects = ["Python"]
```

can have very different effects.

---

# 8.11 Attribute Shadowing Mental Model

Remember this:

```text
                 CLASS
          ┌─────────────────┐
          │ college = TIT   │
          └─────────────────┘
                   │
          ┌────────┴────────┐
          ↓                 ↓
      student1          student2
   college = RGPV       no college
          │                 │
          ↓                 ↓
        RGPV        → finds class value
```

### The core rule

> **If an instance has an attribute with the same name as a class attribute, the instance-level value normally shadows the class-level value for that instance.**

---

# 8.12 Interview Answer

### What is attribute shadowing in Python?

> **Attribute shadowing occurs when an instance defines an attribute with the same name as an attribute on its class. During normal attribute access, the instance-level attribute takes precedence, so it hides the class-level attribute for that particular instance.**

Example:

```python
class Student:
    college = "TIT"


s = Student()

s.college = "RGPV"

print(s.college)      # RGPV
print(Student.college) # TIT
```

---

# 8.13 Common Interview Trap

### Question:

```python
class A:
    x = 10


a = A()

a.x = 20

print(a.x)
print(A.x)
```

Answer:

```text
20
10
```

Why?

Because:

```python
a.x = 20
```

creates an instance attribute named `x`.

It does not modify:

```python
A.x
```

So:

```text
a.__dict__
└── x → 20

A.__dict__
└── x → 10
```

---

# 8.14 Backend / GenAI Relevance

Shadowing becomes useful when an object has a **default class-level configuration** but an individual object needs a different value.

For example:

```python
class LLMClient:

    default_temperature = 0.7

    def __init__(self, model):
        self.model = model
```

Normally:

```python
client1 = LLMClient("model-a")
client2 = LLMClient("model-b")

print(client1.default_temperature)
print(client2.default_temperature)
```

Both can resolve:

```text
0.7
```

If one client needs a different value:

```python
client1.default_temperature = 0.2
```

Now:

```text
client1.default_temperature → 0.2
client2.default_temperature → 0.7
LLMClient.default_temperature → 0.7
```

This is the same instance-over-class shadowing concept.

In real applications, configuration is often handled with instance attributes, dataclasses, dependency injection, or dedicated configuration objects rather than relying heavily on class-level mutable state.

---

# 8.15 What You Should Remember

### 1. Same name can exist at both levels

```python
class Student:
    college = "TIT"


student = Student()

student.college = "RGPV"
```

Now both exist:

```text
Student.college → TIT
student.college → RGPV
```

### 2. Instance value normally wins for that instance

```python
student.college
```

finds the instance's `college`.

### 3. Shadowing doesn't delete the class attribute

```python
Student.college
```

still exists.

### 4. Other instances are unaffected

```python
student2.college
```

can still find the class value.

### 5. Shadowing is different from mutating a shared class object

This distinction becomes especially important with lists, dictionaries, and other mutable objects.

---

## One-Line Interview Memory

> **Attribute shadowing means an instance attribute with the same name as a class attribute hides the class attribute for that particular instance because instance-level lookup takes precedence in the normal lookup model.**

---

## Priority

| Concept | Priority |
|---|---|
| Meaning of attribute shadowing | 🟡 Know & Move On |
| Instance attribute vs class attribute with same name | 🔥 Core |
| Why instance value takes precedence | 🔥 Core |
| `student.__dict__` demonstration | 🟡 Know & Move On |
| Shadowing vs changing class attribute | 🔥 Core |
| Shadowing vs mutable class attribute | 🔥 Core |
| Descriptor-level lookup details | ⚪ Optional for Now |