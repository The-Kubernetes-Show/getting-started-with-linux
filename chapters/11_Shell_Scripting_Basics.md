# Chapter 11: Shell Scripting Basics

---

[YouTube Video](https://youtu.be/03IrMd84VMA)

---

At the command line you already run individual commands. Shell scripting lets you glue those commands together into repeatable, sharable workflows.

## 11.1 Learning goals

By the end of this chapter you should be able to:

- Explain what a shell script is and why you would use one
- Write, save, and execute simple Bash scripts
- Use variables, arguments, and exit codes in basic scripts
- Accept user input and make simple decisions with `if`
- Loop over lists and files
- Apply a few basic safety and style practices

Most examples use Bash, which is the default interactive shell on many Ubuntu and RHEL systems. For details beyond this chapter you can always refer to the GNU Bash Reference Manual:  
https://www.gnu.org/software/bash/manual/bash.html

---

## 11.2 What is a shell script

A shell is a command language interpreter. Bash is one such shell that is widely used on Linux and documented in the GNU Bash manual.

From that manual’s introduction:

> “Bash is the shell, or command language interpreter, for the GNU operating system. A shell is simply a macro processor that executes commands.”

You can think of a shell script as a text file containing a sequence of shell commands, plus some control logic such as variables, conditionals, and loops. When you run the script the shell reads the file and executes those commands in order.

Common use cases:

- Running the same sequence of commands every day without retyping them  
- Automating system maintenance tasks  
- Creating small tools that wrap longer command sequences into a single command  

> **Hands on**  
> - Run:
>   ```bash
>   echo "Hello from the shell"
>   pwd
>   ls
>   ```
>   Now imagine having those lines in a file and running them all at once. That is the starting point for scripting.

---

## 11.3 Your first Bash script

### 11.3.1 Shebang and script file

Most Bash scripts start with a “shebang” line:

```bash
#!/bin/bash
```

This tells the system to use `/bin/bash` to interpret the rest of the file. The GNU Bash manual describes Bash as a command interpreter and explains how it reads and parses input:
https://www.gnu.org/software/bash/manual/bash.html



**Tip:** People use `env` in the `shebang` to make scripts more portable and less tied to a specific filesystem layout. e.g.
`#!/usr/bin/env bash`

**What `env` does here?**
`env` is a small program that runs another program after looking it up in `$PATH`. The GNU coreutils manual describes env as a tool that can “run a command in a modified environment,” and it searches `$PATH` to find the given command:
following this link to read more about it: https://www.gnu.org/software/coreutils/manual/html_node/env-invocation.html

Let us create a minimal script.

> **Hands on: hello world script**

1. Create a file:

```bash
vim hello.sh
```

2. Put this content in it:

```bash
#!/bin/bash
# Simple hello world script

echo "Hello, world from a script"
```

3. Save and exit.
4. Make it executable:

```bash
chmod +x hello.sh
```

5. Run it:

```bash
./hello.sh
```


You should see the greeting printed once, just like if you had typed `echo` yourself.

The Ubuntu “Beginners/BashScripting” page walks through a similar first example with `#!/bin/bash`, `echo`, and `chmod +x`:
https://help.ubuntu.com/community/Beginners/BashScripting

---

### 11.3.2 Running scripts with bash explicitly

You can also run a script by calling Bash directly:

```bash
bash hello.sh
```

In that case the execute bit on the file is not strictly required because `bash` is reading the file as input.

> **Hands on**
> - Remove the execute bit:
>   ```bash 
>   chmod -x hello.sh 
>   ```
> - Run:
>   ```bash 
>   bash hello.sh 
>   ```
>   Note that this still works
> - Try `./hello.sh` again and observe the “permission denied” error

---

## 11.4 Variables and simple parameters

Bash has shell parameters which include variables you define and special parameters such as `$1`, `$2`, and `$?`. The Bash manual describes these under “Shell Parameters” and “Special Parameters” in detail.

### 11.4.1 Defining and using variables

Variables in Bash are simple name value pairs:

```bash
name="Tux"
echo "Hello, $name"
```

There must be no spaces around the `=` sign. When you expand the variable, you prefix it with `$`.

> **Hands on: greeter script**

Create `greeter.sh`:

```bash
#!/bin/bash
# Simple greeter

name="TKS learner"
echo "Hello, $name"
```

Make it executable and run it:

```bash
chmod +x greeter.sh
./greeter.sh
```

Change the `name` value and run it again.

---

### 11.4.2 Script arguments `$1`, `$2`, and `$@`

If you run a script as:

```bash
./greet.sh Alice
```

the shell passes `Alice` to the script as `$1`. `$0` is the script name, `$#` is the number of arguments, and `$@` is the full list of arguments.

Create `greet-user.sh`:

```bash
#!/bin/bash
# Greet a user passed as an argument

echo "Script name: $0"
echo "You passed $# arguments"

name="$1"
echo "Hello, $name"
```

Run:

```bash
chmod +x greet-user.sh
./greet-user.sh Alice
./greet-user.sh "The Kubernetes Show"
```

Read more about positional parameters in the Bash manual section “Special Parameters”.

---

## 11.5 Exit codes and basic error handling

Every command returns an exit status. Bash stores the exit status of the last command in `$?`. By convention `0` means success and non zero means an error. The Bash manual describes this under “Exit Status”.

Example:

```bash
ls /tmp
echo "ls exit status: $?"

ls /does/not/exist
echo "ls exit status: $?"
```

> **Hands on: check command success**

Create `check-backup.sh`:

```bash
#!/bin/bash
# Check if a backup directory exists

BACKUP_DIR="/var/backups"

if [ -d "$BACKUP_DIR" ]; then
  echo "Backup directory found at $BACKUP_DIR"
  exit 0
else
  echo "Backup directory missing at $BACKUP_DIR"
  exit 1
fi
```

Change `BACKUP_DIR` to an existing directory and a non existing one to see the difference in exit codes:

```bash
./check-backup.sh
echo "Script exit code: $?"
```


---

## 11.6 Conditionals: if and test

The basic `if` syntax in Bash looks like this:

```bash
if condition; then
  commands
elif other_condition; then
  other_commands
else
  fallback_commands
fi
```

The Bash manual covers control structures including `if`, `case`, `for`, and `while` in the “Shell Commands” chapter.

### 11.6.1 File tests

The `test` command (or `[ ... ]` syntax) has many options, such as:

- `-f FILE` true if FILE exists and is a regular file
- `-d DIR` true if DIR exists and is a directory
- `-x FILE` true if FILE exists and is executable

> **Hands on: simple file check**

Create `check-file.sh`:

```bash
#!/bin/bash
# Check if a file exists

FILE="$1"

if [ -f "$FILE" ]; then
  echo "File '$FILE' exists"
else
  echo "File '$FILE' does not exist"
fi
```

Run:

```bash
chmod +x check-file.sh
./check-file.sh /etc/passwd
./check-file.sh /no/such/file
```


---

### 11.6.2 Simple numeric comparison

You can compare numbers using `-eq`, `-ne`, `-lt`, `-gt`, and so on:

```bash
#!/bin/bash

NUMBER="$1"

if [ "$NUMBER" -gt 10 ]; then
  echo "Number is greater than 10"
else
  echo "Number is 10 or less"
fi
```

Try this script with a few values to see how it behaves.

---

## 11.7 Loops: for and while

Loops let you repeat actions. Bash provides `for`, `while`, and `until` loops. The “Looping Constructs” section of the Bash manual covers them in detail.

### 11.7.1 for loop over a list

A `for` loop over a simple list looks like this:

```bash
for ITEM in one two three; do
  echo "Item: $ITEM"
done
```

> **Hands on: ping list script**

Create `ping-hosts.sh`:

```bash
#!/bin/bash
# Ping a list of hosts

for host in 1.1.1.1 8.8.8.8 localhost; do
  echo "Pinging $host"
  ping -c 1 "$host" > /dev/null 2>&1

  if [ "$?" -eq 0 ]; then
    echo "  $host is reachable"
  else
    echo "  $host is NOT reachable"
  fi
done
```

Run it and watch the output. This is a trivial example of combining a loop with an exit code check.

---

### 11.7.2 while loop over lines in a file

A `while` loop is often used to read lines from a file or command:

```bash
num=0
while read line; do
  num=$((num + 1))
  echo "Line $num: $line"
done < somefile.txt
```

> **Hands on: count lines**

Create `count-lines.sh`:

```bash
#!/bin/bash
# Count non empty lines in a file

FILE="$1"
count=0

while read line; do
  if [ -n "$line" ]; then
    count=$((count + 1))
  fi
done < "$FILE"

echo "Non-empty lines in $FILE: $count"
```

Test it:

```bash
chmod +x count-lines.sh
./count-lines.sh /etc/passwd
./count-lines.sh hello.sh
echo "======= './count-lines.sh greeter.sh' output ============"
./count-lines.sh greeter.sh
echo "====== 'wc -l greeter.sh' output ============="
wc -l greeter.sh
echo "====== 'cat -n greeter.sh' output ============="
cat -n greeter.sh
```


---

## 11.8 Reading user input

The `read` builtin lets you accept input from the user. You can read about `read` in the Bash manual under “Bash Builtins”.

Simple example:

```bash
#!/bin/bash

echo -n "What is your name? "
read name
echo -e "\nNice to meet you, $name"
```

> **Hands on: interactive greeter**

Create `ask-name.sh` as above, make it executable, and run it a few times. Try piping something into it:

```bash
echo "Kube Learner" | ./ask-name.sh
```

Notice that it still reads a line and responds.

---

## 11.9 Best practices for beginners

The GNU Bash manual and many distribution guides do not dictate a single style, but a few practices make your scripts easier to maintain:

- Always start with `#!/bin/bash` so you know which shell is used
- Use comments to explain what the script does and what its inputs and outputs are
- Quote variables such as `"$FILE"` to avoid surprises with spaces or special characters
- Check for errors and use meaningful exit codes instead of blindly assuming success
- Keep scripts executable only by users who need them and store them under a sensible directory (for example `~/bin` or `/usr/local/bin`)



The Ubuntu beginners Bash scripting wiki has a short list of similar tips and examples:
https://help.ubuntu.com/community/Beginners/BashScripting

---

## 11.10 Homework

To solidify these basics, complete the following exercises on a test system.

1. **Parameterised greeting**
    - Write a script `hello-user.sh` that:
        - Greets the first argument if provided
        - Otherwise asks the user for their name using `read`
    - Use an exit code of `0` for success and `1` if no name was provided from argument or input
2. **Disk usage reporter**
    - Write `disk-report.sh` that:
        - Runs `df -h` and filters lines for `/dev` devices
        - Prints only the filesystem, size, used percentage, and mountpoint
    - Add an optional argument that specifies a mountpoint to focus on (for example `./disk-report.sh /home`)
3. **Log file scanner**
    - Write `scan-logs.sh` that:
        - Takes a filename as `$1`
        - Counts how many lines contain the word `ERROR` and how many contain `WARNING`
        - Prints a short summary
    - Make sure you handle the case where the file does not exist with a friendly message and non zero exit code
4. **Backup wrapper**
    - Write `simple-backup.sh` that:
        - Takes a source directory and a destination directory as arguments
        - Checks that both exist
        - Uses `tar` to create a timestamped archive in the destination directory
    - Make it print a clear success or error message and exit with `0` or `1` accordingly
5. **Loop practice**
    - Create `list-users.sh` that:
        - Reads `/etc/passwd` line by line
        - Prints the username and shell for each user in a nicely formatted way
    - Try to rewrite it once using a `for` loop and once using a `while` loop, and note which version you find easier to read

If you can write and explain these scripts without looking up syntax every time, you are ready to move on to more advanced topics like functions, arrays, and more robust error handling.
