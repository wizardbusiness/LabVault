- **JSON ≠ object** — serialization lies by omission
- **`hasattr` checks existence, not usability** — `None` is a valid value
- **`getattr` with a falsy default collapses both checks into one guard**
```python
# Evaluates false if "children" attribute has a value of None
if getattr(token, "children", None):
	# do something
```