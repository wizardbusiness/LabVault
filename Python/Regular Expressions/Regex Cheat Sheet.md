# Regex cheat sheet (Python `re`)

> Always use raw strings: `r'\d+'` not `'\\d+'`

---

## Anchors

**When to use:** Use anchors when care about _where_ in the string a match sits, not just whether it exists. 

| Token | Matches              | Example pattern | Input           | Match           | When to use                                                                                                                    |
| ----- | -------------------- | --------------- | --------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| `^`   | Start of string/line | `^cat`          | `catfish`       | `cat`fish       | You care about the start of a string exactly matching a pattern with no junk in front of it.                                   |
| `$`   | End of string/line   | `dog$`          | `hotdog`        | hot`dog`        | You care about the end of a string exactly matching a pattern with no junk after it.                                           |
| `\b`  | Word boundary        | `\bword\b`      | `password word` | password `word` | When you want to only match whole words. Without`\b`, `dog` matches inside `hotdog`.                                           |
| `\B`  | Not a word boundary  | `\Bcat\B`       | `concatenate`   | con`cat`enate   | When you want to only match parts of words, no whole words. With`\B`, `dog` matches inside `hotdog` but not with `wiener dog`. |

---

## Character classes

**When to use:** You need to match _a type of character_ rather than a specific one. Prefer shorthand classes (`\d`, `\w`, `\s`) for common types. Use `[...]` when you need a custom set — especially when alternating single characters (`[-—]` beats `-|—`). Use `[^...]` to reject specific characters.

| Token    | Matches                         | Example pattern | Input         | Match             |
| -------- | ------------------------------- | --------------- | ------------- | ----------------- |
| `.`      | Any char except newline         | `c.t`           | `cat cot c9t` | `cat` `cot` `c9t` |
| `\d`     | Digit `[0-9]`                   | `\d+`           | `abc 42 x9`   | `42`, `9`         |
| `\D`     | Non-digit                       | `\D+`           | `abc 42`      | `abc`             |
| `\w`     | Word char `[a-zA-Z0-9_]`        | `\w+`           | `hi! there`   | `hi`, `there`     |
| `\W`     | Non-word char                   | `\W+`           | `hi! there`   | `!`               |
| `\s`     | Whitespace (space, `\t`, `\n`…) | `\s+`           | `a b\tc`      | , `\t`            |
| `\S`     | Non-whitespace                  | `\S+`           | `a b c`       | `a`, `b`, `c`     |
| `[abc]`  | Any char in set                 | `[aeiou]`       | `regex`       | `e`, `e`          |
| `[^abc]` | Any char NOT in set             | `[^aeiou]+`     | `regex`       | `r`, `g`, `x`     |
| `[a-z]`  | Any char in range               | `[a-f]+`        | `abcghij`     | `abc`             |
|          |                                 |                 |               |                   |

```python
# [...]  beats | for single-character alternation
r'\s*[-—]\s*'    # match hyphen or em dash with optional spaces
r'[.!?]'         # sentence-ending punctuation — no escaping needed inside []
```

> `.` inside `[...]` is a literal dot, not "any character." Most special characters lose their meaning inside a character class.

---

## Quantifiers

**When to use:** You need to match a variable number of something. Use `+` when at least one is required (e.g. a number must have digits). Use `*` when zero is valid. Use `{n,m}` when you have a known range (phone numbers, zip codes, dates). Switch to lazy (`+?`, `*?`) when matching delimited content like tags or quoted strings — greedy will overshoot.

Greedy by default — match as much as possible, then backtrack. Append `?` to make lazy.

|Token|Matches|Example pattern|Input|Match|
|---|---|---|---|---|
|`*`|0 or more (greedy)|`ca*t`|`ct cat caat`|`ct`, `cat`, `caat`|
|`+`|1 or more (greedy)|`ca+t`|`ct cat caat`|`cat`, `caat`|
|`?`|0 or 1 (optional)|`colou?r`|`color colour`|`color`, `colour`|
|`{n}`|Exactly n times|`\d{3}`|`12 123 1234`|`123`, `123`|
|`{n,}`|n or more times|`\d{2,}`|`1 12 123`|`12`, `123`|
|`{n,m}`|Between n and m times|`\d{2,3}`|`1 12 1234`|`12`, `123`|
|`*?`|0 or more (lazy)|`<.*?>`|`<b>hi</b>`|`<b>`, `</b>`|
|`+?`|1 or more (lazy)|`a+?`|`aaa`|`a`|

```python
re.findall('<.*>',  '<b>hi</b>')   # → ['<b>hi</b>']  greedy: overshoots
re.findall('<.*?>', '<b>hi</b>')   # → ['<b>', '</b>'] lazy: stops at first >
```

---

## Groups

**When to use:** You need to extract a specific part of the match (capturing group), or apply a quantifier to a sequence rather than a single character (non-capturing group). Use named groups (`(?P<name>...)`) when you have more than two groups — `match.group('year')` beats `match.group(3)`. Use `|` for multi-character alternation; use `[...]` for single-character alternation.


|Token|Matches|Example|
|---|---|---|
|`(abc)`|Capturing group|`(\d{4})` on `2024-01` → `match.group(1)` = `2024`|
|`(?:abc)`|Non-capturing group|`(?:foo)+` on `foofoo` → matches but doesn't capture|
|`(?P<name>…)`|Named capturing group|`(?P<yr>\d{4})` → `match.group('yr')` = `2024`|
|`\1`|Backreference to group 1|`(\w+) \1` on `the the` → `the the`|
|`a\|b`|Alternation (a OR b)|`cat\|dog` on `I have a cat` → `cat`|

