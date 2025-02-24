 
## I. Unix Shell

The Unix shell is both a **command-line interface** (CLI) and a **scripting environment**.

### 1. Shell Commands

- Arguments can be optional or mandatory.
- Options: short (single dash) or long (double dashes).
- Commands return a code: `0` if successful.
- Use `man <command>` for command documentation.

### 2. Locating Commands

- Unix must locate a command before executing it.
- `./main` specifies that the executable is in the current directory.
- Modify your `PATH` to include directories where commands are located: `PATH=yourDir:$PATH`.
- See the current `PATH` with `echo $PATH`. To permanently modify it, edit `~/.bashrc`.

### 3. Error Handling

- Common error messages:
    - `No such file or directory`: ENOENT (Error NO ENTity).
    - Standard streams:
        - `0`: stdin (input)
        - `1`: stdout (output)
        - `2`: stderr (errors)
- Redirect:
    - Output: `command > filename` (overwrite), `>>` (append).
    - Errors: `command 2> filename`.
    - Both: `command &> filename`.
    - Discard errors: `command 2> /dev/null`.
- Use pipes `|` to pass the output of one command as input to another.

### 4. Running Commands as Superuser

- Use `sudo` for superuser access. Ensure the user is listed in `/etc/sudoers` using `visudo`.

### 5. Command Substitution

- Use backquotes or `$()` to use a command’s output as input for another command or a variable.

### 6. Subshells

- Subshells inherit the parent shell’s environment but changes made inside a subshell do not affect the parent.
- To temporarily alter environment variables: `PATH=/new_path:PATH my_program`.

### 7. Command-Line Editing

|Keystroke|Action|
|---|---|
|`CTRL-B`|Move cursor left|
|`CTRL-F`|Move cursor right|
|`CTRL-A`|Move to the start of the line|
|`CTRL-E`|Move to the end of the line|
|`CTRL-W`|Erase preceding word|
|`CTRL-U`|Erase to the beginning of the line|
|`CTRL-K`|Erase to the end of the line|
|`CTRL-Y`|Paste erased text|

---

## II. Scripting in the Shell

### 1. Bash Types

- **Interactive**: Commands are run with user input.
- **Non-interactive**: Typically used for automation.
- **Login**: Run as part of the user login process.
- **Non-login**: Shells run after login.

### 2. Script Execution

- Make a script executable: `chmod u+rx scriptname`.
- Run the script: `./scriptname`.
- Make a script globally available by moving it to `/usr/local/bin` and ensuring it's in your `PATH`.

### 3. The Shebang (`#!`)

- The first line of a script begins with `#!` followed by the interpreter's path: `#!/bin/bash`.
- It indicates that the file is a script to be executed by the interpreter.

### 4. Debugging Scripts

- Add `#!/bin/bash -x` at the beginning of a script to enable debug mode.

### 5. Variables in Bash

- **Untyped**: No need to declare variables.
- **Variable substitution**: Reference variable values using `$variable` or `${variable}`.
- **Environment variables**: Global and UPPERCASE.
- **Shell variables**: Local and lowercase.

Example:
```
var1=1
var2=2
var3=$var1$var2  # Result: "12" (concatenation)
```
- Avoid spaces around '=' when assigning values.

### 6. Quoting

- **Weak quoting** (`" "`): Allows variable substitution.
- **Full quoting** (`' '`): Prevents substitution.

### 7. Special Variables

|Variable|Meaning|
|---|---|
|`$1`, `$2`|Script arguments|
|`$0`|Script name|
|`$#`|Number of arguments|
|`$@`|All arguments|
|`$$`|Shell's process ID|
|`$?`|Exit code of the last command|

### 8. Conditionals

- Use `if`, `then`, `else`, `fi`.
- Format: `if [ condition ]; then <commands>; else <commands>; fi`.

### 9. Logical Constructs

- `&&`: Logical AND.
- `||`: Logical OR.
- `!`: Logical NOT.

### 10. Logical Conditions

|File Tests|Permissions|String Tests|Arithmetic Tests|
|---|---|---|---|
|`-f` regular file|`-r` readable|`=` equal|`-eq` equal to|
|`-d` directory|`-w` writable|`!=` not equal|`-ne` not equal|
|`-e` file exists|`-x` executable|`-z` empty|`-lt` less than|

### 11. Bash Arithmetic Expansion

- Use `expr` for basic arithmetic:
```
z=1
z=`expr $z + 3`
echo $z  # Result: 4

```
- Use double parentheses for arithmetic expansion:
```
((z+=1))
echo $z  # Result: 1
```
### 12. Arrays in Bash

- Initialize arrays:
```
myArray=(1 2 3)
myArray=(1 2 "three" 4 "five" 6)

```
- Access elements: `${arr[1]}` (second element).

### 13. Useful Array Operations

