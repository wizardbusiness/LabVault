##### ***Use To*** Keep Python modules for a project separate from your systems python install so you don't run into dependency issues for different projects using the same package but requiring different versions.   

#### Prerequisites
- [[Python>=3.14]]
- [[Install Pip|Pip]]
#### More Resources
1. [Virtual Environment Project Setup Guide - Claude][1]
2. [venv - Creation of Virtual Environmnents - Python Docs][2]

[1]: https://claude.ai/public/artifacts/cfcae188-e963-4e9a-9fef-97b0309239fc
[2]: https://docs.python.org/3/library/venv.html
#### Related Docs
- [[<Related Doc>]]
#### Quick Reference
[[New Virtual Env#^quickref|Creating New Virtual Env Quick Reference -->]]
### *Info*
##### Virtual Envs (Virtual Environments) are isolated Python installations scoped to a single project. They keep your dependencies from polluting your systems base Python-install and from conflicting with other projects. Each venv gets its own `site-packages` directory.[1]

> [!quote] **Explanation Excerpt - Official Docs**
> *The `venv` module supports creating lightweight “virtual environments”, each with their own independent set of Python packages installed in their [`site`](https://docs.python.org/3/library/site.html#module-site "site: Module responsible for site-specific configuration.") directories. A virtual environment is created on top of an existing Python installation, known as the virtual environment’s “base” Python, and by default is isolated from the packages in the base environment, so that only those explicitly installed in the virtual environment are available.[2]*
> 

---
## Steps

### 1. Create the Project Directory
> [!bash]- Bash `snippet` 
> ~~~bash
> mkdir path/to/<project-name> && cd path/to/<project-name>
> ~~~
> > [!example]- **Decomposition**
> > - `mkdir` - Shell built-in that creates a new directory
> > - `path/to/` - The relative or absolute path of the parent directory within which you're creating the project directory. 
> > - `<project-name>` - The name of the project directory. 
> > - `&&` - Shell operator that runs the next command _only if_ the previous one succeeded. 
> > - `cd` - Shell built-in that changes the current working directory
> > 
>
> >[!tip]-  **Recomposition**
> >- `mkdir` + `path/to/` + `<project-name>` - Creates new directory `<project-name>` so long as that directory doesn't exist in `path/to/`. 
> >- `cd` + `path/to/` + `<project-name>`- Move into the directory `path/to/<project-name>` if it exists. 
> >- `mkdir` + `my-project` + `&&` + `cd` + `my-project` - Creates the project directory, then moves into it - but only if the directory was created successfully. If `mkdir` fails (e.g. the directory already exists), `cd` never runs.
> 
> **Additional Thoughts**
> - *'Do a second thing only if the first thing succeeds' is a common CLI pattern. It provides insurance that operations that depend on the success  of each other don't fail or partially complete silently.*
> - *`mkdir <path><project-name>` is all you actually **need** to do here.*  
> - *Totally fine to do this from the file explorer too.*
> 

### 2. Create the Virtual Environment
> [!py]- ***Python*** `snippet` 
> 
>~~~python
>python3 -m venv .venv
>~~~
>
>
>> [!example]- **Decomposition**
>> 
>>- `python3` - Calls the Python 3 interpreter binary
>>- `-m` - Flag that stands for 'module' — tells Python to run a named standard library module as a script
>>- `venv` - The standard library module responsible for creating virtual environments
>>- `.venv` - The name of the directory where the environment will be created. The leading dot follows Unix convention for hidden directories —keeps it out of `ls` output and is what most tooling (VS Code, pyright) looks for automatically
>
>> [!tip]- **Recomposition**
>>
>>- `python3` + `-m` + `venv` + `.venv` - Uses the Python 3 interpreter to run the `venv` module as a script, which creates an isolated environment in a hidden folder called `.venv`

### 3. Activate the Environment
> [!bash]- Bash `snippet`
> ~~~bash
> source .venv/bin/activate
> ~~~
> > [!example]- **Decomposition**
>> - `source` - Shell built-in that runs a script _inside the current shell session_, not in a subshell
>>  - `.venv/bin/activate` - Path to the activation script that the `venv` module generated
>
> >[!tip]-  **Recomposition**
> >- `source` + `.venv/bin/activate` - Runs the activation script in your current shell so that environment changes - like prepending `.venv/bin` to `$PATH` - stick in your session. Without `source`, the script runs in a child process and the changes die with it. After activation, `python` and `pip` resolve to the venv's binaries instead of the system ones. Your prompt will change to show `(.venv)`.
> 
> 
> 

```powershell
# Windows (PowerShell)
.venv\Scripts\Activate.ps1
```

>[!info] **Additional Tip: How to deactivate the environment**
>To undo: run `deactivate` — a shell function that the `activate` script injected into your session.
### 4. Confirm You're in the Right Environment
> [!bash]- Bash `Snippet`
> ```bash
> which python
>```
>
>>[!example]- **Decomposition**
>>- `which` - Shell built-in that searches `$PATH` and prints the full resolved path of a binary
>>- `python` - The binary to look up
>
>>[!tip]- **Recomposition**
>>- `which` + `python` - Asks your shell "which `python` binary would actually run right now?" After activation the answer should be `.venv/bin/python`. If it's still pointing at `/usr/bin/python3`, your activation didn't stick.
### 5. Install Packages
> [!bash]- Bash `snippet`
>```bash
> pip install requests flask pandas
>```
>>[!example]- **Decomposition**
>>
>> - `pip` - Python's package installer. Stands for 'Pip Installs Packages' — a recursive acronym
>> - `install` - Subcommand that tells pip to fetch and install packages from PyPI
>> - `requests flask pandas` - Space-separated list of package names to install
>
>>[!tip]- **Recomposition**
>>
>>- `pip` + `install` + `requests flask pandas` - Downloads and installs the listed packages from PyPI into the active environment's `site-packages`. Because the venv is active, they install here — not system-wide.

---

### 6. Freeze Dependencies

```bash
pip freeze > requirements.txt
```

**Decomposition**

- `pip` - Python's package installer
- `freeze` - Subcommand that outputs all installed packages with their exact pinned versions to stdout
- `>` - Shell redirect operator that sends stdout to a file, overwriting it if it already exists
- `requirements.txt` - The target filename. This is a Python convention — every tool in the ecosystem recognizes it

**Recomposition**

- `pip` + `freeze` + `>` + `requirements.txt` - Asks pip to list every installed package at its exact version, then redirects that output into `requirements.txt` instead of printing it to the terminal. The `==` pinned versions (e.g. `requests==2.31.0`) mean you get identical installs when recreating the environment later.

---

## 7. Recreate the Environment Elsewhere

```bash
pip install -r requirements.txt
```

**Decomposition**

- `pip` - Python's package installer
- `install` - Subcommand to fetch and install packages
- `-r` - Flag that stands for 'requirements' — tells pip to read package specs from a file instead of the command line
- `requirements.txt` - The file to read from

**Recomposition**

- `pip` + `install` + `-r` + `requirements.txt` - Tells pip to read the requirements file and install everything listed, with pinned versions. Run this after creating and activating a fresh venv on any machine to get an identical environment.

---

## 8. .gitignore

```
.venv/
__pycache__/
*.pyc
```

**Decomposition**

- `.venv/` - The virtual environment directory. Never commit this — it's hundreds of MB of installed packages. Recreate it from `requirements.txt`
- `__pycache__/` - Directory Python auto-creates to store compiled bytecode. The double underscores (called **dunders**) signal a name that Python itself manages — you'll see this convention throughout the language (`__init__`, `__main__`, etc.)
- `*.pyc` - Compiled Python bytecode files. The `*` is a glob wildcard matching any filename with the `.pyc` extension
- `.pyc` - Stands for 'Python compiled' — these are machine-generated artifacts, not source code

**Recomposition**

- `.venv/` + `__pycache__/` + `*.pyc` - Tells git to ignore all auto-generated, environment-specific artifacts. Only `requirements.txt` and your actual source code belong in version control.

---

## Full Setup — Start to Finish

```bash
mkdir my-project && cd my-project
python3 -m venv .venv
source .venv/bin/activate
pip install <your-packages>
pip freeze > requirements.txt
```

---
#### Quick Reference ^quickref

| Task                   | Command                           |
| ---------------------- | --------------------------------- |
| Create venv            | `python3 -m venv .venv`           |
| Activate (Linux/macOS) | `source .venv/bin/activate`       |
| Activate (Windows)     | `.venv\Scripts\Activate.ps1`      |
| Deactivate             | `deactivate`                      |
| Install package        | `pip install <pkg>`               |
| Freeze deps            | `pip freeze > requirements.txt`   |
| Install from file      | `pip install -r requirements.txt` |
| Confirm active Python  | `which python`                    |
