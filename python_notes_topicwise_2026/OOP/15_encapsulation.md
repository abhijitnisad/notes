# 15. Encapsulation 🔥 Core

## 15.1 What Is Encapsulation?

**Encapsulation** is the practice of keeping data and the methods that operate on that data together, while providing a clear interface for interacting with the object.

It helps protect an object's internal state from accidental misuse and makes code easier to maintain.

Example: A bank account should not allow its balance to become negative through an invalid withdrawal.

```python
class BankAccount:

    def __init__(self, balance):
        self._balance = balance

    def deposit(self, amount):
        if amount > 0:
            self._balance += amount

    def get_balance(self):
        return self._balance


account = BankAccount(1000)
account.deposit(500)

print(account.get_balance())  # 1500
```

Here, the account's data and related behavior are kept inside one class. The `deposit()` method provides a controlled way to change the balance.

**Important:** `_balance` is a naming convention, not a strict access restriction. Python still allows `account._balance` to be accessed directly.

## 15.2 Public Attributes

A **public attribute** can be accessed directly from outside the class.

In Python, ordinary attribute names without leading underscores are public by convention.

```python
class Student:

    def __init__(self, name, age):
        self.name = name
        self.age = age


student = Student("Abhijit", 21)

print(student.name)
print(student.age)

student.age = 22
print(student.age)
```

Output:

```text
Abhijit
21
22
```

Here, `name` and `age` are public attributes.

### When to use public attributes?

Use them when direct access is appropriate and there is no need for additional validation or control.

For example:

```python
student.name = "Rahul"
```

This is simple and readable, but any value can be assigned unless you add some form of validation.

## 15.3 Protected Attributes — `_attribute`

In Python, a **single leading underscore** indicates that an attribute is intended for internal use.

```python
class Employee:

    def __init__(self, name, salary):
        self.name = name
        self._salary = salary


employee = Employee("Aman", 30000)

print(employee.name)
print(employee._salary)  # Accessible, but intended for internal use
```

The underscore communicates:

> This attribute is an implementation detail. External code should generally avoid depending on it.

Python does not enforce protected access. The attribute remains accessible.

This convention is particularly useful when designing classes that may be extended through inheritance.

```python
class Employee:

    def __init__(self, salary):
        self._salary = salary


class Manager(Employee):

    def show_salary(self):
        print(self._salary)
```

The subclass can access `_salary`, as expected.

**Remember:** `_salary` is a convention, not a security mechanism.

## 15.4 Private Attributes — `__attribute`

A **double leading underscore** triggers name mangling in ordinary Python class definitions.

```python
class BankAccount:

    def __init__(self, balance):
        self.__balance = balance

    def get_balance(self):
        return self.__balance


account = BankAccount(5000)

print(account.get_balance())  # 5000
```

But this normally fails:

```python
print(account.__balance)
```

Python raises an `AttributeError` because the attribute is stored under a mangled name rather than the literal name `__balance`.

This helps avoid accidental access and name collisions, particularly in inheritance.

**Important:** Double underscores do not make an attribute truly inaccessible or secure. Python uses name mangling rather than strict private access control.

## 15.5 What Is Name Mangling?

**Name mangling** is Python's mechanism for transforming certain names that begin with at least two leading underscores and do not end with two trailing underscores.

Inside a class named `BankAccount`, the attribute:

```python
self.__balance = balance
```

is effectively stored under a name like:

```python
self._BankAccount__balance
```

Example:

```python
class BankAccount:

    def __init__(self, balance):
        self.__balance = balance


account = BankAccount(5000)

print(account.__dict__)
print(account._BankAccount__balance)
```

Output:

```text
{'_BankAccount__balance': 5000}
5000
```

The second line works because the mangled name can still be accessed directly. This is a demonstration of Python's behavior, **not a recommended way to access private-style attributes**.

### Why does name mangling exist?

One major purpose is to prevent accidental name clashes between a base class and a subclass.

```python
class Parent:

    def __init__(self):
        self.__value = "Parent value"


class Child(Parent):

    def __init__(self):
        super().__init__()
        self.__value = "Child value"


obj = Child()

print(obj.__dict__)
```

Output:

```text
{
    '_Parent__value': 'Parent value',
    '_Child__value': 'Child value'
}
```

The two attributes coexist because their names are mangled differently.

Without name mangling, both classes using the same ordinary attribute name could unintentionally overwrite each other's state.

### Which names are mangled?

| Attribute | Mangled? |
|---|---|
| `name` | No |
| `_name` | No |
| `__name` | Yes, in a class context |
| `__name__` | No — conventionally a special or dunder name |

