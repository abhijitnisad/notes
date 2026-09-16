# Recursion ⭐ 🟡 Know & Move On

**Recursion** is a programming technique where a function **calls itself** to solve a problem by breaking it into smaller versions of the same problem.

For your Python → GenAI/Agentic AI path, you should understand recursion clearly, but you do **not** need extensive recursion problem-solving practice right now.

---

# 71. What is Recursion?

Recursion occurs when a function calls itself.

Simple example:

```python
def countdown(n):
    if n == 0:
        return

    print(n)
    countdown(n - 1)


countdown(3)
```

Output:

```text
3
2
1
```

The function keeps calling itself:

```text
countdown(3)
    ↓
countdown(2)
    ↓
countdown(1)
    ↓
countdown(0)
```

At `0`, the function stops.

---

# 72. Recursive Function

A **recursive function** is a function that calls itself directly or indirectly.

Example:

```python
def countdown(n):
    if n == 0:
        return

    print(n)
    countdown(n - 1)
```

Here:

```python
countdown(n - 1)
```

is the recursive call.

A recursive function generally needs two important parts:

```text
Base case
    +
Recursive case
```

---

# 73. Base Case

The **base case** is the condition that tells the recursive function:

> "Stop making recursive calls."

Example:

```python
def countdown(n):
    if n == 0:       # Base case
        return

    print(n)
    countdown(n - 1)
```

Here:

```python
if n == 0:
    return
```

is the base case.

Without a proper base case, recursion may continue indefinitely until Python raises:

```text
RecursionError: maximum recursion depth exceeded
```

### Mental model

```text
Is base case reached?
       ↓
     Yes → Stop
       ↓ No
Make recursive call
```

---

# 74. Recursive Case

The **recursive case** is the part where the function calls itself with a smaller or simpler version of the problem.

Example:

```python
def countdown(n):
    if n == 0:          # Base case
        return

    print(n)
    countdown(n - 1)    # Recursive case
```

Here:

```python
countdown(n - 1)
```

moves the problem toward the base case.

### A good recursive function therefore needs:

```text
1. Base case
2. Recursive case
3. Progress toward the base case
```

---

# 75. How Recursion Works

Consider:

```python
```

Execution:

```text
countdown(3)
    print 3
    ↓
countdown(2)
    print 2
    ↓
countdown(1)
    print 1
    ↓
countdown(0)
    stop
```

But there is another important thing happening.

The previous function calls are **waiting** for the recursive call to finish.

Conceptually:

```text
countdown(3)
    waiting for countdown(2)
        waiting for countdown(1)
            waiting for countdown(0)
```

When the base case is reached, the calls can return back upward.

```text
countdown(0) → returns
countdown(1) → returns
countdown(2) → returns
countdown(3) → returns
```

This leads to the **call stack**.

---

# 76. Call Stack

The **call stack** is a stack data structure used by the program to keep track of active function calls.

When a function is called, information about that call is placed on the stack.

When the function finishes, its information is removed.

For recursion:

```text
countdown(3)
countdown(2)
countdown(1)
countdown(0)
```

the stack grows as new calls are made.

Then it shrinks as calls return.

### Visual model

```text
          ┌─────────────┐
          │ countdown(0)│ ← current call
          ├─────────────┤
          │ countdown(1)│
          ├─────────────┤
          │ countdown(2)│
          ├─────────────┤
          │ countdown(3)│
          └─────────────┘
```

After `countdown(0)` returns:

```text
          ┌─────────────┐
          │ countdown(1)│
          ├─────────────┤
          │ countdown(2)│
          ├─────────────┤
          │ countdown(3)│
          └─────────────┘
```

The stack continues to shrink.

---

# 77. Stack Frames

Each function call gets its own **stack frame**.

A stack frame contains information needed for that particular function invocation, such as:

* local variables
* arguments
* execution state
* where execution should continue after the function returns

With recursion, each recursive call creates a **new stack frame**.

Example:

```python
def countdown(n):
    if n == 0:
        return

    print(n)
    countdown(n - 1)
```

Calling:

```python
countdown(3)
```

creates roughly:

```text
Frame 1 → countdown(3)
Frame 2 → countdown(2)
Frame 3 → countdown(1)
Frame 4 → countdown(0)
```

Each frame has its own value of `n`.

