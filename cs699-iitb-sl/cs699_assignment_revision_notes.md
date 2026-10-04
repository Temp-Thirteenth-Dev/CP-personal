# CS699\_Assignment\_Revision\_Notes

## CS699 Assignment Revision Notes --- Python for Shell/System Tasks

> **Purpose:** Lab-exam + viva preparation\
> **Basis:** The submitted assignment covering Q1--Q9, Python filesystem handling, command-line arguments, system information, login information, and process management.

***

### 0. What this assignment is really testing

The assignment converts tasks that were previously done using **shell scripting** into Python.

The important idea is **not** to memorize nine programs independently. Most questions are combinations of a small number of patterns:

1. **Input / command-line arguments**
2. **Variables**
3. **Conditions (`if`, `elif`, `else`)**
4. **Loops**
5. **Functions**
6. **Filesystem operations**
7. **Reading/writing files**
8. **Directory traversal with `os.walk()`**
9. **Sorting and filtering**
10. **System information**
11. **Running Linux commands from Python**
12. **Error handling**
13. **Formatted output**

The assignment itself explicitly describes Q1 as combining variables, file tests, conditions, traversal, counting and disk usage; Q5 as combining functions, loops and conditionals; and Q6 as command/output-redirection style report generation.

***

## 1. Core Python patterns you must know

### 1.1 Running a Python program

```bash
python3 program.py
```

If the program accepts command-line arguments:

```bash
python3 health_checkup.py dir1 dir2 dir3
```

Arguments are available through:

```python
import sys

print(sys.argv)
```

For:

```bash
python3 test.py abc xyz
```

`sys.argv` is approximately:

```python
["test.py", "abc", "xyz"]
```

Therefore:

```python
sys.argv[0]   # program name
sys.argv[1]   # first user argument
sys.argv[2]   # second user argument
```

#### Very important

```python
for directory in sys.argv[1:]:
```

means:

> Process every command-line argument except the program name.

***

## 2. Conditions and existence checks

### 2.1 Check whether a directory exists

```python
import os

if os.path.isdir("mydir"):
    print("Directory exists")
else:
    print("Directory does not exist")
```

### 2.2 Check whether a file exists

```python
if os.path.isfile("students.csv"):
    print("File exists")
else:
    print("File does not exist")
```

#### Viva question

**Q: Why use `os.path.isfile()` instead of only `os.path.exists()`?**

`exists()` checks whether the path exists, regardless of whether it is a file or directory.

`isfile()` specifically checks that the path exists **and is a regular file**.

Similarly:

```python
os.path.isdir(path)
```

checks specifically for a directory.

***

## 3. `exit()` vs `sys.exit()`

Your Q1/Q2 code uses:

```python
exit(1)
```

A more explicit script-oriented form is:

```python
import sys
sys.exit(1)
```

Conventionally:

```python
sys.exit(0)
```

means successful termination, while a non-zero status such as:

```python
sys.exit(1)
```

indicates an error/failure.

#### Viva

**Q: Why terminate when the input file does not exist?**

Because continuing would cause later file operations to fail or produce meaningless results.

***

## 4. Directory traversal --- `os.walk()`

This is one of the **most important concepts in the assignment**.

```python
for root, dirs, files in os.walk(directory):
    ...
```

For every directory encountered, Python gives:

* `root` → current directory path
* `dirs` → subdirectories inside `root`
* `files` → files inside `root`

Example:

```
project/
├── a.txt
├── src/
│   ├── main.py
│   └── test.py
└── docs/
    └── README.md
```

Then `os.walk("project")` traverses:

```
project
project/src
project/docs
```

and gives the corresponding files at each level.

### 4.1 Counting all files recursively

```python
count = 0

for root, dirs, files in os.walk(directory):
    count += len(files)

print(count)
```

This counts files in the directory **and all nested subdirectories**.

This is the basic pattern used in Q1 and Q5.

***

## 5. `os.path.join()` --- very important

Never manually assume how path separators should be constructed.

Use:

```python
path = os.path.join(root, file)
```

For example:

```python
root = "/home/user/project/src"
file = "main.py"

path = os.path.join(root, file)
```

produces:

```
/home/user/project/src/main.py
```

#### Viva

**Q: Why not simply do `root + "/" + file`?**

`os.path.join()` is the platform-aware and standard way to construct paths. It avoids manually handling path separators.

***

## 6. File size

To get a file's size in bytes:

```python
size = os.path.getsize(path)
```

Therefore:

```python
total_size = 0

for root, dirs, files in os.walk(directory):
    for file in files:
        path = os.path.join(root, file)

        if os.path.isfile(path):
            total_size += os.path.getsize(path)
```

The result is in **bytes**.

***

## 7. Bytes → MB / GB

The assignment uses:

```python
size_mb = total_size / (1024 * 1024)
```

So:

```
1 KB = 1024 bytes
1 MB = 1024² bytes
1 GB = 1024³ bytes
```

For GB:

```python
size_gb = total_size / (1024 ** 3)
```

#### Remember the classification in Q5

```
< 100 MB       → SMALL
100 MB–1 GB    → MEDIUM
> 1 GB         → LARGE
```

A clean implementation is:

```python
if size_mb < 100:
    print("SMALL")
elif size_mb <= 1024:
    print("MEDIUM")
else:
    print("LARGE")
```

