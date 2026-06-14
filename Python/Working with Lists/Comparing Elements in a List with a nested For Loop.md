
```python
items = ['a', 'b', 'c']

for i in range(len(items)):
    for j in range(i + 1, len(items)):
        print(items[i], items[j])

# ab, ac, bc
```

**Decomposition**

- `range(len(items))` — produces indices `0, 1, 2` for the outer loop
- `i` — the index of the current "left" element
- `range(i + 1, len(items))` — starts the inner range _one ahead of_ `i`, so `j` is always greater than `i`
- `j` — the index of the current "right" element; always ahead of `i`, never equal to it
- `items[i], items[j]` — element lookup by index for both sides of the comparison

**Recomposition**

- `range(i + 1, ...)` — by starting `j` at `i + 1`, you guarantee two things at once: `j != i` (no self-comparison) and `j > i` (no revisiting pairs you've already seen)
- The whole thing: for every element, compare it against every element that comes _after_ it in the list — visiting each unique pair exactly once
---
**Additional thoughts:** The pattern `range(i + 1, len(items))` is the standard way to iterate over the **upper triangle** of an implicit N×N comparison matrix — skipping the diagonal (self) and the lower triangle (duplicates). If you find yourself needing this frequently, `itertools.combinations(items, 2)` does exactly this and is more expressive:

```python
from itertools import combinations
for a, b in combinations(items, 2):
    print(a, b)
```

Prefer `combinations` in production code; prefer the manual nested loop when you need the indices themselves.

📎 [`itertools.combinations` — Python docs](https://docs.python.org/3/library/itertools.html#itertools.combinations)