This is important:

```text
countdown(3) → n = 3
countdown(2) → n = 2
countdown(1) → n = 1
countdown(0) → n = 0
```

They are separate function calls with separate local state.

---

# 78. Recursion Tracing / Dry Run ⭐

Being able to **trace recursion manually** is more important than memorizing recursive solutions.

Consider factorial:

```python
def factorial(n):
    if n == 0:
        return 1

    return n * factorial(n - 1)
```

Call:

```python
factorial(3)
```

### Step 1

```text
factorial(3)
= 3 * factorial(2)
```

### Step 2

```text
= 3 * (2 * factorial(1))
```

### Step 3

```text
= 3 * (2 * (1 * factorial(0)))
```

### Step 4 — Base case

```text
factorial(0) = 1
```

Now the calls return upward:

```text
factorial(1)
= 1 * 1
= 1
```

```text
factorial(2)
= 2 * 1
= 2
```

```text
factorial(3)
= 3 * 2
= 6
```

Final result:

```text
6
```

### The key idea

Recursion has two phases:

```text
Going down
    ↓
recursive calls

Going back up
    ↓
returning results
```

This "going down and coming back up" model is extremely useful for understanding recursive code.

---

# 79. Recursion vs Iteration

**Iteration** means repeating using loops such as `for` or `while`.

The same countdown can be written iteratively:

### Recursion

```python
def countdown(n):
    if n == 0:
        return

    print(n)
    countdown(n - 1)
```

### Iteration

```python
def countdown(n):
    while n > 0:
        print(n)
        n -= 1
```

Both produce:

```text
3
2
1
```

### Comparison

| Feature            | Recursion                               | Iteration                                    |
| ------------------ | --------------------------------------- | -------------------------------------------- |
| Uses               | Function calls                          | Loops                                        |
| Memory             | Uses call stack                         | Usually less call-stack overhead             |
| Code               | Can be elegant for recursive structures | Often simpler for repetitive tasks           |
| Risk               | `RecursionError` / deep stack           | Usually no recursion-depth issue             |
| Common use         | Trees, divide-and-conquer               | General repetition                           |
| Python performance | Often more overhead                     | Usually more efficient for simple repetition |

### Important

In Python, recursion is **not automatically better** than a loop.

For simple repetition, iteration is often preferable.

---

# 80. Common Recursion Mistakes

## Mistake 1 — Missing Base Case

```python
def count(n):
    print(n)
    count(n - 1)
```

There is no stopping condition.

Eventually:

```text
RecursionError
```

---

## Mistake 2 — Base Case Never Reached

```python
def count(n):
    if n == 0:
        return

    count(n + 1)
```

If you start with:

```python
count(1)
```

the values become:

```text
1
2
3
4
5
...
```

The function moves **away** from the base case.

A recursive function must make progress toward termination.

---

## Mistake 3 — Incorrect Base Case Result

Factorial:

```python
def factorial(n):
    if n == 0:
        return 0     # ❌
```

The correct mathematical base case is:

```python
if n == 0:
    return 1
```

because:

```text
0! = 1
```

---

## Mistake 4 — Forgetting to Return the Recursive Result

Incorrect:

```python
def factorial(n):
    if n == 0:
        return 1

    n * factorial(n - 1)
```

The recursive expression is calculated but not returned.

Correct:

```python
def factorial(n):
    if n == 0:
        return 1

    return n * factorial(n - 1)
```

---

# 81. Recursion in DSA 🟡

Recursion is heavily used in DSA.

You will commonly encounter it in:

* tree traversal
* graph traversal
* binary search
* divide-and-conquer
* backtracking
* sorting algorithms such as merge sort and quicksort

However, **you do not need to focus heavily on DSA recursion right now**.

For your current Python learning, understand:

```text
function calls itself
        ↓
base case
        ↓
recursive case
        ↓
call stack
        ↓
stack frames
        ↓
trace execution
```

That's enough for now.

---

# Examples

## 1. Factorial ⭐

Mathematical definition:

```text
n! = n × (n-1) × (n-2) × ... × 1
```

Recursive implementation:

```python
def factorial(n):
    if n == 0:
        return 1

    return n * factorial(n - 1)


print(factorial(5))
```

Output:

```text
120
```

Flow:

```text
5 × 4 × 3 × 2 × 1
```

---

# 2. Fibonacci 🟡

Fibonacci sequence:

```text
0, 1, 1, 2, 3, 5, 8, 13...
```

A simple recursive implementation:

```python
def fibonacci(n):
    if n <= 1:
        return n

    return fibonacci(n - 1) + fibonacci(n - 2)
```

Example:

```python
print(fibonacci(6))
```

Output:

```text
8
```

### Important warning

This simple recursive Fibonacci implementation is **very inefficient for larger ****`n`** because it repeatedly calculates the same values.

For example:

```text
fibonacci(5)
├── fibonacci(4)
│   ├── fibonacci(3)
│   └── fibonacci(2)
└── fibonacci(3)
```

The same subproblems are calculated multiple times.

You don't need to deeply optimize this now; just understand **why naive recursion can become expensive**.

---

# 3. Sum of Numbers

Calculate:

```text
1 + 2 + 3 + ... + n
```

Recursive version:

```python
def sum_numbers(n):
    if n == 0:
        return 0

    return n + sum_numbers(n - 1)


print(sum_numbers(5))
```

Output:

```text
15
```

Trace:

```text
5 + sum_numbers(4)
    ↓
5 + 4 + sum_numbers(3)
    ↓
5 + 4 + 3 + sum_numbers(2)
    ↓
5 + 4 + 3 + 2 + sum_numbers(1)
    ↓
5 + 4 + 3 + 2 + 1 + sum_numbers(0)
    ↓
15
```

---

# 4. Reverse String

A simple recursive approach:

```python
def reverse_string(s):
    if len(s) <= 1:
        return s

    return reverse_string(s[1:]) + s[0]


print(reverse_string("hello"))
```

Output:

```text
olleh
```

### How?

```text
"hello"
→ reverse("ello") + "h"
→ reverse("llo") + "e" + "h"
→ reverse("lo") + "l" + "e" + "h"
→ reverse("o") + "l" + "l" + "e" + "h"
→ "olleh"
```

This is useful for understanding recursion, but **don't memorize this as the best way to reverse strings in Python**.

Python provides simpler tools:

```python
"hello"[::-1]
```

---

# 5. Binary Search 🟡

Binary search repeatedly divides a **sorted** search space in half.

Conceptually:

```text
[1, 3, 5, 7, 9, 11, 13]
              ↑
            middle
```

If the target is larger than the middle:

```text
search right half
```

If smaller:

```text
search left half
```

A recursive implementation:

```python
def binary_search(arr, target, left, right):
    if left > right:
        return -1

    mid = (left + right) // 2

    if arr[mid] == target:
        return mid

    if target < arr[mid]:
        return binary_search(arr, target, left, mid - 1)

    return binary_search(arr, target, mid + 1, right)
```

Example:

```python
numbers = [1, 3, 5, 7, 9, 11, 13]

index = binary_search(
    numbers,
    9,
    0,
    len(numbers) - 1
)

print(index)
```

Output:

```text
4
```

### Why recursion fits here?

Each call solves a smaller version of the same problem:

```text
Entire array
     ↓
Left/right half
     ↓
Smaller half
     ↓
...
```

You don't need to deeply practice recursive binary search right now. Understand the **divide-the-problem** idea.

---

# 6. Tree Traversal ⭐ for Understanding

Trees are one of the places where recursion feels natural.

Example tree:

```text
        A
       / \
      B   C
     / \
    D   E
```

A simple preorder traversal:

```python
class Node:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None


def preorder(node):
    if node is None:
        return

    print(node.value)
    preorder(node.left)
    preorder(node.right)
```

The important idea is:

```text
Visit current node
      ↓
Traverse left subtree
      ↓
Traverse right subtree
```

Why does recursion work so naturally?

Because a **subtree is itself a smaller tree**.

So the same function can solve:

```text
whole tree
   ↓
left subtree
   ↓
left subtree's subtree
   ↓
...
```

Tree recursion becomes much easier once you understand the idea of **"same problem, smaller input."**

---

# ⭐ Important Points

1. **Recursion** = a function calling itself.
2. A recursive function needs a **base case**.
3. The **recursive case** makes the function call itself.
4. Each recursive call creates a new **stack frame**.
5. Stack frames are managed through the **call stack**.
6. Recursive execution usually has:

   * calls going deeper
   * returns coming back upward
