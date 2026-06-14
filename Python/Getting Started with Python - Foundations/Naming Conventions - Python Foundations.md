# Python Naming Conventions

## Variables & Functions

|Pattern|Convention|Example|
|---|---|---|
|Variables|`snake_case`|`user_name`|
|Functions|`snake_case`|`get_user()`|
|Constants|`UPPER_SNAKE_CASE`|`MAX_RETRIES`|

## Classes & Types

|Pattern|Convention|Example|
|---|---|---|
|Classes|`PascalCase`|`UserAccount`|
|Type aliases|`PascalCase`|`UserId = int`|
|Exceptions|`PascalCase` + `Error` suffix|`ValidationError`|

## Modules & Packages

|Pattern|Convention|Example|
|---|---|---|
|Modules (`.py` files)|`snake_case`|`user_utils.py`|
|Packages (directories)|`snake_case`, short|`mypackage/`|

## Underscores — What They Signal

|Pattern|Meaning|Example|
|---|---|---|
|`_single_leading`|Internal use / "private by convention"|`_helper()`|
|`__double_leading`|Name-mangled — class-private|`__secret`|
|`__dunder__`|Python-reserved magic method|`__init__`, `__str__`|
|`_` (standalone)|Throwaway / ignored value|`for _ in range(3)`|

## Boolean Variables

Prefix with `is_`, `has_`, `can_`, `should_`:

```python
is_active = True
has_permission = False
can_edit = True
```

## Iterators & Collections

Name collections as plurals; loop variables as their singular:

```python
users = [...]
for user in users: ...
```

## Arguments

|Pattern|Convention|Example|
|---|---|---|
|Regular args|`snake_case`|`def greet(user_name)`|
|`*args`|`snake_case` (usually `args`)|`def fn(*args)`|
|`**kwargs`|`snake_case` (usually `kwargs`)|`def fn(**kwargs)`|

## Key Rules

- **Avoid** single-letter names except `i`, `j`, `k` (loop counters), `x`, `y` (math/coords), `e` (exceptions), `f` (files)
- **Avoid** shadowing builtins: don't name things `list`, `id`, `type`, `input`, `filter`
- If a name conflicts with a builtin, add a trailing underscore: `type_`, `id_`, `list_`
- Be descriptive over clever

## Reference

- [PEP 8 — Style Guide for Python Code](https://peps.python.org/pep-0008/#naming-conventions)
- [PEP 423 — Naming conventions for packages](https://peps.python.org/pep-0423/)