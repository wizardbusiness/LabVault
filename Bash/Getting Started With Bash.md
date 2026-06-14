# Bash: An Intro Reference

###### Surface-level orientation to core concepts, with code examples broken down into their parts.

---

## Table of Contents

1. [[#1. Piping and Streams|Piping and Streams]]
2. [[#2. Command Patterns|Command Patterns]]
3. [[#3. Linux Everything is a File|Linux Everything is a File]]
4. [[#4. grep, cat, awk|grep, cat awk]]
5. [[#5. Key System Commands|Key System Commands]]
6. [Debugging]
7. [Configs and Package Management]
8. [cron and systemd]
9. [Scripting Syntax Intuition]
10. [Loops]
11. [Data Structures]
12. [Bash Oddities]
13. [Bash with Docker, Ansible, Terraform]
14. [What You Might Have Missed]

---

## 1. Piping and Streams

#### **Understanding Piping and Streams and How These Concepts Interconnect**

###### Every process has three standard streams: **stdin** (input, fd 0), **stdout** (output, fd 1), **stderr** (errors, fd 2). A pipe (`|`) connects one [[Command -- Bash#Definition|command]]'s stdout directly to the next [[Command -- Bash#Definition|command]]'s stdin. Think of it as an assembly line for text. 

#### **Learning with code**
##### Piping between processes

```ad-bash
~~~bash
ps aux | grep nginx | awk '{print $2}'
~~~	
**Decomposition**

- `ps` - "process status" tool; lists running processes
- `aux` - flags: `a` = all users, `u` = show user/owner, `x` = include processes not attached to a terminal
- `|` - pipe; connects stdout of first [[Command (Linux)#Definition|command]] to stdin of second [[Command (Linux)#Definition|command]], left to right. 
- `grep`- Filter tool which works by matching patterns in text. Stands for `Global Regular Expression Print`. Accepts a regex pattern, and newline-delimited (`\n`) text either via a file argument or the stdin stream. Only lines that match the regex pattern are written to stdout. Also emits an exit code: `0` for match found, `1`for no match, `2` for error.
- `nginx` - The specifric <regex> pattern being matched in the above [[Command (Linux)#Definition|command]]. Tells bash "Only pass through lines containing the exact characters 'nginx'". 
- `awk` - A pattern-matching / action-focused languge which processes stuctured text line by line, operating on fields. **AWK** stands for **Aho, Weinberger, and Kernighan** - the last names of the three Bell Labs engineers who created it in 1977. Useful for processing structured text - examples include unix [[Command (Linux)#Definition|command]] output, csvs and logs. The most common pattern is `awk '<pattern> { <action> $<field #>, $<field #>}'`. In other words, awk evaluates the pattern against each line and if it evaluates true, performs the \<action> on the \<field #> values. *Example:* `awk '$3 > 10 { print $1, $11 }'` -> prints the current value of fields `1` and `11` but only if the value of field `3` is greater than 10. 
  
**Recomposition**

- `ps` + `aux` - get all processes for all users, show the user / owner and include processes not attached to a terminal and return result to stdout. 
- `ps` + `aux` + `|` +  `grep` + `nginx` - get all processes, pipe all stdout output to grep function to filter processes output to only `nginx`-containing lines
- `ps` + `aux` +  `|` + `grep` +  `nginx` + `|` +  `awk` + `{print $2}` - get all processes, pipe all output to grep function to filter processes output to include only nginx-related lines, then from those lines extract just the process IDs 
-  `{print $2}` - Print the second whitespace-delimited field (the PID column in the `ps` tool stdout text)  
  
##### **Additional Thoughts**

The single quotes around the arguments passed to `awk` are important. They prevent bash from interpreting the $1 and $11 values as variables to expand.


```

---
##### Redirecting stderr stream to stdout stream, then redirecting stdout + stderr content into a file:

```ad-bash
~~~bash
./my_script.sh > output.log 2>&1
~~~	
**Decomposition**

- `./my_script.sh` - Execute script `my_script.sh` in current directory (`./` means "look here, not in $PATH")
- `>` - Redirect. Redirects stdout from `my_script.sh` to another stream or a file. If the redirect source is a script or file, redirects stdout unless otherwise specified. 
- `output.log` - The destination file to which the stdout of my_script.sh will be written as newline-delimited text. *Important:* When redirecting to a file '`>`' overwrites file contents - use '`>>`' to append new content to the file.
- `2` - File descriptor 2, (stderr) stream. Note that this is the start of a separate redirect instruction from the previous one. 
- `>&` - Redirect to a file descriptor (stdin, stdout or stderr). *Important:* Without `&` `>` will redirect to a file. 
- `1` - File descriptor 1 (stdout), which at this point has been redirected to output.log by the previous instruction.

**Recomposition**
- `./my_script.sh` + `>` + `output.log` - redirect stdout content of `my_script.sh` (located in the current directory) to `output.log` (also will be created in current directory).  
- `2` + `>&` + `1` — send stderr content to wherever file descriptor 1 is pointing.
- `./my_script.sh` +  `>` + `output.log` + `2` + `>&` + `1` - Send stdout of ./my_script.sh to `output.log` and send stderr from ./my_script.sh to `output.log` too, so that both streams write to one log. *Important:* Redirects evaluate left to right. so first stdout is redirected to `output.log` and *then* stderr is redirected to the same location as stdout, which is `output.log`. 
  
##### **Additional thoughts**
I personally found redirects pretty confusing to learn and it came down to how they're written by convention. Referencing the above example again, even if you understand there are two separate redirect instructions happening, it looks like only the first instruction includes the script file -> `[./my_script.sh > output.log]` `[2>&1]`. That's actually wrong. **The script file is the root [[Command (Linux)#Definition|command]], and the redirects are modifiers on it.** So the actual grouping is `[./my_script.sh]` `[> output.log]` `[2>&1]`. What makes this so confusing is convention for writing redirects omits the `1` on the first redirect instruction after the process. A valid more explicit way to write the same thing would be `./my_script 1> output.log 2>&1`.  
```

---
## 2. [[Command -- Bash#Definition|command]] Patterns

##### ***Ask*** 
1. *what shape is my data currently?*
2. *What shape do I need it in?*  
3. *What tool can transform my data into that shape?*
4. *How do I want to handle errors when a transformation fails?*
##### ***Answer***
**The shape will depend on what you need to accomplish but the pattern is almost always: _get data → filter → transform → present_**

#### Useful patterns

###### Chaining on exit codes — every [[Command -- Bash#Definition|command]] returns `0` (success) or non-zero (failure) exit code, and that exit code can be used to make decisions:

```ad-bash
~~~bash
mkdir /tmp/mydir && cd /tmp/mydir || echo "something failed"
~~~
**Decomposition**

- `mkdir` - Stands for 'Make Directory'. Builtin terminal program that creates a directory. 
- `/tmp/mydir` — The directory path starting at root (`/`). *Note:* The path of a directory start at the system root directory is also known as the *absolute path*
- `&&` — "AND" operator: run next [[Command (Linux)#Definition|command]] only if previous exited with exit code `0` (success)
- `cd` — Stands for 'Change Directory'. Builtin terminal program for changing into a directory
- `/tmp/mydir` - The absolute path to `mydir'. 
- `||` — "OR" operator: run next [[Command (Linux)#Definition|command]] only if previous exited with non-zero (failure) exit code. 
- `echo` - Builtin terminal program that prints string argument to stdout (default stdout prints to terminal).
- `"something failed"` - Arbitrary \<string>. An error message indicating a program process unexpectedly terminated (non-zero exit code).

**Recomposition**

- `mkdir` + `/tmp/mydir` - Create a new directory `mydir` inside `/tmp/`
- `cd` + `/tmp/mydir` -  Enter the directory `/tmp/mydir`. This puts the new working directory at `/tmp/mydir/`(note the trailing `/` ) 
- `echo`+ `something failed` - prints the string `'something failed'`to the terminal.
- `mkdir` + `/tmp/mydir` + `&&` + `cd` + `/tmp/mydir` - Make a new directory `mydir` inside path `/tmp/` *and* change directory to it if it is created successfully.
- `mkdir` + `/tmp/mydir` + `&&` + `cd` + `/tmp/mydir` + `||` + `echo` + `"something failed"` - Make a new directory `mydir` inside path `/tmp/` *and* if directory creates successfully, change location to the directory. If either directory creation **or** changing location to the directory fails print the message `"something failed"` to the terminal. 
```

---
## 3. Linux: Everything is a File

###### Devices, processes, and sockets are all exposed as files. For example `/proc` exposes live kernel and process data. The same tools (`cat`, `grep`, `echo`) work on all of the above because they are all files that read as newline-delimited text content. 


#### Devices as a file - Printing CPU model name to the terminal by accessing the `cpuinfo` file 

```ad-bash
~~~bash
cat /proc/cpuinfo | grep "model name" | head -1
~~~	
**Decomposition**

- `cat` - Stands for *Concatenate*; Builtin program that dumps file contents to stdout location (terminal by default)
- `/proc/cpuinfo` - not a real file on disk; the kernel generates this content on-the-fly when read.
= `|` - Pipe; connects stdout of first [[Command (Linux)#Definition|command]] to stdin of second [[Command (Linux)#Definition|command]], left to right. 
- `grep` - Filter tool which works by matching patterns in text. Stands for *Global Regular Expression Print*. Accepts a regex pattern, and newline-delimited (`\n`) text either via a file argument or the stdin stream. Only lines that match the regex pattern are written to stdout.
- `"model name"` - String being used as a regex pattern argument for `grep`. This pattern means 'Keep only lines containing the contiguous string "model name"'. 
- `head` - Refers to the beginning of some newline-delimited text. Builtin program that prints a subset of lines from stdin or file starting at the top. 
- `-1` - Modifier to `head` [[Command (Linux)#Definition|command]]. Tells `head` 'Print only the first line' (`-1` = one line; default is 10)

**Recomposition**

- `cat` + `/proc/cpuinfo` = print the contents of `/proc/cpuinfo` to the terminal.
- `grep` + `"model name"` = Read stdout from previous [[Command (Linux)#Definition|command]] (not shown) and filter to only lines containing the phrase `"model name"`
- `head -1` = Read stdout from previous [[Command (Linux)#Definition|command]] (not shown) and show only the first line. (first 10 lines without `-1`)
- `cat` + `/proc/cpuinfo` + `|` + `grep` +  `"model name"` + `| head -1` = print the contents of `/proc/cpuinfo` to the terminal, but filter for only lines containing the regex pattern "model name" and print only the first line from potentially many matching lines.  
```

---
#### Writing to the terminal device directly, bypassing any stdout stream redirection:

~~~ad-bash
```bash
echo "hello" > /dev/tty
```
**Decomposition**

- `echo` - Terminal built-in that prints text to stdout (terminal by default). 
- "hello"` - Arbitrary string. Being passed as argument to echo
- `>` - Redirect. Redirects a stream to another stream or a file. If the redirect source is a script or file, redirects stdout unless otherwise specified. 
- `/dev/tty` — the current terminal device (a file representing your terminal)

**Recomposition**

- `echo` + `> /dev/tty` — write text directly to the terminal, bypassing any pipe redirection
~~~





---

## 4. grep, cat, awk

The core text-processing triad. `cat` gets data out, `grep` filters it, `awk` transforms and structures it.

 **Name origins:**
- `cat` - Con***cat***enate (originally for joining files, now mostly used to dump one)
- `grep` - From the `ed` editor [[Command -- Bash#Definition|command]] `g/re/p`: _globally search a regular expression and print_
- `awk` - Named after its authors: **A**ho, **W**einberger, **K**ernighan

#### Cat

~~~ad-bash
```bash
cat /var/log/syslog
```
**Decomposition**

- `cat /var/log/syslog` — dump the syslog file to stdout

**Recomposition**

- `cat` + `| grep "ERROR"` — get all log lines, keep only errors
- `| grep` + `| awk '{print $1, $2, $NF}'` — from error lines, print timestamp (fields 1-2) and the last field (usually the message or code)

~~~

***
#### Grep 

~~~ad-bash
```bash
grep "ERROR" /var/log/syslog
```
**Decomposition**
- `grep "ERROR"` — filter to only lines containing "ERROR"
**Recomposition**
~~~

***
#### Awk 

##### Pipeline AWK use - Printing specific fields from  stdout of previous [[Command -- Bash#Definition|command]]

```ad-bash
~~~bash
awk '{print $1, $2, $NF}'
~~~~

**Decomposition**
- `awk` - A pattern-matching / action-focused languge which processes stuctured text line by line, operating on fields. **AWK** stands for **Aho, Weinberger, and Kernighan** - the last names of the three Bell Labs engineers who created it in 1977. Useful for processing structured text - examples include unix [[Command (Linux)#Definition|command]] output, csvs and logs. The most common pattern is `awk '<pattern> { <action> $<field #>, $<field #>}'`. In other words, awk evaluates the pattern against each line and if it evaluates true, performs the \<action> on the \<field #> values. *Example:* `awk '$3 > 10 { print $1, $11 }'` -> prints the current value of fields `1` and `11` but only if the value of field `3` is greater than 10. 
- `'...'` - Single quotes pass code content to `awk` as-is, preventing bash from interpreting `$1`, `$2` etc as shell variables. 
- `{...}` - Wraps `awk` action block. The action block applies an action to any fields defined within the block 
- `print` - `awk` built-in function which outputs text to stdout. Is being used as the 'action' in the action block here. Usually used to print fields in lines of structured text, but can print any text.
- `$<index>` - Resolves to the field at position `<index>`. 
- `NF`- Awk built-in variable meaning 'Number of Fields'. Resolves to the number *n* within a line with *n* fields.   
  
**Recomposition**
- `awk` + `'` + `{` + `print` + `$` + `1,` + `$` + `2,` + `$` + `NF` + `}` + `'` - Executes awk code (wrapped in a single-quoted string) that prints the 1st, 2nd, and last field in a line of structured whitespace-delimited text. 
```

##### Print specific fields from a specified text file. 

###### Parsing  `/etc/passwd` using `awk` with a custom field separator 

```ad-bash
~~~bash
awk -F: '{print $1, $7}' /etc/passwd
~~~
**Decomposition**

`awk` - A pattern-matching / action-focused languge which processes stuctured text line by line, operating on fields. **AWK** stands for **Aho, Weinberger, and Kernighan** - the last names of the three Bell Labs engineers who created it in 1977. Useful for processing structured text - examples include unix [[Command (Linux)#Definition|command]] output, csvs and logs. The most common pattern is `awk '<pattern> { <action> $<field #>, $<field #>}'`. In other words, awk evaluates the pattern against each line and if it evaluates true, performs the \<action> on the \<field #> values. *Example:* `awk '$3 > 10 { print $1, $11 }'` -> prints the current value of fields `1` and `11` but only if the value of field `3` is greater than 10. 
- `-F` - Field delimiter flag 
- `{...}` - Wraps `awk` action block. The action block applies an action to any fields defined within the block 
- `print` - Built-in `awk` function which outputs text to stdout. Is being used as the 'action' in the action block here. Usually used to print fields in lines of structured text, but can print any text.
- `$<index>,` - Resolves to the field at position `<index>`. Field definitions are separated by commas. 
- `/etc/passwd` — input file passed directly to awk (no cat needed). *Note:* Path definitions without spaces are automatically converted to strings. Paths with spaces must be wrapped in single or double quotes. 
  
**Recomposition**

- `awk` + `-F` + `:` + `'` + `{` + `print` + `$` + `1,` + `$` + `7}` + `'` + `/etc/passwd`- Executes awk code (wrapped in a single-quoted string) to split each line of text in the file `/etc/passwd` on colons(`:`), extracts the 1st and 7th fields (username and assigned shell) and prints to stdout (default terminal)
```

---

## 5. Key System [[Command -- Bash#Definition|command]]s

Don't memorize flags — learn to read `man` pages fast. Pattern: single-dash for short flags (`-a`), double-dash for long (`--all`).

```ad-bash
find /var/log -name "*.log" -mtime -7 -size +1M
```

**Decomposition**

- `find` — recursively search for files matching criteria
- `/var/log` — starting directory for the search
- `-name "*.log"` — match files where name ends in `.log` (`*` = wildcard)
- `-mtime -7` — modified within the last 7 days (`-` means "less than"; `+` would mean "more than")
- `-size +1M` — larger than 1 megabyte

**Recomposition**

- `find /var/log` + `-name "*.log"` — look in log directory for log files
- `-name "*.log"` + `-mtime -7` + `-size +1M` — narrow to: recently modified AND large; useful for finding runaway logs

---

```bash
df -h | sort -k5 -rn | head -5
```

**Decomposition**

- `df` — "disk free"; reports filesystem disk space usage
- `-h` — human-readable sizes (MB, GB instead of blocks)
- `sort` — sort lines of text
- `-k5` — sort by the 5th field (the "Use%" column in df output)
- `-r` — reverse order (highest first)
- `-n` — numeric sort (so "90%" sorts above "9%")
- `head -5` — show only top 5 lines

**Recomposition**

- `df -h` + `| sort -k5 -rn` — get disk usage, rank by fullest first
- `| sort -k5 -rn` + `| head -5` — from ranked list, show only the top 5 most full filesystems

---

## 6. Debugging

```bash
#!/bin/bash
set -euxo pipefail
```

**Decomposition**

- `#!/bin/bash` — shebang; tells the OS which interpreter to use for this script
- `set` — modify shell behavior flags
- `-e` — exit immediately if any [[Command -- Bash#Definition|command]] returns non-zero
- `-u` — treat unset variables as errors (prevents silent bugs from typos)
- `-x` — print each [[Command -- Bash#Definition|command]] to stderr before executing it (execution trace)
- `-o pipefail` — if any [[Command -- Bash#Definition|command]] in a pipe fails, the whole pipe returns failure (without this, `false | true` returns 0)

**Recomposition**

- `-e` + `-u` — catch errors and undefined variables; the script won't silently keep running after something breaks
- `-x` + `-o pipefail` — trace execution and catch pipe failures; together these four flags make scripts dramatically easier to debug

---

Checking exit codes manually:

```bash
curl -sf https://example.com/health || { echo "Health check failed" >&2; exit 1; }
```

**Decomposition**

- `curl` — transfer data from URLs
- `-s` — silent; suppress progress output
- `-f` — fail silently on HTTP errors (returns non-zero exit code on 4xx/5xx)
- `||` — run right side only if left side failed
- `{ ...; }` — group [[Command -- Bash#Definition|command]]s (note: space after `{` and `;` before `}` are required)
- `echo "Health check failed"` — print error message
- `>&2` — redirect that echo to stderr (where error messages belong)
- `exit 1` — terminate script with failure code

**Recomposition**

- `curl -sf` + `||` + `{ echo >&2; exit 1; }` — check endpoint; on failure, log to stderr and abort the script

---

## 7. Configs and Package Management

```bash
export PATH="$HOME/.local/bin:$PATH"
```

**Decomposition**

- `export` — mark variable for export to child processes (without this, child processes won't see it)
- `PATH` — special env variable; colon-separated list of directories the shell searches for [[Command -- Bash#Definition|command]]s
- `"$HOME/.local/bin:$PATH"` — prepend your local bin directory to the existing PATH (`:` is the separator; quoting preserves spaces in paths)

**Recomposition**

- `$HOME/.local/bin:` + `$PATH` — put your directory first so it takes priority over system-wide [[Command -- Bash#Definition|command]]s with the same name

---

Finding where a [[Command -- Bash#Definition|command]] lives:

```bash
which python3 && type python3
```

**Decomposition**

- `which python3` — searches `$PATH` and prints the full path of the binary
- `&&` — run next if previous succeeded
- `type python3` — bash built-in; tells you more than `which` — distinguishes aliases, functions, builtins, and files

**Recomposition**

- `which` + `type` — belt-and-suspenders check; `type` will reveal if `python3` is actually an alias masking the real binary

---

## 8. cron and systemd

**cron** — old-school scheduler. `crontab -e` opens your user crontab.

```
# ┌───── minute (0-59)
# │ ┌───── hour (0-23)
# │ │ ┌───── day of month (1-31)
# │ │ │ ┌───── month (1-12)
# │ │ │ │ ┌───── weekday (0-7, 0 and 7 = Sunday)
# │ │ │ │ │
  0 2 * * 1 /usr/local/bin/weekly-backup.sh >> /var/log/backup.log 2>&1
```

**Decomposition**

- `0 2 * * 1` — at minute 0, hour 2, any day, any month, on Monday (weekday 1)
- `/usr/local/bin/weekly-backup.sh` — absolute path required; cron runs with a minimal environment and won't find scripts via PATH shortcuts
- `>> /var/log/backup.log` — append stdout to log file
- `2>&1` — merge stderr into stdout so both go to the log

**Recomposition**

- `0 2 * * 1` + absolute path — schedule script to run every Monday at 2am
- `>> log` + `2>&1` — capture all output; without this, cron silently discards it (or emails it, which you don't want)

---

**systemd** — modern service manager. Unit files live in `/etc/systemd/system/`.

```ini
[Unit]
Description=My App
After=network.target

[Service]
ExecStart=/usr/local/bin/myapp
Restart=always
User=myappuser

[Install]
WantedBy=multi-user.target
```

**Decomposition**

- `[Unit]` — metadata and dependency ordering
- `After=network.target` — don't start until networking is up
- `[Service]` — how to run the process
- `ExecStart=` — the [[Command -- Bash#Definition|command]] to run
- `Restart=always` — restart the process if it dies, for any reason
- `User=myappuser` — run as this user, not root
- `WantedBy=multi-user.target` — enable this service when the system reaches normal multi-user mode (i.e., on boot)

```bash
systemctl enable --now myapp.service
```

- `systemctl` — interface to systemd
- `enable` — create symlinks so service starts on boot
- `--now` — also start it immediately, don't wait for next reboot
- `journalctl -u myapp.service -f` — follow live logs for this unit

---

## 9. Scripting Syntax Intuition

Key rules: **no spaces around `=` in assignments**, variables are untyped strings, quoting is critical.

```bash
#!/bin/bash
name="world"
greeting="Hello, ${name}!"
echo "$greeting"
```

**Decomposition**

- `name="world"` — variable assignment; no spaces around `=` (bash would interpret `name = "world"` as running a [[Command -- Bash#Definition|command]] called `name`)
- `${name}` — variable expansion with braces; braces are optional here but required when followed directly by more characters (e.g., `${name}s`)
- `"Hello, ${name}!"` — double quotes: variables expand inside them; use single quotes to prevent expansion
- `echo "$greeting"` — always quote variable expansions; unquoted variables get word-split which causes subtle bugs

**Recomposition**

- `name=` + `"${name}"` in later string — assign once, reference many times; braces make the boundary of the variable name explicit

---

[[Command -- Bash#Definition|command]] substitution — capturing [[Command -- Bash#Definition|command]] output into a variable:

```bash
current_date=$(date +%Y-%m-%d)
echo "Today is $current_date"
```

**Decomposition**

- `$(...)` — [[Command -- Bash#Definition|command]] substitution; runs the [[Command -- Bash#Definition|command]] inside and substitutes its stdout output
- `date` — print current date/time
- `+%Y-%m-%d` — format string: 4-digit year, month, day separated by hyphens
- `` `backticks` `` — older syntax for the same thing; avoid in favor of `$()` which is nestable

**Recomposition**

- `$(date +%Y-%m-%d)` — execute date [[Command -- Bash#Definition|command]], capture output, assign to variable for later use

---

## 10. Loops

```bash
for file in /var/log/*.log; do
    echo "Processing: $file"
    wc -l "$file"
done
```

**Decomposition**

- `for file in` — loop; assign each item in the list to variable `file`
- `/var/log/*.log` — glob pattern; shell expands this to all matching filenames before the loop starts
- `do` — begin loop body (required keyword)
- `wc -l "$file"` — "word count" with `-l` flag = line count; `wc` counts words, lines, or bytes
- `"$file"` — quoted to handle filenames with spaces
- `done` — end loop body (required keyword)

**Recomposition**

- `for file in *.log` + `do...done` — iterate over every log file; the glob expansion means you never hardcode filenames

---

Reading a file line by line (the correct pattern):

```bash
while IFS= read -r line; do
    echo "$line"
done < /etc/hosts
```

**Decomposition**

- `while` — loop while condition is true
- `IFS=` — temporarily set Internal Field Separator to empty string; prevents leading/trailing whitespace from being stripped
- `read` — read one line from stdin into a variable
- `-r` — raw mode; prevents backslash interpretation (almost always what you want)
- `line` — variable name to store each line
- `done < /etc/hosts` — redirect file into the while loop's stdin; the `<` is on the `done` line by convention

**Recomposition**

- `IFS= read -r line` — the safe line-reading idiom; omitting either `IFS=` or `-r` causes subtle data corruption on certain inputs
- `done < file` — feed file into loop via stdin redirect rather than piping (piping would create a subshell, making variable assignments inside the loop invisible outside it)

---

C-style for loop:

```bash
for ((i=0; i<5; i++)); do
    echo "Iteration $i"
done
```

**Decomposition**

- `((...))` — arithmetic context; enables C-style expressions
- `i=0` — initialize counter
- `i<5` — continue condition
- `i++` — increment after each iteration

---

## 11. Data Structures

**Indexed array:**

```bash
fruits=("apple" "banana" "cherry")
echo "${fruits[0]}"        # first element
echo "${fruits[@]}"        # all elements
echo "${#fruits[@]}"       # array length
fruits+=("date")           # append
```

**Decomposition**

- `fruits=(...)` — declare indexed array; parentheses with space-separated values
- `${fruits[0]}` — access by index; braces required for array access
- `${fruits[@]}` — `@` expands to all elements as separate words (use this, not `*`)
- `${#fruits[@]}` — `#` prefix gives length
- `+=("date")` — append to array

---

**Associative array (hashmap):**

```bash
declare -A config
config["host"]="localhost"
config["port"]="5432"
echo "${config["host"]}"
echo "${!config[@]}"   # all keys
```

**Decomposition**

- `declare -A` — declare associative array (`-A` flag); required, unlike indexed arrays
- `config["host"]=` — set value for key "host"
- `${!config[@]}` — `!` prefix with array gives keys instead of values

**Recomposition**

- `declare -A` + `config["key"]="val"` — the two-step for hashmaps; skip `declare -A` and you'll get cryptic errors

---

## 12. Bash Oddities

**`[[ ]]` vs `[ ]`** — always use double brackets:

```bash
filename="my file.txt"
if [[ -f "$filename" && "$filename" == *.txt ]]; then
    echo "It's a text file"
fi
```

**Decomposition**

- `[[ ]]` — bash keyword (not a [[Command -- Bash#Definition|command]]); safer, supports `&&`/`||` inside, no word-splitting on variables
- `[ ]` — POSIX `test` [[Command -- Bash#Definition|command]]; older, portable, but requires more quoting and can't use `&&` inside
- `-f` — file test: true if path exists and is a regular file (other tests: `-d` dir, `-e` exists, `-r` readable, `-x` executable)
- `*.txt` — glob pattern matching inside `[[ ]]` (works without quotes; would be literal string in `[ ]`)

---

**Arithmetic — no floats natively:**

```bash
count=10
result=$(( count * 3 + 1 ))
echo $result  # 31

# For floats, use bc or awk:
echo "scale=2; 10 / 3" | bc       # 3.33
awk 'BEGIN { printf "%.2f\n", 10/3 }'  # 3.33
```

**Decomposition**

- `$(( ))` — arithmetic expansion; integer math only
- `scale=2` — bc directive: set decimal places
- `bc` — "basic calculator"; arbitrary precision math via stdin
- `awk 'BEGIN { }'` — `BEGIN` block runs before any input is processed; used here for pure calculation with no input file

---

**vs PowerShell:** PowerShell pipes _objects_, bash pipes _text_. In PowerShell, `Get-Process | Where-Object CPU -gt 10` passes structured objects. In bash, you're always parsing text — which is why `awk`, `grep`, and `cut` exist. Neither is strictly better; they reflect different philosophies.

**vs Python:** Use bash for process orchestration and gluing tools together. Switch to Python when you need real data structures, error handling, or anything beyond ~50 lines of logic. The heuristic: if you're fighting bash syntax, use Python.

---

## 13. Bash with Docker, Ansible, Terraform

Bash glues these tools together — looping over resources, conditional deploys, parsing JSON output.

```bash
# Restart all stopped Docker containers
docker ps -a --filter "status=exited" --format "{{.ID}}" | \
    xargs -r docker start
```

**Decomposition**

- `docker ps -a` — list all containers (`-a` = include stopped)
- `--filter "status=exited"` — only show stopped containers
- `--format "{{.ID}}"` — Go template format; output only container IDs, one per line
- `|` — pipe IDs to next [[Command -- Bash#Definition|command]]
- `xargs` — "extended arguments"; takes stdin and converts it to [[Command -- Bash#Definition|command]]-line arguments
- `-r` — don't run [[Command -- Bash#Definition|command]] if stdin is empty (prevents `docker start` with no arguments)
- `docker start` — xargs appends the IDs here; effectively runs `docker start id1 id2 id3...`

**Recomposition**

- `docker ps --format "{{.ID}}"` + `| xargs docker start` — extract IDs into a stream, then use xargs to feed them as arguments to the next [[Command -- Bash#Definition|command]]; this pattern works for any "get list, act on each" task

---

Parsing Terraform output with `jq`:

```bash
terraform output -json | jq -r '.instance_ip.value'
```

**Decomposition**

- `terraform output -json` — emit all outputs as JSON
- `jq` — [[Command -- Bash#Definition|command]]-line JSON processor (like awk for JSON)
- `-r` — raw output; strips JSON string quotes from the result
- `'.instance_ip.value'` — jq filter: navigate to `instance_ip` key, then its `value` field

**Recomposition**

- `-json` + `| jq -r '...'` — the standard pattern for extracting values from any CLI tool that speaks JSON; `jq` is essential once you're working with infrastructure tools

---

## 14. What You Might Have Missed

**Process substitution** — treat [[Command -- Bash#Definition|command]] output as a file:

```bash
diff <(sort file1.txt) <(sort file2.txt)
```

- `<(...)` — runs [[Command -- Bash#Definition|command]] in subshell, substitutes a file descriptor; `diff` sees two "files" even though they're [[Command -- Bash#Definition|command]] outputs

---

**Heredoc** — multiline string, inline:

```bash
cat <<EOF > /tmp/config.yml
host: localhost
port: 5432
name: mydb
EOF
```

- `<<EOF` — start heredoc; read stdin until a line containing only `EOF`
- `EOF` — delimiter (any word works; `EOF` is convention)
- Everything between is treated as stdin to `cat`, which redirects it to the file

---

**trap** — run cleanup on exit or signal:

```bash
tmpfile=$(mktemp)
trap "rm -f $tmpfile" EXIT
```

- `trap "[[Command (Linux)#Definition|command]]" SIGNAL` — register [[Command -- Bash#Definition|command]] to run when signal is received
- `EXIT` — pseudo-signal; fires when script exits for any reason (clean or crash)
- Pattern: create temp resources, register trap to clean them up — they'll be removed even if the script errors out

---

**Subshell vs current shell:**

```bash
source ~/.bashrc    # runs in current shell — changes affect current session
./myscript.sh       # runs in subshell — changes (cd, export) don't affect parent
```

- `. file` or `source file` — execute in current shell; used to load configs and functions
- `./file` — spawn child process; environment changes are isolated

---

**xargs for parallelism:**

```bash
cat urls.txt | xargs -P 4 -I {} curl -s {} -o /dev/null
```

- `-P 4` — run up to 4 processes in parallel
- `-I {}` — replace `{}` with each input item (allows placing the argument anywhere in the [[Command -- Bash#Definition|command]])
- `-o /dev/null` — discard response body; `/dev/null` is the "black hole" file — writes succeed, reads return nothing

---

**job control:**

```bash
sleep 100 &        # run in background
jobs               # list background jobs
fg %1              # bring job 1 to foreground
wait               # wait for all background jobs to finish
```

- `&` — run [[Command -- Bash#Definition|command]] in background (returns shell prompt immediately)
- `%1` — job specification; `%1` = first job
- `wait` — block until all background jobs complete; essential in scripts that parallelize work

---

_Reference this as a map, not a manual. Each section is a door — go deeper when you need to._