7. Always make progress toward the base case.
8. Recursion can be elegant for naturally recursive problems.
9. In Python, recursion has a recursion-depth limit, so extremely deep recursion can raise `RecursionError`.
10. Many recursive problems can also be solved iteratively.

---

# ⚠️ Common Confusions

### Recursion is NOT the same as a loop

```python
# Recursion
def count(n):
    if n == 0:
        return
    count(n - 1)
```

versus:

```python
# Iteration
for i in range(n, 0, -1):
    ...
```

Both repeat work, but recursion uses **function calls and the call stack**, while iteration uses **loops**.

---

### Base case vs recursive case

```python
def factorial(n):

    if n == 0:              # Base case
        return 1

    return n * factorial(n - 1)  # Recursive case
```

Think:

> **Base case = when to stop.**
> **Recursive case = how to continue.**

---

### Why does the result come back upward?

Because earlier function calls are waiting for their recursive calls to finish.

For:

```python
factorial(3)
```

you can think:

```text
factorial(3)
   waits for
factorial(2)
   waits for
factorial(1)
   waits for
factorial(0)
   ↓
returns 1
   ↓
returns 1 × 1
   ↓
returns 2 × 1
   ↓
returns 3 × 2
```

This is the most important mental model for tracing recursion.

---

# 🎯 Interview Answer

> **What is recursion?**

"Recursion is a technique where a function calls itself to solve a problem by breaking it into smaller versions of the same problem. A recursive function needs a base case to stop the recursion and a recursive case that moves toward that base case."

> **How does recursion work internally?**

"Each recursive function call creates a new stack frame on the call stack. The calls continue until the base case is reached, after which the stack frames return one by one."

> **Recursion vs iteration?**

"Recursion uses repeated function calls and the call stack, while iteration uses loops. Recursion can make some problems such as tree traversal easier to express, but iteration is often more memory-efficient and practical for simple repetition in Python."

---

# 🤖 Backend / GenAI Relevance

Recursion itself is **not a major day-to-day GenAI development concept**.

You'll encounter the underlying idea more often in:

* tree-like data
* nested structures
* parsing
* traversing hierarchical data
* recursive algorithms inside libraries

For example, JSON-like structures can be nested:

```python
data = {
    "user": {
        "profile": {
            "skills": {
                "python": "advanced"
            }
        }
    }
}
```

More importantly, understanding recursion strengthens your ability to reason about **nested and hierarchical structures**, which can be useful in software engineering.

You don't need to turn recursion into a major study phase for your current goal.

---

# 📌 Priority for Your Roadmap

| Topic                  | Priority           |
| ---------------------- | ------------------ |
| What recursion is      | 🔥 Core            |
| Recursive function     | 🔥 Core            |
| Base case              | 🔥 Core            |
| Recursive case         | 🔥 Core            |
| How recursion works    | 🔥 Core            |
| Call stack             | 🔥 Core            |
| Stack frames           | 🟡 Know & Move On  |
| Recursion tracing      | 🔥 Core            |
| Recursion vs iteration | 🟡 Know & Move On  |
| Common mistakes        | 🟡 Know & Move On  |
| Recursion in DSA       | ⚪ Optional for Now |
| Factorial              | 🔥 Core example    |
| Fibonacci              | 🟡 Know & Move On  |
| Sum of numbers         | 🟡 Know & Move On  |
| Reverse string         | 🟡 Know & Move On  |
| Binary search          | ⚪ Optional for Now |
| Tree traversal         | ⚪ Optional for Now |

### 🎯 Your target

You should be able to look at:

```python
def factorial(n):
    if n == 0:
        return 1
    return n * factorial(n - 1)
```

and immediately explain:

```text
factorial()
   ↓
base case → n == 0
   ↓
recursive case → factorial(n - 1)
   ↓
calls build up on stack
   ↓
base case reached
   ↓
results return back upward
```

**That's enough recursion knowledge for now.** Don't let recursion pull you back into a DSA-heavy study path. The higher-value Python topics after this are **comprehensions → modules/packages → exceptions → iterators/generators → decorators → context managers → type hints/dataclasses → async/await**, which are much more relevant to your eventual backend/GenAI work.