#### Boundary question

At exactly:

```
100 MB
```

it is **MEDIUM**.

At exactly:

```
1024 MB = 1 GB
```

it is also **MEDIUM**.

***

## 8. Modification time

The assignment uses:

```python
os.path.getmtime(path)
```

This returns the file's **modification time** as a timestamp.

A common pattern is:

```python
files_with_time.append((modification_time, path))
```

Then:

```python
files_with_time.sort(reverse=True)
```

Because the timestamp is the first tuple element, this sorts by modification time from newest to oldest.

### Why tuples?

Example:

```python
[
    (1700000000, "a.txt"),
    (1700001000, "b.txt"),
    (1699999000, "c.txt")
]
```

Sorting in reverse gives:

```
b.txt
a.txt
c.txt
```

#### Viva

**Q: What does `reverse=True` do?**

It sorts in descending order.

Since larger modification timestamps correspond to later modification times, the newest files appear first.

***

## 9. Sorting in Python

Basic:

```python
numbers.sort()
```

Descending:

```python
numbers.sort(reverse=True)
```

For tuples:

```python
items.sort(reverse=True)
```

By default Python compares tuple elements from left to right.

Example:

```python
data = [
    (50, "A"),
    (100, "B"),
    (20, "C")
]

data.sort(reverse=True)
```

Result:

```
(100, "B")
(50, "A")
(20, "C")
```

***

## 10. Slicing --- especially Q3

Q3 uses:

```python
directories[:10]
```

This means:

> Take the first 10 elements.

After:

```python
directories.sort(reverse=True)
```

the first 10 are the largest.

Therefore:

```python
for size, directory in directories[:10]:
```

prints only the largest ten.

#### Viva

**Q: What if there are only 6 directories?**

`directories[:10]` simply returns all 6. Python does not produce an error.

***

## 11. Human-readable sizes

Q3 converts bytes into:

```
B → K → M → G → T
```

using:

```python
units = ["B", "K", "M", "G", "T"]

while size_hr >= 1024 and unit < len(units) - 1:
    size_hr /= 1024
    unit += 1
```

The idea is:

```
bytes
  ↓ /1024
KB
  ↓ /1024
MB
  ↓ /1024
GB
  ↓ /1024
TB
```

Then:

```python
print(f"{size_hr:.1f}{units[unit]}")
```

`:.1f` means:

> print the floating-point value with one digit after the decimal point.

Example:

```
12.4G
```

***

## 12. Reading a text file

Q2 uses:

```python
with open(filename, "r") as file:
    lines = file.readlines()
```

Important pieces:

* `open()` → opens the file
* `"r"` → read mode
* `with` → automatically closes the file
* `readlines()` → reads all lines into a list

Example:

```
students.csv
```

might become:

```python
[
    "RollNo,Name,Department,Year\n",
    "101,Aarav,CSE,1\n",
    ...
]
```

***

## 13. `read()` vs `readline()` vs `readlines()`

#### `read()`

Reads the whole file as one string.

```python
data = file.read()
```

#### `readline()`

Reads one line.

```python
line = file.readline()
```

#### `readlines()`

Reads all lines and returns a list.

```python
lines = file.readlines()
```

#### Viva

**Q: Why does `len(lines)` give the number of lines?**

Because `lines` is a list in which each element corresponds to one line read from the file.

***

## 14. Searching inside strings

Q2 uses:

```python
if "CSE" in line:
    cse_no += 1
```

This checks whether the substring `"CSE"` occurs anywhere in the line.

Similarly:

```python
if "EE" in line:
    ee_no += 1
```

#### Important limitation

This is a simple solution suitable for the assignment, but it is not a full CSV parser.

For example, robust CSV processing would normally use Python's `csv` module.

For **this assignment**, understand the pattern:

```python
for line in lines:
    if "CSE" in line:
        count += 1
```

***

## 15. Dictionaries for counting --- Q4

Q4 asks for filenames occurring more than once regardless of location.

The core pattern is:

```python
filename_count = {}

for root, dirs, files in os.walk("."):
    for file in files:
        if file in filename_count:
            filename_count[file] += 1
        else:
            filename_count[file] = 1
```

This creates a frequency table.

Example:

```
src/README.md
docs/README.md
backup/README.md
```

produces:

```python
{
    "README.md": 3
}
```

Then:

```python
if filename_count[filename] > 1:
    print(filename)
```

prints duplicate names.

***

## 16. Dictionary pattern to memorize

This pattern appears everywhere in programming:

```python
count = {}

for item in items:
    if item in count:
        count[item] += 1
    else:
        count[item] = 1
```

Meaning:

> Count how many times each item occurs.

An alternative you should recognize is:

```python
count[item] = count.get(item, 0) + 1
```

But for a lab/viva, be comfortable with the explicit `if/else` version too.

***

## 17. Why `sorted(filename_count)`?

Q4 does:

```python
for filename in sorted(filename_count):
```

A dictionary iteration can be used directly, but `sorted()` makes the output alphabetical.

Then:

```python
if filename_count[filename] > 1:
    print(filename)
```

selects only duplicates.

***

## 18. Q1 --- Project Summary

### Requirement

The assignment asks Q1 to:

* store project directory name in a variable
* check whether it exists
* count files
* show total disk usage
* list files sorted by modification time
* print "Analysis Completed"

The assignment explicitly identifies the concepts as variables + file tests + `if` + traversal/counting + disk usage. fileciteturn0file0L4-L20

### Program structure

#### Step 1 --- Input

```python
proj_dir = input("Enter dir name : ")
```

#### Step 2 --- Check directory

```python
if not os.path.isdir(proj_dir):
    print("Proj dir does not exist.")
    exit(1)
```

#### Step 3 --- Count files

```python
file_cnt = 0

for root, dirs, files in os.walk(proj_dir):
    file_cnt += len(files)
```

#### Step 4 --- Calculate total size

```python
total_size = 0

for root, dirs, files in os.walk(proj_dir):
    for file in files:
        path = os.path.join(root, file)

        if os.path.isfile(path):
            total_size += os.path.getsize(path)
```

#### Step 5 --- Collect modification times

```python
files_with_time = []

for root, dirs, files in os.walk(proj_dir):
    for file in files:
        path = os.path.join(root, file)
        modification_time = os.path.getmtime(path)

        files_with_time.append((modification_time, path))
```

#### Step 6 --- Sort

```python
files_with_time.sort(reverse=True)
```

#### Step 7 --- Print

```python
for modification_time, path in files_with_time:
    print(path)
```

***

### Q1 Viva questions

#### Q: Why use `os.walk()`?

Because the project may contain nested directories and we want to process files recursively.

#### Q: What does `root` represent?

The current directory being visited.

#### Q: What does `files` contain?

The names of files directly inside the current `root`.

#### Q: Why use `os.path.join()`?

To construct a valid path from the directory and filename.

#### Q: What unit is returned by `os.path.getsize()`?

Bytes.

#### Q: What does `getmtime()` return?

A modification timestamp.

#### Q: Why store `(time, path)` as a tuple?

So that sorting the list naturally sorts files according to their modification timestamp.

***

## 19. Q2 --- Student Data Analysis

### Requirements

Q2 asks the program to:

* check whether `students.csv` exists
* print an error and terminate if absent
* otherwise print total lines
* count CSE students
* count EE students
* print "Analysis Completed"

These are explicitly listed in the assignment. fileciteturn0file0L21-L37

### Core code pattern

```python
import os

filename = "students.csv"

if not os.path.isfile(filename):
    print("Error: students file not present")
    exit(1)

with open(filename, "r") as file:
    lines = file.readlines()

print("Total lines:", len(lines))

cse_no = 0
for line in lines:
    if "CSE" in line:
        cse_no += 1

ee_no = 0
for line in lines:
    if "EE" in line:
        ee_no += 1

print("CSE:", cse_no)
print("EE:", ee_no)
```

The submitted test data has a header plus 11 data/blank lines, and the recorded output reports 12 total lines, 5 CSE students and 3 EE students. fileciteturn0file0L285-L310

### Important viva point

The program counts **lines containing the department string**, not parsed CSV records.

So if the examiner asks:

> "Is this a proper CSV parser?"

Answer:

> No. This solution treats each line as a string and searches for `"CSE"` or `"EE"`. A proper CSV-based solution could use Python's `csv` module.

***

## 20. Q3 --- Ten Largest Directories

### Requirements

Q3 asks for:

* directories inside the home directory
* the 10 largest
* descending order
* human-readable sizes
* ignore permission errors
* print only 10

The assignment states these requirements directly. fileciteturn0file0L38-L54

### Finding home directory

```python
home_dir = os.path.expanduser("~")
```

`~` represents the user's home directory in a shell-like path.

`expanduser("~")` expands it to the actual path.

***

### Finding directories

```python
for entry in os.scandir(home_dir):
    if entry.is_dir():
        ...
```

`os.scandir()` gives directory entries.

`entry.is_dir()` checks whether the entry is a directory.

***

### Recursively calculating size

```python
total_size = 0

for root, dirs, files in os.walk(entry.path):
    for file in files:
        filepath = os.path.join(root, file)
        total_size += os.path.getsize(filepath)
```

***

### Permission handling

The submitted program catches:

```python
except (PermissionError, FileNotFoundError):
    continue
```

Meaning:

* if permission is denied → skip that file
* if the file disappeared between traversal and size calculation → skip it

This is useful because filesystem contents can change while a program is running.

***

### Sorting

```python
directories.sort(reverse=True)
```

Each element is:

```python
(total_size, entry.path)
```

Therefore descending sorting puts largest sizes first.

***

### Top 10

```python
directories[:10]
```

***

### Q3 Viva

**Q: Why do we catch `FileNotFoundError` even though we are traversing existing files?**

Because a file may be deleted or moved after `os.walk()` finds it but before `getsize()` accesses it.

**Q: Why catch `PermissionError`?**

Some directories/files may not be readable by the current user.

**Q: Why sort before slicing?**

If we slice before sorting, we would get the first ten arbitrary entries rather than the ten largest.

***

## 21. Q4 --- Duplicate File Names

### Problem idea

Suppose:

```
src/main.py
docs/main.py
src/README.md
docs/README.md
backup/README.md
```

The task is to print:

```
README.md
main.py
```

The **paths do not matter**. Only the filename matters.

***

### Python solution pattern

