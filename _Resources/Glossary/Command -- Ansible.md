---
term: Command
category:
  - Automation
  - Configuration Management
  - Ansible
related:
  - Module
  - Task
  - Playbook
  - Shell
  - Idempotency
  - Ad-hoc
  - Inventory
  - Control Node
---
##### Definition

**`Command`**

**A directive sent to a managed host to execute a shell instruction directly, bypassing Ansible's module system.**
##### More 

> [!detail]- *Expanded Explanation*
> **Used in _tasks_ via the `command` module, it runs a specified executable on the target _managed node_ without invoking a shell interpreter — meaning shell features like pipes (`|`) and redirects (`>`) are unavailable; for those, the `shell` module is used instead. Unlike most Ansible _modules_, `command` is not inherently _idempotent_, so it will run every time a _playbook_ is executed unless explicitly conditioned with `when` or `creates`.**

> [!detail] _Also called_
> 
> - Shell Instruction
> - Directive