|Operation|Example|
|---|---|
|Create empty array|`arr=()`|
|Initialize array|`arr=(1 2 3)`|
|Retrieve all elements|`${arr[@]}`|
|Retrieve array size|`${#arr[@]}`|
|Append values|`arr+=(4)`|

---

## III. Important Shell Commands

### 1. Displaying Content

- `echo`: Displays a line of text.
- `cat`, `bat`: Concatenate and display file contents.
- `more`, `less`: Paginate the output of long files.
- `head`: Display the first few lines of a file.
- `tail`: Display the last few lines of a file.
- `sort`: Sort the lines of a file.

### 2. File and Directory Management

- `touch`: Create a new, empty file.
- `mkdir`: Create a new directory.
- `cp`: Copy files or directories.
- `mv`: Move or rename files or directories.
- `rm`: Remove files or directories.
- `rmdir`: Remove empty directories.
- `file`: Determine the type of a file (binary, text, etc.).

### 3. Directory Navigation

- `ls`: List directory contents.
- `cd`: Change the current working directory.
- `pwd`: Print the current working directory.

### 4. Permissions Management

- `chmod`: Change file or directory permissions.
### 5. File Management Utilities

- `basename`: Strip directory and suffix from filenames.
    - Example: `basename /path/to/file.txt` returns `file.txt`.
- `mktemp`: Create temporary files and directories.
    - Example: `mktemp /tmp/tempfile.XXXXXX` creates a temporary file with a unique name.

### 6. Text Manipulation

- `tr`: Translate or delete characters.
    - Replace characters: `echo 'Hello!' | tr le 13` → `H311o`.
    - Delete digits: `echo 'abc123' | tr -d '0-9'` → `abc`.
    - Squeeze repeating characters: `echo 'Hello World' | tr -s ' '` → `Hello World`.
#### Example : 
`cat /dev/random | tr -cd '[:print:]' | fold -w 12 | head -n 1`
this generates a random 12 character password
### 7. File Archiving

- `tar`: Archive multiple files into a single file.
    - Create archive: `tar cf archive.tar file1 file2 ...`.
    - Unpack archive: `tar xf archive.tar`.
    - View contents of archive: `tar tf archive.tar`.
- Compressed archives (`.tar.gz`):
    - Unpack compressed archive: `tar zxf file.tar.gz`.
    - `xf` to extract file
    - `z` to uncompress
### 8. Running Commands on Multiple Files

- `xargs`: Run commands on multiple files provided as input.
    - Example: `find . -name '*.txt' | xargs rm` removes all `.txt` files.
    - To handle filenames with spaces: `find . -name '*.txt' -print0 | xargs -0 rm`.

 
 Install the **Ranger** package for file management: `sudo apt install ranger`.

### 9. Exhaustive list of bash special characters 
| Special Character | Meaning/Function                                                                  |
| ----------------- | --------------------------------------------------------------------------------- |
| `#`               | Comment indicator, everything after is ignored                                    |
| `$`               | Variable or command substitution                                                  |
| `&`               | Run a command in the background                                                   |
| `\|`              | Pipe, passes the output of one command to another<br>                             |
| `;`               | Command separator, allows multiple commands on a single line                      |
| `*`               | Wildcard, matches zero or more characters                                         |
| `?`               | Matches a single character                                                        |
| `~`               | Home directory of the current user                                                |
| `.`               | Current directory (e.g., `./command`)                                             |
| `..`              | Parent directory                                                                  |
| `"`               | Double quotes, prevents some interpretation, allows variable/command substitution |
| `'`               | Single quotes, prevents interpretation of special characters                      |
| `` ` ``           | Backticks, used for command substitution (deprecated, use `$(command)`)           |
| `\`               | Escape character, prevents special interpretation of next character               |
| `/`               | Directory separator in paths                                                      |
| `>`               | Redirects output to a file (overwrites)                                           |
| `>>`              | Redirects output to a file (appends)                                              |
| `<`               | Redirects input from a file                                                       |
| `2>`              | Redirects stderr to a file                                                        |
| `&>`              | Redirects both stdout and stderr to a file                                        |
| `&&`              | Logical AND, executes next command if previous succeeds                           |
| `\|\|`            | Logical OR, executes next command if previous fails                               |
| `()`              | Command grouping in a subshell                                                    |
| `{}`              | Command grouping in the current shell                                             |
| `[]`              | Used in conditionals (e.g., `[ condition ]` for `test`)                           |
| `!`               | Logical negation                                                                  |
| `-`               | Commonly used for options (e.g., `ls -l`)                                         |
| `:`               | No-op (does nothing)                                                              |
| `@`               | Represents all positional parameters or array elements                            |
| `#`               | Length of a string or array                                                       |
| `${}`             | Parameter expansion (e.g., `${variable}`)                                         |
| `$#`              | Number of arguments passed to a script                                            |
| `$?`              | Return code of the last executed command                                          |
| `$$`              | Process ID of the current shell                                                   |
| `$!`              | Process ID of the last background process                                         |
| `*`               | Matches any string of characters                                                  |
| `?`               | Matches any single character                                                      |
| `[]`              | Matches any character within the brackets (e.g., `[a-z]`)                         |
| `[^]`             | Negated character class, matches any character not in the brackets                |