```python
filename_count = {}

for root, dirs, files in os.walk("."):
    for file in files:
        if file in filename_count:
            filename_count[file] += 1
        else:
            filename_count[file] = 1

for filename in sorted(filename_count):
    if filename_count[filename] > 1:
        print(filename)
```

***

### Shell equivalent from the assignment

The submitted assignment also records the shell idea:

```bash
find . -type f -printf "%f\n" 2>/dev/null | sort | uniq -d
```

Understand the pipeline:

```
find
  ↓
print only filenames
  ↓
sort
  ↓
uniq -d
  ↓
duplicate names
```

The assignment's test project contains multiple `README.md` files and multiple `main.py` files, and the recorded output is `main.py` and `README.md`. fileciteturn0file0L363-L408

***

## 22. Q5 --- Directory Health Check

This is one of the **highest-priority questions for the lab exam** because the assignment explicitly describes it as combining almost the entire lecture.

Requirements:

* multiple directory names as input
* verify existence
* count files
* display disk usage
* classify:
  * Small `<100 MB`
  * Medium `100 MB–1 GB`
  * Large `>1 GB`
* use functions
* use loops
* use if/else

fileciteturn0file0L60-L88

***

### Why use a function?

The same operation must be performed for multiple directories.

Instead of writing:

```python
# code for directory 1
# code for directory 2
# code for directory 3
```

write:

```python
def analyze(directory):
    ...
```

and call:

```python
for directory in sys.argv[1:]:
    analyze(directory)
```

This is a classic **function + loop + command-line arguments** pattern.

***

### Function skeleton

```python
def analyze(directory):

    if os.path.isdir(directory):
        ...
    else:
        print("Directory not found")
```

***

### Count files and calculate size together

```python
no_of_files = 0
total_size = 0

for root, dirs, files in os.walk(directory):
    no_of_files += len(files)

    for file in files:
        filepath = os.path.join(root, file)

        try:
            total_size += os.path.getsize(filepath)
        except (PermissionError, FileNotFoundError):
            continue
```

***

### Classification

```python
size_mb = total_size / (1024 * 1024)

if size_mb < 100:
    print("Category: SMALL")
elif size_mb <= 1024:
    print("Category: MEDIUM")
else:
    print("Category: LARGE")
```

***

### Complete mental template

If you see:

> "For every directory passed as an argument, check it, traverse it, count files, calculate size, classify it."

Immediately think:

```
sys.argv
   ↓
for each directory
   ↓
function
   ↓
isdir()
   ↓
os.walk()
   ↓
count files
   ↓
getsize()
   ↓
bytes → MB
   ↓
if / elif / else
```

That pattern is extremely likely to be useful in a lab test.

***

## 23. Q6 --- System Information Report

Q6 asks for:

* date/time
* logged-in user
* hostname
* current working directory
* disk space
* memory
* uptime
* save/report the information

fileciteturn0file0L89-L109

The submitted solution uses several Python modules.

***

### 23.1 Date and time

```python
import datetime

print(datetime.datetime.now())
```

`datetime.datetime.now()` gives the current local date/time.

***

### 23.2 Logged-in user

```python
import getpass

print(getpass.getuser())
```

***

### 23.3 Hostname

```python
import socket

print(socket.gethostname())
```

***

### 23.4 Current working directory

```python
import os

print(os.getcwd())
```

#### Viva

**Q: What is the difference between current working directory and home directory?**

* `os.getcwd()` → directory from which the program is currently operating.
* `os.path.expanduser("~")` → user's home directory.

They are not necessarily the same.

***

## 24. Disk usage with `shutil.disk_usage()`

The submitted program uses:

```python
import shutil

total, used, free = shutil.disk_usage("/")
```

The values are in bytes.

Then:

```python
total / (1024**3)
```

converts bytes to GB.

The three returned values are:

```
total → total filesystem space
used  → used space
free  → free/available space
```

***

## 25. Memory information through `/proc`

The submitted Linux-oriented solution reads:

```python
with open("/proc/meminfo", "r") as file:
    memory_info = file.readlines()
```

Then:

```python
for line in memory_info[:3]:
    print(line.strip())
```

This uses the Linux `/proc` pseudo-filesystem.

#### Viva

**Q: Is `/proc/meminfo` a normal file stored on disk?**

No. `/proc` is a virtual/pseudo-filesystem provided by the Linux kernel. It exposes system/process information.

***

## 26. Uptime through `/proc/uptime`

The submitted program does:

```python
with open("/proc/uptime", "r") as file:
    uptime_seconds = float(file.read().split()[0])
```

Suppose the first value is:

```
123456.78
```

It represents uptime in seconds.

Then:

```python
days = int(uptime_seconds // 86400)
hours = int((uptime_seconds % 86400) // 3600)
minutes = int((uptime_seconds % 3600) // 60)
```

Remember:

```
1 minute = 60 seconds
1 hour   = 3600 seconds
1 day    = 86400 seconds
```

***

## 27. Q7/Q8 --- User Login Report

Q7/Q8 are essentially the same task in the submitted assignment.

Requirements include:

* currently logged-in users
* login times
* total number logged in
* check whether a specified user is logged in
* last 10 logins
* unique users
* most recent logins
* save to a file

fileciteturn0file0L110-L130

***

