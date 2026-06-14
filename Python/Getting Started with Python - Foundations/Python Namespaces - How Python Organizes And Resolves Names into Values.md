
https://realpython.com/python-scope-legb-rule/ python namespace resolution order - Local -> Enclosing -> Global -> Built-in

**Question** What are namespaces in Python, how do they work, and why do they exist?

**Answer:** A namespace is just a **mapping of names to objects** — Python literally implements it as a dictionary. The name `x` doesn't exist in some void; it lives inside a specific namespace. When you write `x = 5`, Python isn't just storing `5` — it's storing the _binding_ `"x" → 5` somewhere specific.

```
[Namespace as dict → Name lookup → LEGB resolution]
```

```python
x = 10  # global namespace: {"x": 10, ...}

def foo():
    x = 99  # local namespace: {"x": 99}
    print(x) # looks here first → 99

foo()
print(x) # global namespace → 10
```

**Decomposition**

- `x = 10` — binds the name `"x"` to the object `10` in the **global namespace** (the module-level dict)
- `def foo():` — creates a new **local namespace** when called; each function call gets its own fresh dict
- `x = 99` inside `foo` — binds `"x"` to `99` in `foo`'s _local_ namespace — completely separate from global `x`
- `print(x)` inside `foo` — Python looks up `"x"` starting in local namespace, finds it immediately → `99`
- `print(x)` outside `foo` — local namespace is gone (call ended); looks in global namespace → `10`

**Recomposition**

- `x = 10` + `def foo(): x = 99` — two separate namespaces hold two separate bindings for the same name `"x"`, with zero conflict
- The whole thing demonstrates that **names are not values** — they're labels pinned to a specific namespace, and the same label in different namespaces points to completely independent objects

---

**The four namespaces Python actually uses — LEGB:**

|Scope|What it is|
|---|---|
|**L**ocal|Inside the current function|
|**E**nclosing|Any wrapping function (closures)|
|**G**lobal|Module-level|
|**B**uilt-in|Python's own builtins (`len`, `print`, etc.)|
 
When you reference a name, Python walks that chain in order and uses the first match. This is **name resolution**.

```python
len = "oops"         # shadows built-in 'len' in global namespace
len([1, 2, 3])       # TypeError — 'len' resolves to your string now
```

You can **inspect namespaces directly** because they're just dicts:

```python
print(globals())   # the global namespace as a dict
print(locals())    # the local namespace as a dict (inside a function)
```

---

**Additional thoughts:** The famous line from the Zen of Python — _"Namespaces are one honking great idea"_ — exists because without them, every name in every library you import would collide. When you do `import os`, `os` itself is a name in your namespace, and `os.path` accesses the `path` name _inside the `os` module's namespace_, using dot notation as a namespace separator. That's also why `from os import path` can be dangerous at scale — you're pulling a name out of its namespace into yours, where it can collide.

This mental model (name → namespace → LEGB lookup) explains `global`, `nonlocal`, `del`, import aliasing, and why monkey-patching works. It's load-bearing.

- [Python Execution Model — Naming and Binding](https://docs.python.org/3/reference/executionmodel.html#naming-and-binding)
- [Built-in functions: `globals()`, `locals()`](https://docs.python.org/3/library/functions.html#globals)