Name mangling is based on the class name, with leading underscores in the class name stripped for the transformation. It is not a security feature.

## 15.6 Public vs Protected vs Private

| Naming style | Example | Meaning |
|---|---|---|
| Public | `self.name` | Intended for normal external use |
| Protected convention | `self._salary` | Intended for internal or subclass use |
| Private-style | `self.__balance` | Name-mangled to reduce accidental clashes |

### Easy way to remember

- `name` → public
- `_name` → internal-use convention
- `__name` → name mangling

Python generally relies on conventions and developer discipline rather than strict access modifiers like `private` in Java.

## 15.7 Encapsulation With Controlled Access

Suppose a bank account must prevent invalid withdrawals. Directly exposing the balance as an unrestricted public attribute makes that rule harder to enforce.

A better design can use a property:

```python
class BankAccount:

    def __init__(self, balance):
        self._balance = balance

    @property
    def balance(self):
        return self._balance

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("Amount must be positive")

        self._balance += amount

    def withdraw(self, amount):
        if amount <= 0:
            raise ValueError("Amount must be positive")

        if amount > self._balance:
            raise ValueError("Insufficient balance")

        self._balance -= amount


account = BankAccount(1000)

account.deposit(500)
account.withdraw(200)

print(account.balance)  # 1300
```

Here:

- `_balance` is an internal-use attribute by convention.
- `@property` exposes a read-only interface named `balance`.
- `deposit()` and `withdraw()` enforce business rules.
- `account.balance` looks like normal attribute access, but calls the property's getter.

Notice that `@property` makes the public interface convenient; it does not make `_balance` inaccessible. A caller could still deliberately assign `account._balance = -100`.

For a writable property, you can define a setter with `@balance.setter`. We'll study `@property` in detail separately.

## 15.8 Why Is Encapsulation Useful?

**1. Data integrity**

Methods can validate values before modifying internal state.

**2. Reduced accidental misuse**

Underscore conventions communicate which attributes are implementation details.

**3. Easier maintenance**

You can change the internal implementation while keeping the public interface stable.

**4. Clear responsibilities**

A class controls the rules for changing its own state.

**5. Safer inheritance**

Name mangling reduces accidental attribute-name collisions between base classes and subclasses.

### Backend and GenAI relevance

For example, an API client might keep its internal configuration or retry state under underscore-prefixed names and expose public methods such as `generate()`.

A service can validate requests through methods rather than allowing arbitrary changes to its internal state.

Encapsulation improves code organization and correctness, but it is **not a replacement for security controls**, authentication, authorization, or secret management.

## 15.9 Common Interview Questions

**Q1. What is encapsulation?**

Encapsulation combines data and related behavior within a class and provides a controlled interface for interacting with an object's state.

**Q2. Does Python support private variables?**

Python supports private-style names through double-leading-underscore name mangling. It does not enforce strict private access in the same way as languages with access modifiers.

**Q3. What is the difference between `_name` and `__name`?**

`_name` is a convention indicating internal use. `__name` triggers name mangling inside a class, which helps avoid accidental name clashes.

**Q4. Is name mangling the same as data security?**

No. It changes the attribute's name to reduce accidental collisions. It does not prevent deliberate access.

**Q5. Why is encapsulation useful?**

It helps maintain data integrity, reduces accidental misuse, separates public interfaces from implementation details, and makes classes easier to maintain.

**Q6. Does encapsulation require every attribute to be private?**

No. Public attributes are appropriate when direct access is safe and simple. Use internal conventions and controlled methods where they provide a genuine benefit.

## Final Mental Model

```text
Encapsulation
    |
    ├── Public:  name
    |      └── Normal direct access
    |
    ├── Protected convention: _name
    |      └── Intended for internal use
    |
    └── Private-style: __name
           └── Name mangling
               └── Helps prevent accidental name clashes
```

**One-line interview answer:**

Encapsulation keeps data and its related behavior together and provides a clear interface for working with object state. Python uses naming conventions and name mangling to discourage accidental misuse, but these do not provide strict private access control.

## Priority

| Concept | Priority |
|---|---|
| Encapsulation and its purpose | 🔥 Core |
| Public attributes | 🔥 Core |
| `_protected` naming convention | 🔥 Core |
| `__private` and name mangling | 🔥 Core |
| Encapsulation with validation | 🔥 Core |
| `@property` and setters | 🔥 Core — study separately |
| Security implications and advanced internals | 🟡 Know & Move On |