### Command-line argument

The submitted program starts with:

```python
if len(sys.argv) < 2:
    print("Usage: python3 usr_login_rep.py <username>")
    sys.exit(1)

specified_user = sys.argv[1]
```

This is important.

If the examiner runs:

```bash
python3 usr_login_rep.py vamsi
```

then:

```python
sys.argv[0] = "usr_login_rep.py"
sys.argv[1] = "vamsi"
```

***

## 28. `subprocess.run()`

The submitted solution uses:

```python
result = subprocess.run(
    ["who"],
    capture_output=True,
    text=True
)
```

This runs the Linux command:

```bash
who
```

from Python.

***

### Important arguments

#### `["who"]`

A list representing the command and its arguments.

For:

```bash
who -u
```

use:

```python
["who", "-u"]
```

#### `capture_output=True`

Capture the command's standard output rather than simply letting it print directly.

#### `text=True`

Return captured output as a string rather than bytes.

Then:

```python
result.stdout
```

contains the command's output.

***

## 29. `who`

The Linux command:

```bash
who
```

shows users currently logged in.

The assignment uses:

```python
subprocess.run(["who"], ...)
```

***

## 30. `who -u`

The submitted program uses:

```python
subprocess.run(["who", "-u"], ...)
```

This provides more detailed information about logged-in users, including login/session details.

***

## 31. Counting logged-in users

The assignment does:

```python
logged_in_users = len(result.stdout.strip().splitlines())
```

Understand the chain:

```
result.stdout
     ↓
strip()
     ↓
splitlines()
     ↓
list of lines
     ↓
len()
     ↓
number of lines
```

Example:

```python
output = "user1 ...\nuser2 ...\n"
```

Then:

```python
output.strip()
```

removes surrounding whitespace.

```python
output.splitlines()
```

becomes:

```python
["user1 ...", "user2 ..."]
```

and:

```python
len(...)
```

is:

```
2
```

***

## 32. Checking whether a specified user is logged in

The submitted pattern is:

```python
logged_in = False

for line in result.stdout.splitlines():
    username = line.split()[0]

    if username == specified_user:
        logged_in = True
        break
```

Then:

```python
if logged_in:
    ...
else:
    ...
```

This is a very useful generic pattern:

```
flag = False

for item in collection:
    if condition:
        flag = True
        break
```

***

## 33. `split()` vs `splitlines()`

#### `split()`

Splits a string on whitespace.

```python
line.split()
```

For:

```
vamsi tty2 2026-08-31
```

you get approximately:

```python
["vamsi", "tty2", "2026-08-31"]
```

Therefore:

```python
line.split()[0]
```

gets the first whitespace-separated field.

#### `splitlines()`

Splits a multi-line string into separate lines.

***

## 34. Output files

The submitted login report uses:

```python
with open("user_login_report.txt", "w") as report:
```

`"w"` means write mode.

Important modes:

```
"r" → read
"w" → write / overwrite
"a" → append
```

#### Viva

**Q: What happens if the report already exists and you use `"w"`?**

Its old contents are overwritten.

If you want to add to the end instead:

```python
open("file.txt", "a")
```

***

## 35. Q9 --- Process Status and Management

Q9 asks the program to accept a process name and:

* check whether it is running
* display PID
* CPU usage
* memory usage
* print suitable message if not found
* save report to a text file

The assignment identifies the concepts as arguments + process commands + conditionals + formatted output. fileciteturn0file0L131-L147

A key point for revision:

> **The submitted file content shown for this assignment does not include the Q9 implementation itself.**

So do not memorize a nonexistent Q9 program from this submission. The requirements are present, but the actual implementation is not included in the supplied assignment text.

***

## 36. Error handling

Q3/Q5 use:

```python
try:
    ...
except (PermissionError, FileNotFoundError):
    continue
```

Understand the logic:

```
try:
    risky filesystem operation
       ↓
if successful → continue normally
       ↓
if PermissionError/FileNotFoundError
       ↓
skip the problematic item
```

#### Why is this important?

Filesystem operations are not guaranteed to succeed.

A file can:

* be inaccessible
* disappear during traversal
* be changed while the program is running

***

## 37. `try/except` viva questions

#### Q: What is an exception?

An exception is an error/event raised during program execution that interrupts the normal flow unless handled.

#### Q: Why not let the program crash?

Because one inaccessible/missing file should not necessarily stop analysis of all other files.

#### Q: What does `continue` do inside the `except`?

It skips the current iteration and moves to the next iteration of the loop.

***

## 38. `with open(...)` --- must know

Pattern:

```python
with open("file.txt", "r") as file:
    data = file.read()
```

The `with` statement manages the file resource and ensures the file is closed when the block ends.

Use this rather than:

```python
file = open(...)
...
file.close()
```

unless there is a specific reason.

***

## 39. Important modules from this assignment

Module Main use in assignment

***

`os` files, directories, traversal, paths, current directory `sys` command-line arguments and exiting `shutil` disk usage `getpass` current/logged-in username `socket` hostname `datetime` date/time `subprocess` execute Linux commands `/proc` Linux system information

Memorize the **purpose**, not just the import statement.

***

## 40. High-value `os` functions

### `os.getcwd()`

Current working directory.

```python
os.getcwd()
```

### `os.path.isdir(path)`