## IV. Finding inside the Shell
### 1. Reverse-i Search
- press `ctrl + R` to start the reverse-i search
### 2. Shell Globing
- The shell substitutes arguments containing globs to filenames
	- the substitution is called expansion because the shell substitutes all matching filenames.
#### Globbing characters:
 - `*`
 - `?`
#### Notes: 
- If no file matches a glob, the bash shell performs no expansion, and the command runs with literal characters such as `*`
- the shell only performs expansion before running commands. Therefore if a `*` makes it to a command without expanding the shell won't do anything more with it.
## Find
*Syntax* -> `find (starting directory) (matching criteria and actions)`
### Important matching criteria:
- `-name nam` (nam with globbing syntax)
- `-regex nam` (nam with regex syntax) `find -regex '\.c$'`
- `-path pth `(pth is the path)
- `-type c` (c is for file type: f, d)
- `-user usr `(file’s owner is usr)
- `-group grp `(file’s group owner is grp)
- `-perm p` (the files access mode is exactly p) ; variants -p(at least p) or /p (any of p)
- `-size n` (file is n blocks big; a block is 512 bytes)
#### Note:
Each permission is assigned a specific value:
- **Read (r)** = 4
- **Write (w)** = 2
- **Execute (x)** = 1
file permissions are represented in 3 digits the *first one being for the owner*
*second for the group* and the *third for others*
### Chaining matching criteria:
Chaining matching criteria:
- `!` for negating
- `( expr )` force precedence
- `-o` for or
- `,` for both (not and)
### Important actions:
- `-print`: Outputs matched files.
- `-delete`: Deletes matched files.
- `-ls`: Lists detailed information about matched files.
- `-prune`: Excludes a directory from being searched.
- `-exec {}` `\;`: Runs a command for each matched file.
- `-exec {}` `+`: Runs a command once for all matched files.
- `-ok {}` `\;`: Like `-exec`, but asks for confirmation.
- `-execdir {}` `\;`: Executes the command from the matched file's directory.
## Grep
#### General Syntax:
```bash
grep [OPTIONS] PATTERN [FILE...]
```
- `PATTERN` is a regex pattern defining what you want to find in the content of the files specified by the `FILE` argument.
#### Important (Basic) Patterns:
- `[]`: Match any single character inside the list.
- `[^]`: Match any character **not** in the list.
- `^`: Match the beginning of the line.
- `$`: Match the end of the line.
- `.`: Match any single character.
- `*`: Match zero or many occurrences of the previous character.
#### Important (Extended) Patterns:
Use `-E` with `grep` to enable extended regular expressions(or `egrep`).
- `?`: Match zero or one occurrence of the previous character.
- `+`: Match one or many occurrences of the previous character.
- `{}`: Intervals.
- `|`: OR operator.
- `()`: Grouping.
## Sed
#### General Syntax:
```
sed [OPTIONS] SCRIPT FILE
```
- `SCRIPT` is the `sed` script that will be executed on every line in the file specified by `FILE`.
#### Script Structure:
```
[addr] X
```
- `addr`: Condition applied to the lines of the text file.
- `X`: Command that `sed` will execute.
#### Address (`addr`):
- Fixed number or interval: `first,last`
- Regex pattern: `/regex/`
- `first~step`: Match the first line and every step-th line after.
- `first,+N`: Match the first line and the next N lines (N+1 total lines).
#### Commands:
- `p`: Print.
- `d`: Delete.
- `s/pattern/replacement/`: Search for `pattern` and replace it with `replacement`.
#### Examples:
1. `sed '1,+5 d' file.txt`: Delete the first 6 lines (1st through 6th).
2. `sed 's/hello world/bonjour le monde/' file.txt`: Replace "hello world" with "bonjour le monde" in the file.
## Awk
#### General Syntax:
```
awk [options] 'pattern { action }' file
```
- `pattern`: A regex pattern tested against every input line.
- `action`: Script to execute if the pattern matches.
#### Pattern:
- The pattern is a regular expression tested against each line.
- If the pattern matches, `awk` executes the corresponding action.
- If no pattern is provided, the action is executed on every line.
- **Special patterns**: `BEGIN`, `END`.
#### Actions:
- `print`: Outputs matching lines or results of actions.
- **Special variables**:
  - `$0`: The entire line.
  - `$1`, `$2`, etc.: Fields in the line split by whitespace (first field, second field, etc.).
#### Examples:
1. `awk '/hello/ { print $0 }' file.txt`: Print all lines containing the word "hello".
2. `ls -l | awk '/\.c$/ { count++ } END { print count }'`: Count the number of `.c` files in the current directory and print the count.