```python
# Factor shared tokens out of alternation — don't repeat yourself
r'\s*(-|—)\s*'   # fine
r'\s*[-—]\s*'    # better — single chars → use a class

# Named groups pay off when there are many
m = re.search(r'(?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})', '2024-01-15')
m.group('month')   # → '01'
```

> **Python-specific:** named groups use `(?P<name>…)`. Most other languages use `(?<name>…)`.

---
## Lookaround

**When to use:** You want to match something only when it's preceded or followed by something else, but you don't want that context included in the match. Common use cases: extract numbers after a currency symbol, match words only at certain positions, or split on a delimiter without consuming it. If you find yourself capturing something just to throw it away, a lookaround is probably the cleaner tool.

Zero-width assertions — peek without consuming characters.

|Token|Matches|Example pattern|Input|Match|
|---|---|---|---|---|
|`(?=…)`|Lookahead (followed by)|`\d+(?= hrs)`|`3 hrs 5 min`|`3`|
|`(?!…)`|Negative lookahead|`\d+(?! hrs)`|`3 hrs 5 min`|`5`|
|`(?<=…)`|Lookbehind (preceded by)|`(?<=\$)\d+`|`$100 €200`|`100`|
|`(?<!…)`|Negative lookbehind|`(?<!\$)\d+`|`$100 €200`|`200`|

```python
# Extract price digits without capturing the $ sign
re.findall(r'(?<=\$)\d+', 'Total: $42 or €99')   # → ['42']
```

---

## Flags (`re` module)

| Flag   | Effect                                 | Usage                                           | When To Use                                                                               |
| ------ | -------------------------------------- | ----------------------------------------------- | ----------------------------------------------------------------------------------------- |
| `re.I` | Case-insensitive                       | `re.findall('cat', 'CAT', re.I)` → `['CAT']`    | whenever input casing is inconsistent (user input, log files)                             |
| `re.M` | `^`/`$` match each line                | `re.findall('^x', 'x\nx', re.M)` → `['x', 'x']` | When processing multi-line strings and `^`/`$` need to anchor to each line                |
| `re.S` | `.` matches newline too                | `re.search('.+', 'a\nb', re.S)` → `'a\nb'`      | when your pattern needs to span line breaks                                               |
| `re.X` | Allow whitespace & comments in pattern | `\d{4} # year` same as `\d{4}`                  | for any complex pattern — lets you add whitespace and comments so the pattern is readable |
|        |                                        |                                                 |                                                                                           |
|        |                                        |                                                 |                                                                                           |
> [!example] `re.x` example
> 
> ```python
> # re.X makes complex patterns maintainable
> pattern = re.compile(r"""
>     (?P<year>  \d{4}) -   # four-digit year
>     (?P<month> \d{2}) -   # two-digit month
>     (?P<day>   \d{2})     # two-digit day
> """, re.X)
> ```
> 

> [!tip] ***Advanced Tip***
> Flags can be combined: `re.I | re.M`

---

## Key functions

**When to use which:**

|Function|Use when…|
|---|---|
|`re.search()`|You want to know if a pattern exists, or find the first match|
|`re.match()`|You only care about a match at the very start of the string|
|`re.fullmatch()`|The entire string must match the pattern (validation)|
|`re.findall()`|You want all matches as a list of strings|
|`re.finditer()`|You want all matches and need position info or groups|
|`re.sub()`|You want to replace matches|
|`re.split()`|You want to split on a pattern, not a fixed string|
|`re.compile()`|You're using the same pattern more than once|

```python
import re

re.search(pattern, string)      # first match anywhere → Match object or None
re.match(pattern, string)       # match at start only → Match object or None
re.fullmatch(pattern, string)   # entire string must match → Match object or None
re.findall(pattern, string)     # all matches → list of strings
re.finditer(pattern, string)    # all matches → iterator of Match objects
re.sub(pattern, repl, string)   # replace matches → new string
re.split(pattern, string)       # split on matches → list
re.compile(pattern, flags)      # precompile for reuse → Pattern object
```

> `re.search()` returns a Match object (truthy) or `None` (falsy) — use it directly as a boolean: `if re.search(r'\d+', text):`

---

## Compile for reuse

**When to use:** Any time the same pattern appears more than once — in a loop, a function called repeatedly, or a module that processes many strings. Compiling caches the internal automaton so Python doesn't re-parse the pattern on every call.

```python
pattern = re.compile(r'\b\d{4}\b', re.I)
pattern.findall("Born in 1984, moved in 2010")
# → ['1984', '2010']
```

---

## Common gotchas

**Greedy vs. lazy** — `.*` matches from the first `<` to the _last_ `>`. Use `.*?` to stop at the first `>`.

```python
re.findall('<.*>',  '<b>hi</b>')   # → ['<b>hi</b>']  greedy
re.findall('<.*?>', '<b>hi</b>')   # → ['<b>', '</b>'] lazy
```

**Raw strings** — without `r''`, every backslash needs doubling:

```python
re.search('\\d+', '42')   # works but painful
re.search(r'\d+', '42')   # always do this
```

**`re.match` vs `re.search`** — `match` only checks at position 0; `search` scans the whole string.

```python
re.match(r'\d+', 'abc 42')    # → None
re.search(r'\d+', 'abc 42')   # → <Match '42'>
```

**`|` vs `[...]`** — use `|` for multi-character alternation, `[...]` for single characters.

```python
r'cat|dog'       # alternation of words — use |
r'[-—]'          # alternation of single chars — use []
r'\s*[-—]\s*'    # factor shared tokens outside the alternation
```