Is it a directory?

```python
os.path.isdir(path)
```

### `os.path.isfile(path)`

Is it a regular file?

```python
os.path.isfile(path)
```

### `os.path.join(a, b)`

Build a path.

```python
os.path.join(a, b)
```

### `os.path.getsize(path)`

File size in bytes.

### `os.path.getmtime(path)`

Modification timestamp.

### `os.path.expanduser("~")`

Expand home directory.

### `os.walk(path)`

Recursively traverse directory tree.

### `os.scandir(path)`

Iterate over entries in a directory.

***

## 41. High-value `sys` functions

### Command-line arguments

```python
sys.argv
```

### Exit

```python
sys.exit(1)
```

Remember:

```python
sys.argv[0]
```

is the program name.

***

## 42. `os.walk()` vs `os.scandir()`

This is a likely viva question.

#### `os.walk()`

Used for recursive traversal.

```python
for root, dirs, files in os.walk(path):
    ...
```

It automatically goes through nested directories.

#### `os.scandir()`

Lists entries in one directory.

```python
for entry in os.scandir(path):
    ...
```

It does **not by itself recursively traverse the entire tree**.

In Q3:

```python
os.scandir(home_dir)
```

is used to identify directories directly inside home.

Then:

```python
os.walk(entry.path)
```

recursively calculates the size of each directory.

***

## 43. Common examiner traps

### Trap 1 --- `getsize()` of a directory

Do not assume:

```python
os.path.getsize(directory)
```

means the total size of everything inside it.

For recursive directory size, traverse the directory and sum file sizes.

***

### Trap 2 --- `len(os.listdir())`

This is not the same as recursive file count.

```python
len(os.listdir(path))
```

counts entries directly inside the directory, including directories.

For recursive file counting:

```python
for root, dirs, files in os.walk(path):
    count += len(files)
```

***

### Trap 3 --- `sys.argv`

If the command is:

```bash
python3 health_checkup.py dir1 dir2
```

there are three `argv` elements:

```
argv[0] = health_checkup.py
argv[1] = dir1
argv[2] = dir2
```

Therefore:

```python
sys.argv[1:]
```

contains user-provided directory arguments.

***

### Trap 4 --- `"w"` mode

```python
open("report.txt", "w")
```

overwrites an existing report.

***

### Trap 5 --- bytes vs MB

`os.path.getsize()` gives bytes.

Do not compare raw bytes directly with `100` if the requirement is 100 MB.

Use:

```python
size_mb = total_size / (1024 * 1024)
```

***

### Trap 6 --- sorting tuples

If:

```python
data.append((size, path))
```

then:

```python
data.sort(reverse=True)
```

sorts primarily by `size`.

***

## 44. Patterns to memorize for the lab

### Pattern A --- Check and terminate

```python
if not os.path.isfile(filename):
    print("File not found")
    sys.exit(1)
```

***

### Pattern B --- Recursive file count

```python
count = 0

for root, dirs, files in os.walk(path):
    count += len(files)
```

***

### Pattern C --- Recursive total size

```python
total = 0

for root, dirs, files in os.walk(path):
    for file in files:
        filepath = os.path.join(root, file)

        try:
            total += os.path.getsize(filepath)
        except (PermissionError, FileNotFoundError):
            continue
```

***

### Pattern D --- Command-line arguments

```python
import sys

for arg in sys.argv[1:]:
    print(arg)
```

***

### Pattern E --- Function + arguments

```python
def analyze(path):
    ...

for path in sys.argv[1:]:
    analyze(path)
```

***

### Pattern F --- Count occurrences

```python
count = {}

for item in items:
    if item in count:
        count[item] += 1
    else:
        count[item] = 1
```

***

### Pattern G --- Find duplicates

```python
for item in sorted(count):
    if count[item] > 1:
        print(item)
```

***

### Pattern H --- Sort largest first

```python
items.sort(reverse=True)
```

***

### Pattern I --- Top K

```python
items[:k]
```

***

### Pattern J --- Read file line by line

```python
with open(filename, "r") as file:
    for line in file:
        ...
```

This is often preferable to loading the entire file when the file can be large.

***

### Pattern K --- Write report

```python
with open("report.txt", "w") as report:
    report.write("Hello\n")
```

***

### Pattern L --- Run a Linux command

```python
import subprocess

result = subprocess.run(
    ["who"],
    capture_output=True,
    text=True
)

print(result.stdout)
```

***

## 45. How to reconstruct a program in the lab

If you forget the exact code, don't panic.

Use this five-step process.

### Step 1 --- Identify the input

Ask:

> Is the input coming from `input()` or `sys.argv`?

Examples:

```python
name = input(...)
```

or:

```python
sys.argv[1:]
```

***

### Step 2 --- Identify the object being processed

Is it:

* file?
* directory?
* list of files?
* command output?
* process?
* system information?

***

### Step 3 --- Choose the core operation

Requirement Think of

***

file exists `os.path.isfile()` directory exists `os.path.isdir()` recursively visit files `os.walk()` file size `os.path.getsize()` modification time `os.path.getmtime()` build path `os.path.join()` count variable + loop duplicates dictionary largest sort descending first 10 `[:10]` system command `subprocess.run()` current directory `os.getcwd()` hostname `socket.gethostname()`

