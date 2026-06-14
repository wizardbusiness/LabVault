**Feedback:** You're describing **operator overloading** — specifically, Python's **dunder method dispatch protocol**.

**Rephrase:** How does Python decide what to do when you use an operator like `+` or `==` on an object?

---

**Answer:** When you write `a + b`, Python doesn't hardcode what `+` means — it _dispatches_ (routes) the operation to a method. Every operator maps to a dunder method. Python looks it up, finds the method, and calls it.

```
[a + b → dispatch → a.__add__(b)]
```

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

v1 = Vector(1, 2)
v2 = Vector(3, 4)
v3 = v1 + v2  # dispatches to v1.__add__(v2)
```

**Decomposition**

- `v1 + v2` — syntactic sugar; the `+` operator is a token Python needs to resolve
- `__add__` — a dunder ("double underscore") method; Python's name for the method that handles `+` on the _left_ operand
- `self` — the left operand (`v1`)
- `other` — the right operand (`v2`)
- `return Vector(...)` — produces a new object; operators typically don't mutate, they return

**Recomposition**

- `v1 + v2` → Python sees `+`, looks up `type(v1).__add__`, calls it with `v1` and `v2` as arguments
- The full dispatch chain: `v1 + v2` → `type(v1).__add__(v1, v2)` → a new `Vector(4, 6)`

---

**The reflected operator:** If `a.__add__(b)` returns `NotImplemented` (not `None` — a specific singleton), Python doesn't give up. It tries `b.__radd__(a)` — the _reflected_ version on the _right_ operand. This is how `3 + my_vector` can still work even though `int` knows nothing about `Vector`.

```python
def __radd__(self, other):
    return self.__add__(other)  # addition is commutative, so just flip it
```

**The full dispatch priority:**

1. `type(a).__add__(a, b)`
2. If that returns `NotImplemented` → `type(b).__radd__(b, a)`
3. If that also returns `NotImplemented` → `TypeError` is raised

**Every operator has a dunder:**

|Operator|Dunder|Reflected|
|---|---|---|
|`+`|`__add__`|`__radd__`|
|`==`|`__eq__`|—|
|`<`|`__lt__`|`__gt__`|
|`[]`|`__getitem__`|—|
|`()`|`__call__`|—|

---

**Additional thoughts:** This is the **Data Model** — Python's core protocol that makes everything consistent. The same dispatch mechanism handles comparisons, indexing, iteration, context managers (`with`), even function calls. Once you see it, you realize Python syntax is almost entirely method calls in disguise.

- [Python Data Model — official docs](https://docs.python.org/3/reference/datamodel.html)
- [Operator dispatch and `NotImplemented`](https://docs.python.org/3/reference/datamodel.html#emulating-numeric-types)