---
term: import
category:
  - Python Programming
  - Module System
  - Code Organization
related:
  - module
  - namespace
  - package
  - from statement
  - sys.path
  - library
  - dependency
  - scope
---
##### Definition

**`import`**

**A Python statement that loads code from another file or module into your current program so you can use its functions, classes, and variables.**


> [!detail]- _Synonyms_
> 
> - Module loading
> - Namespace injection
> - Dependency inclusion
###### Expand Detail

> [!detail]- _**+**_ 
> **When you write `import module_name`, Python searches your system's module paths (defined in `sys.path`), finds the file, executes it, and creates a namespace containing all its definitions—you then access those definitions using dot notation (e.g., `module_name.function_name()`).**