***

### Step 4 --- Add error handling

Filesystem questions often need:

```python
try:
    ...
except (PermissionError, FileNotFoundError):
    continue
```

***

### Step 5 --- Format the output

Only after the logic works should you worry about:

```python
f"{value:.2f}"
```

or headings/report formatting.

***

## 46. Likely viva questions --- rapid revision

### Python basics

**Q1. What is `sys.argv`?**\
A list containing command-line arguments passed to the Python program.

**Q2. What is `sys.argv[0]`?**\
Usually the script/program name.

**Q3. What is `sys.argv[1:]`?**\
All user-provided arguments after the program name.

**Q4. Difference between `input()` and `sys.argv`?**\
`input()` reads interactively while the program is running; `sys.argv` receives arguments from the command line when the program is invoked.

**Q5. Why use functions?**\
To encapsulate reusable logic and avoid repeating code.

***

### Files/directories

**Q6. `isfile()` vs `isdir()`?**\
One checks regular files; the other checks directories.

**Q7. What does `os.walk()` do?**\
Recursively traverses a directory tree.

**Q8. What are `root`, `dirs`, and `files` in `os.walk()`?**\
Current directory path, subdirectory names, and file names.

**Q9. What does `getsize()` return?**\
File size in bytes.

**Q10. What does `getmtime()` return?**\
Modification timestamp.

**Q11. Why `os.path.join()`?**\
To construct paths correctly.

***

### Lists/dictionaries

**Q12. Why use a dictionary in Q4?**\
To maintain a frequency/count for every filename.

**Q13. Why `> 1`?**\
A filename appearing more than once is a duplicate.

**Q14. Why sort before selecting the top 10?**\
To ensure the selected entries are actually the largest.

**Q15. What does `[:10]` mean?**\
The first ten elements.

***

### Exceptions

**Q16. Why handle `PermissionError`?**\
Some files/directories may not be accessible.

**Q17. Why handle `FileNotFoundError` during traversal?**\
A file may disappear after it was discovered.

**Q18. What does `continue` do?**\
Skips the current loop iteration.

***

### Linux/system

**Q19. What does `subprocess.run()` do?**\
Runs an external command/program from Python.

**Q20. Why use a list such as `["who", "-u"]`?**\
It separates the executable/command from its arguments.

**Q21. What does `capture_output=True` do?**\
Captures stdout/stderr instead of directly sending the output to the terminal.

**Q22. What does `text=True` do?**\
Makes captured standard streams strings rather than byte sequences.

**Q23. What is `/proc`?**\
A Linux virtual filesystem exposing kernel/system/process information.

**Q24. What is `/proc/meminfo`?**\
A virtual file containing memory-related information.

**Q25. What is `/proc/uptime`?**\
A virtual file containing system uptime information.

***

## 47. Code-reading questions you should be able to answer

### What does this do?

```python
for root, dirs, files in os.walk(path):
    for file in files:
        filepath = os.path.join(root, file)
```

Answer:

> Recursively visits the directory tree and constructs the full path for every file encountered.

***

### What does this do?

```python
directories.sort(reverse=True)
for size, directory in directories[:10]:
```

Answer:

> Sorts directory records in descending order and processes only the first ten, which are the largest.

***

### What does this do?

```python
username = line.split()[0]
```

Answer:

> Splits the line on whitespace and takes the first field, which in the `who` output is the username.

***

### What does this do?

```python
logged_in = False
...
if username == specified_user:
    logged_in = True
    break
```

Answer:

> Uses a Boolean flag to remember whether the specified user was found and stops searching once a match is found.

***

## 48. Important differences

Concept Meaning

***

`os.path.exists()` path exists `os.path.isfile()` path is a file `os.path.isdir()` path is a directory `os.getcwd()` current working directory `os.path.expanduser("~")` home directory `os.walk()` recursive traversal `os.scandir()` directory entries `getsize()` file size `getmtime()` modification timestamp `read()` entire file as string `readline()` one line `readlines()` list of lines `split()` split by whitespace `splitlines()` split into lines `sort()` sort list in place `sorted()` return sorted result `"r"` read `"w"` overwrite/write `"a"` append

***

## 49. What you should be able to write from memory

Before the lab exam, make sure you can write these **without looking them up**:

#### Filesystem

```python
import os

os.path.isfile(path)
os.path.isdir(path)
os.path.join(a, b)
os.path.getsize(path)
os.path.getmtime(path)

for root, dirs, files in os.walk(path):
    ...
```

#### Arguments

```python
import sys

sys.argv
sys.argv[1:]
sys.exit(1)
```

#### File I/O

```python
with open(filename, "r") as f:
    ...
```

```python
with open(filename, "w") as f:
    ...
```

#### Dictionary counting

```python
count = {}

for item in items:
    if item in count:
        count[item] += 1
    else:
        count[item] = 1
```

#### Sorting

```python
items.sort(reverse=True)
```

#### Exceptions

```python
try:
    ...
except (PermissionError, FileNotFoundError):
    continue
```

#### Subprocess

```python
result = subprocess.run(
    ["command", "argument"],
    capture_output=True,
    text=True
)

print(result.stdout)
```

***

## 50. Last-minute lab exam checklist

### Before starting

