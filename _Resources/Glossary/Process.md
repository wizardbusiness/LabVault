---
term: Process
category:
  - Operating System
  - Systems Programming
  - Concurrency
related:
  - Thread
  - Scheduler
  - Kernel
  - Memory Space
  - PID
  - Context Switch
  - Daemon
  - Fork
  - IPC
  - CPU
---

##### Definition

**`Process`**

**A running instance of a [[Program#Definition|program]], managed by the operating system.**

###### Expand Details

> [!detail]- **+** 
**When a program executes, the OS wraps it in a process — allocating it its own protected _memory space_, a _PID_ (Process ID), and CPU time via the _scheduler_. Multiple processes can run concurrently, but each is isolated; they communicate only through defined _IPC_ (Inter-Process Communication) channels.**

> [!detail]- _Synonyms_
> 
> - Task
> - Job (in batch computing contexts)

