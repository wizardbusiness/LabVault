Comparing to JS because that's my background

https://claude.ai/chat/e643388c-ff5d-465a-8a27-687c90dc172a
https://realpython.com/python-scope-legb-rule/

**Feedback:** This is lexical scoping differences between Python and JavaScript.

**Rephrase:** Why can Python access a variable declared inside a `with` block from the outer scope, when JavaScript would block that?

**Answer:** Exactly — and the key correction to your framing: you're _not_ crossing from local to global scope. You're staying in the _same_ scope the whole time. The `with` block isn't a scope boundary at all — it's just indentation for readability. The variable was always global (or whatever scope you're in).

The JavaScript equivalent that's tripping you up:

```javascript
// JS — block creates scope with let/const
{
  let test = "hello";
}
console.log(test); // ReferenceError
```

```python
# Python — no new scope created
with open(TEST) as f:
    test = f.read()
print(test)  # works fine — same scope throughout
```

**The mental model shift:**

In Python, only these constructs create a new scope:

- `def` — functions
- `class` — classes
- comprehensions (`[x for x in ...]`)

That's basically it. `with`, `for`, `if`, `try` — all just indented blocks. No scope wall.

So when you write `test = f.read()` inside a `with` block at module level, you're writing a global variable. The indentation is grouping logic, not declaring a scope boundary.

---

**Additional thoughts:** JavaScript's `var` actually behaves more like Python here — it leaks out of blocks (just not out of functions). `let`/`const` were added precisely to fix that footgun with block scoping. Python made the opposite bet: keep it simple, fewer scope rules. Both are deliberate design choices, not oversights.

- [Python Scoping — LEGB](https://realpython.com/python-scope-legb-rule/)
- [MDN — JS block scoping](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/block#block_scoping_rules_with_let_const_class_and_function_declaration)