* [ ] Read the exact output requirement.
* [ ] Identify whether input is `input()` or `sys.argv`.
* [ ] Identify file vs directory.
* [ ] Decide whether recursion is required.
* [ ] Decide whether errors should be skipped or terminate the program.

### While coding

* [ ] Import required modules.
* [ ] Check input validity.
* [ ] Use `os.walk()` for recursive directory tasks.
* [ ] Use `os.path.join()` for paths.
* [ ] Use `try/except` for potentially inaccessible files.
* [ ] Use functions when multiple inputs require the same operation.
* [ ] Convert bytes to MB/GB before threshold comparisons.
* [ ] Sort before selecting top K.
* [ ] Use `sys.argv[1:]`, not `sys.argv`, when processing user arguments.

### Before submitting

* [ ] Test a valid directory.
* [ ] Test a nonexistent file/directory.
* [ ] Test multiple directories.
* [ ] Test an empty directory if relevant.
* [ ] Test a duplicate filename case.
* [ ] Test a user that is not logged in.
* [ ] Check whether the output file was created.
* [ ] Check that `"w"` did not unexpectedly overwrite something important.

***

## 51. One-page mental map

```
                 PYTHON SYSTEM / SHELL-STYLE LAB
                              |
          +-------------------+-------------------+
          |                   |                   |
      FILESYSTEM           SYSTEM              COMMANDS
          |                   |                   |
      os.path.*           shutil              subprocess
      os.walk()           getpass             who
      os.scandir()        socket              who -u
          |               datetime            last
          |
     +----+----+
     |         |
   files     dirs
     |         |
 getsize()   isdir()
 getmtime()
 isfile()

              DATA PROCESSING
                    |
          +---------+---------+
          |         |         |
        loops   dictionaries  sorting
          |         |         |
       counting   frequency   reverse=True
                             [:10]

              INPUT / OUTPUT
                    |
          +---------+---------+
          |                   |
       sys.argv            open()
          |               /     \
      [1:]              "r"     "w"

              ERROR HANDLING
                    |
                 try/except
                    |
        PermissionError / FileNotFoundError
```

***

## 52. Priority order for revision

If you have limited time, revise in this order:

#### 🔴 Highest priority

1. `os.walk()`
2. `os.path.isfile()` / `isdir()`
3. `os.path.join()`
4. `os.path.getsize()`
5. `sys.argv`
6. functions + loops
7. dictionary counting
8. sorting + `[:10]`
9. `try/except`
10. file reading/writing

#### 🟠 Next

11. `subprocess.run()`
12. `who` / `who -u`
13. `/proc/meminfo`
14. `/proc/uptime`
15. `shutil.disk_usage()`

#### 🟢 Then

16. `getpass.getuser()`
17. `socket.gethostname()`
18. `datetime.datetime.now()`
19. formatting with f-strings

***

## 53. Final exam strategy

The most useful way to prepare is to practice **reconstructing**, not memorizing.

For each question, hide the submitted code and try to write the skeleton from the requirement.

For example:

> "Accept multiple directories, count files, calculate size, classify them."

Your brain should immediately produce:

```python
import os
import sys

def analyze(directory):
    if not os.path.isdir(directory):
        ...
        return

    count = 0
    size = 0

    for root, dirs, files in os.walk(directory):
        count += len(files)

        for file in files:
            path = os.path.join(root, file)
            try:
                size += os.path.getsize(path)
            except (PermissionError, FileNotFoundError):
                continue

    size_mb = size / (1024 * 1024)

    if size_mb < 100:
        ...
    elif size_mb <= 1024:
        ...
    else:
        ...

for directory in sys.argv[1:]:
    analyze(directory)
```

That is the level of reconstruction you should aim for.

> **Do not memorize every print statement. Memorize the problem-solving pattern and the purpose of each Python/Linux operation.**

***

## 54. Ultra-short viva sheet

```
os.walk()             → recursive directory traversal
os.scandir()          → entries of a directory
isfile()              → regular file?
isdir()               → directory?
join()                → construct path
getsize()             → bytes
getmtime()            → modification timestamp
getcwd()              → current directory
expanduser("~")       → home directory

sys.argv              → command-line arguments
sys.argv[0]           → program name
sys.argv[1:]          → user arguments
sys.exit(1)           → terminate with error

open("x","r")         → read
open("x","w")         → overwrite/write
open("x","a")         → append

read()                → whole file string
readline()            → one line
readlines()           → list of lines

split()               → whitespace fields
splitlines()          → lines

dict                  → frequency counting
sort(reverse=True)    → descending
[:10]                 → first 10

try/except             → handle runtime errors
PermissionError       → access denied
FileNotFoundError     → path disappeared/not found

subprocess.run()      → run external command
capture_output=True   → capture output
text=True             → output as string

shutil.disk_usage()   → disk statistics
getpass.getuser()     → username
socket.gethostname()  → hostname
/proc/meminfo         → Linux memory info
/proc/uptime          → Linux uptime
```

***

### Source coverage note

These notes cover the submitted assignment's Q1--Q9 requirements and the implementations actually present in the supplied file. The supplied assignment includes Q9's requirements but does **not** include its implementation, so Q9 has intentionally not been fabricated here. The assignment's directory structure and submitted source files are documented in the provided material. fileciteturn0file0L150-L182
