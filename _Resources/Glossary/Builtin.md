---
term: builtin
category:
  - Linux
  - Shell
  - Bash
  - Command Line
related:
  - Shell
  - Bash
  - Command
  - PATH
  - exec
  - fork
  - subshell
  - external command
  - type
  - which
---

##### Definition

**`builtin`**

**A command that is compiled directly into the shell itself rather than existing as a separate executable file on disk. Runs inside the current shell process without spawning a child process, giving it the ability to directly read and modify the shell's own state (variables, working directory, options).**

> Use `builtin <command>` to explicitly invoke the shell's built-in version of a command, bypassing any function or alias of the same name. Use `type <command>` to check whether a command resolves as a builtin, function, alias, or external executable.

###### **Synonyms**

- Shell builtin

