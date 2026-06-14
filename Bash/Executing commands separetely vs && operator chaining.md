

The `&&` operator is a logical AND that creates a dependency between [[Command -- Bash#Definition|commands]]. 

**Answer:**

Yes, there's a critical difference in **error handling**.

**Separate lines:**

```bash
apt update
apt install unifi -y
```

Line-by-line breakdown:

- `apt update` - runs, could fail or succeed
- `apt install unifi -y` - **runs regardless** of whether update succeeded
    - `apt` = package manager [[Command -- Bash#Definition|command]]
    - `install unifi` = package to install
    - `-y` = auto-confirm prompts (yes to all)

**Chained with `&&`:**

```bash
apt update && apt install unifi -y
```

Decomposition:

- `apt update` - runs first
- `&&` = logical AND operator (conditional execution)
    - Only executes right side if left side **exits with status 0** (success)
- `apt install unifi -y` - **only runs if update succeeded**

**Why `&&` matters:** If your package lists fail to update (network issue, repo down, whatever), you don't want to proceed with installing from stale/broken metadata. `&&` prevents that failure cascade.

**[[Command -- Bash#Definition|command]] context:**

- `&&` = "AND operator" - borrowed from boolean logic, just like in programming languages

**Additional thoughts:**

This is part of bash's **exit status** system. Every [[Command -- Bash#Definition|command]] returns a status code (0 = success, non-zero = failure). You have three main chaining operators:

- `&&` = run next [[Command -- Bash#Definition|command]] only if previous **succeeded** (AND)
- `||` = run next [[Command -- Bash#Definition|command]] only if previous **failed** (OR)
- `;` = run next [[Command -- Bash#Definition|command]] **regardless** (same as separate lines)

Smart sysadmins chain critical operations with `&&` because **fail-fast is better than fail-dirty**. If step 1 breaks, you want the whole operation to stop, not continue and potentially corrupt state.

For deeper reading on exit statuses and control operators:

- GNU Bash manual on exit status: https://www.gnu.org/software/bash/manual/html_node/Exit-Status.html
- Bash reference on lists ([[Command -- Bash#Definition|command]] chaining): https://www.gnu.org/software/bash/manual/html_node/Lists.html
- https://claude.ai/share/b24062ae-c4e8-4f05-8b8b-ef2a77d297c6
