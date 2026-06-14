
package: functools

usage: 

> [!bash] Python`snippet`
> ```python
> from functools import reduce
> 
> reduce(lambda acc, x: acc + x, [1, 2, 3, 4], 0)
> ```
> 
> > [!example]- **Decomposition**
> > - `reduce` - For an iterable, accumulates the value of each element `x` to an initial value `acc`. *Note: The function is usually a `lambda` function.*
> > - `lambda acc, x:` - A lambda is an anonymous function. This one has two parameters.  
> > 	- `acc` - Accumulator, the accumulated result from all previous iterations) 
> > 	-  `x` -  Current element in the list.
> > - `acc + x` - **Key Step** -> For each element '`x`', `x` is added to the current value of `acc`. The return value of `acc + x` becomes the new value of `acc` for the next iteration.
> > 	- Each element in `[1, 2, 3, 4]` is added to the current value of acc.
> > 	- The result is returned to the next `reduce` call as the current value of `acc`. *Note: You **must** return a value from your function call, otherwise reduce will return a `NoneType` error.* 
> > 	- If no value is given for `acc` it defaults to the value of iterable[0], in this case `1`
> > > [!example]- *Example*
> > > [*>1*, 2, 3, 4] -> *x = 1* -> *lambda (acc = 0, x = 1)*  ->   *acc = 1*
> > - `[1, 2, 3, 4]` - the sequence being folded
> > - `0` - Optional initial value for the accumulator. If not present defaults to the first element in the sequence. *Tip Useful if the sequence might be empty `[]`.*
> 
> > [!tip]-  **Recomposition**
> > - `lambda acc, x: acc + x` - Lambda function that adds two things together
> > - `lambda acc, x: acc + x`, `[1, 2, 3,4]`, `0` - arguments to reduce. A lambda, an iterable (list in this case) and an optional starting value for `acc. 
> > - `reduce(lambda acc, x: acc + x, [1, 2, 3, 4]), 0` — starting from the left, repeatedly add the next number to the running total, producing `10`
> 
> **TLDR;**
> - The most common use of `reduce` is used to add list elements to an accumulator.
> - Reduce can be hard to understand, and in my opinon that's partially down to its syntax with its seemingly arbitrary argument order and the way it obscures the starting accumulator value. 
> - Key things to remember
> 	- Reduce *iterates*. It calls the lambda on each element value and returns it. 
> 	- The **return value** of the lamda is the **initial** value of `acc` on the next iteration. 
> 	- The *first* argument of the lamda is *always* the accumulator. 
> 	- The *second* argument of the lamda is *always* the value of the current element. 
> 	- If the third argument isn't declared, `acc` defaults to iterable[0]. 
> **Gotchas**
> - Using a method that has a return value of `None` in your lamda function will give a `Nonetype` error. This is because `reduce` requires a return value to feed back into itself as the new accumulator value on the next loop. *Example: `reduce(lamda acc, x: acc.append(x), [1, 2, 3], []` -> `AttributeError: 'NoneType' object has no attribute 'append'`*

#### Reference Implementation

Decompose the pure-Python equivalent of `reduce` from the docs.

**Note**: A **reference implementation** is a plain-Python rewrite of a built-in that reveals its mechanics. 

---
**Breakdown**
Three distinct concepts are doing work here. Break them apart first.
The specific techniques here touch on **sentinel values**, **positional-only parameters**, and **iterator protocol**.

---

### Concept 1 — The sentinel
> [!py] py
> 
> ```python
> initial_missing = object()
> ```
> 
> **Decomposition**
>  - `initial_missing` — A module-level name bound to that instance. Acts as a **sentinel**: a private signal value that can never be accidentally passed in by a caller, because no one else has a reference to this exact object
> - `object()` — instantiates a bare, anonymous object. No methods, no value, no meaning — just a unique identity in memory
> 
> **Why not just use `None`?** `None` is a valid thing a user might want to pass as an initial value. `object()` is unforgeable — `initial_missing` is the only name pointing to it, so `x is initial_missing` can only ever be `True` if you're holding the exact sentinel.

---

### Concept 2 — Positional-only parameters (`/`)

```python
def reduce(function, iterable, /, initial=initial_missing):
```

**Decomposition**

- `function` — first positional-only param: the folding function
- `iterable` — second positional-only param: the sequence to fold
- `/` — **positional-only marker**: everything to the left cannot be passed as a keyword argument. `reduce(function=len, iterable=[])` would raise a `TypeError`. Named after the mathematical convention for dividing parameter modes.
- `initial=initial_missing` — keyword-capable param with a default; the sentinel stands in for "not provided"

---

### Concept 3 — Iterator protocol

```python
it = iter(iterable)
```

**Decomposition**
- `it` — the iterator object bound to that cursor

- `iter()` — builtin that calls `__iter__` on the object, returning an **iterator**: a stateful cursor that moves forward through the sequence one item at a time, never backward
- `iterable` - A 'Recipe' that describes what to loop over, no cursor
```python
value = next(it)
```

**Decomposition**

- `next()` — builtin that calls `__next__` on the iterator, advancing the cursor and returning the current item
- First call consumes index 0, leaving the iterator pointing at index 1

**Why does this matter?** The `for element in it:` loop that follows starts from wherever the cursor currently sits. If no initializer was given, `next(it)` consumed the first element as `value`, so the loop processes the rest. If an initializer was given, the cursor is untouched and the loop processes everything.

---

### Full recomposition

```python
initial_missing = object()

def reduce(function, iterable, /, initial=initial_missing):
    it = iter(iterable)
    if initial is initial_missing:
        value = next(it)
    else:
        value = initial
    for element in it:
        value = function(value, element)
    return value
```

**Decomposition**

- `initial_missing = object()` — forge a sentinel no caller can replicate
- `def reduce(function, iterable, /, initial=initial_missing)` — declare the function; `function` and `iterable` are positional-only; `initial` defaults to the sentinel
- `it = iter(iterable)` — wrap the iterable in a stateful cursor
- `if initial is initial_missing:` — `is` checks identity, not equality. The only way this is `True` is if the caller passed nothing for `initial`
- `value = next(it)` — consume the first element as the seed, advancing the cursor
- `value = initial` — use the caller's provided seed instead, cursor untouched
- `for element in it:` — iterate from wherever the cursor currently sits
- `value = function(value, element)` — apply the fold: new accumulator = function(old accumulator, current element)
- `return value` — return the final accumulated result

**Recomposition**

- `iter()` + `next(it)` — consuming the first element as seed and leaving the cursor at index 1 is what makes the "no initializer" case work without slicing the list
- The whole function — wrap the iterable in a cursor, seed the accumulator either from the caller or from the first element, then walk the remaining elements applying `function(acc, element)` at each step, returning the final value

---

**Additional thoughts:**

The `is` operator instead of `==` for the sentinel check is load-bearing. `==` tests _equality_ (can be overridden via `__eq__`); `is` tests _identity_ (same object in memory, no override possible). A malicious or broken `__eq__` can't fool `is`. This is standard sentinel idiom in CPython's own source.

The `/` positional-only syntax was added in **Python 3.8** ([PEP 570](https://peps.python.org/pep-0570/)). Before that, positional-only params existed in C builtins but couldn't be expressed in pure Python.

Relevant references:

- [Iterator protocol — `__iter__` and `__next__`](https://docs.python.org/3/library/stdtypes.html#iterator-types)
- [PEP 570 — Positional-only parameters](https://peps.python.org/pep-0570/)
- [`is` vs `==`](https://docs.python.org/3/reference/expressions.html#is)