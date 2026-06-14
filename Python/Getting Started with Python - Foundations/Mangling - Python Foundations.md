
**Question:** What is name mangling in Python, and how does it work?

**Answer:** Name mangling is Python's way of making an attribute _harder_ to accidentally override in a subclass. When you prefix an attribute with **two leading underscores** (and at most one trailing underscore), Python silently rewrites the name at compile time.

```
[Definition → Decomposition → Recomposition]
```

```python
class Foo:
    def __init__(self):
        self.__secret = 42

f = Foo()
print(f._Foo__secret)  # 42
print(f.__secret)      # AttributeError
```

**Decomposition**

- `__secret` — an attribute name with two leading underscores. This triggers mangling. Python transforms it at **compile time** (not runtime) into `_ClassName__secret`.
- `_Foo__` — the mangled prefix. Constructed as `_` + the **class name** + the original name. Python strips leading underscores from the class name before prepending.
- `f.__secret` — fails because the attribute literally does not exist under that name on the object. Python stored it as `_Foo__secret`, not `__secret`.
- `f._Foo__secret` — works because you're using the actual stored name.

**Recomposition**

- `__secret` → `_Foo__secret` — Python rewrites the name during class compilation, so any access through the original `__secret` name will miss unless you're inside the class body itself (where Python applies the same rewrite automatically).
- The full picture: **inside** the class, `self.__secret` works fine — Python rewrites both the definition and the access. **Outside** the class, you must use the mangled name explicitly, which makes accidental collision from subclasses unlikely.

```python
class Bar(Foo):
    def __init__(self):
        super().__init__()
        self.__secret = 99  # becomes _Bar__secret — doesn't clobber Foo's _Foo__secret

b = Bar()
print(b._Foo__secret)  # 42 — Foo's copy, untouched
print(b._Bar__secret)  # 99 — Bar's own copy
```

**Why "mangled"?** The word comes from the general CS sense of _mangling_ — transforming a name into a different, encoded form. You see it in **C++ name mangling** too, where the compiler encodes type signatures into symbol names. Same idea: the name you write is not the name that gets stored.

**One sharp edge:** this is **not** true privacy. It's purely a namespacing trick to reduce accidental collision in inheritance chains. Anyone can access `_Foo__secret` directly. If you want "private by convention," use a **single** leading underscore (`_secret`) — that's the idiomatic signal in Python.

**Additional thoughts:** Name mangling sits in a hierarchy of Python's naming conventions worth knowing cold:

|Convention|Meaning|
|---|---|
|`name`|Public|
|`_name`|Internal / don't touch (convention only)|
|`__name`|Mangled — subclass collision protection|
|`__name__`|Dunder — reserved for Python itself|

📖 [Python Data Model — Private name mangling](https://docs.python.org/3/reference/expressions.html#atom-identifiers) 📖 [PEP 8 — Naming conventions](https://peps.python.org/pep-0008/#naming-conventions)