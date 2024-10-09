# Class 2: Shell Script

## shell scripting

The shell script is optimized for performing shell-related tasks. Thus, creating command pipelines, saving results into files, and reading from standard input are primitives in shell scripting.

### variables

Assign variables:`foo=bar`
`foo = bar` will not work: calling the `foo` program with arguments `=` and `bar`.

- `''`: literal strings, not substitute variable values
- `""`: variable values

```bash
var="Hello"
echo '$var' # output: $var

var="Hello"
echo "$var" # output: Hello
```

### function

#### arguments

```bash
 
mcd () {
  //creates a directory and `cd`s into it
  mkdir -p "$1"
  cd "$1"
}
```

Special variables to refer to arguments, error codes, and other relevant variables.

- `$0` - Name of the script
- `$1` to `$9` - Arguments to the script. `$1` is the first argument and so on.
- `$@` - All the arguments
- `$#` - Number of arguments
- `$?` - Return code of the previous command
- `$$` - Process identification number (PID) for the current script
- `!!` - Entire last command, including arguments. A common pattern is to execute a command only for it to fail due to missing permissions; you can quickly re-execute the command with sudo by doing `sudo !!`
- `$_` - Last argument from the last command. If you are in an interactive shell, you can also quickly get this value by typing `Esc` followed by `.` or `Alt+.`

#### return values

Commands will often return output using `STDOUT`, errors through `STDERR`

Return value:

- `0` OK;
- anything different from `0` : an error occurred.

Conditionally execute commands:
 `||`(or), `&&`(and), `;`(both)

```bash
false || echo "Oops, fail"  #Oops, fail
    
true || echo "Will not be printed"

true && echo "Things went well" # Things went well
    
false && echo "Will not be printed"
    
true ; echo "This will always run" # This will always run
    
false ; echo "This will always run" # This will always run
```

#### command substitution

Get the output of a command as a variable:

- `$(CMD)`: execute the `CMD`, get the output of the command, and substitute it in place
- `for file in $(ls)`: first call `ls`, `ls` returns a list of filename, and `for` iterates all these values

Process substitute:

- `<( CMD )`: execute `CMD`, its output as input stream, place the result into a temporary file, and substitute `<()` with that filename.
- value passed by file instead by STDIN, when command needs file to be the argument
- ex: `diff <(ls foo) <(ls bar)`

#### showcase

```bash
echo "Starting program at $(date)" # date will be substituted
    
echo "running program $0 with $    # arguments with pid $$"
    
for file in "$@"; do
  grep foobar "$file" > /dev/null 2> /dev/null 
  if [[ $? -ne 0 ]]; then
    echo "File $file does not have any foobar, adding one"
    echo "# foobar" >> "$file"
  fi
done
```

- It will iterate through the arguments provided, `grep` for the string `foobar`, and append it to the file as a comment if it's not found.

- `if [[ $? -ne 0 ]]`:
  - whether `$0`(the name of the script) not equal to 0,
  - if 0, ;
  - if not 0, then
  - use `[[]]` for shell compatible

- `grep foobar "$file" > /dev/null 2> /dev/null`
  - When pattern is not found, grep has exit status 1
  - redirect `STDOUT` and `STDERR` to a `null` since we do not care about them
  - `> /dev/null`: `STDOUT` redirect to `null`
  - `2> /dev/null`: `2` refers to `STDERR`,`STDERR` redirect to `null`

#### shell globbing

Use pattern characters to express argument patterns:

- Wildcards:
  - `?` `*`: match some number of characters
  - `foo, foo1, foo2, foo10, bar`: `rm foo?` will delete `foo1, foo2`, `rm foo*` delete all but `bar`
  - Curly braces `{}`:
    - automatically expand command

```bash
convert image.{png,jpg}
convert image.png image.jpg
    
 cp /path/to/project/{foo,bar,baz}.sh /newpath
cp /path/to/project/foo.sh /path/to/project/bar.sh /path/to/project/baz.sh /newpath
    
mv *{.py,.sh} folder     # Will move all *.py and *.sh files

mkdir foo bar
touch {foo,bar}/{a..h}.     # This creates files foo/a, foo/b, ... foo/h,bar/a, bar/b, ... bar/h
touch {foo,bar}/{a..h}
touch foo/x bar/y
```

Notice:

- Writing `bash` scripts can be tricky and unintuitive. There are tools like [shellcheck](https://github.com/koalaman/shellcheck) that will help you find errors in your sh/bash scripts.

- It is good practice to write shebang lines using the [`env`](https://www.man7.org/linux/man-pages/man1/env.1.html) command that will resolve to wherever the command lives in the system.Ex: `#!/usr/bin/env python`.

- Functions are executed in the current shell environment whereas scripts execute in their own process. Thus, functions can modify environment variables, whereas scripts can't. Scripts will be passed by value environment variables that have been exported using [`export`](https://www.man7.org/linux/man-pages/man1/export.1p.html)

## Useful shell commands

### Finding how to use commands

- `-h`, `--help`
- `man`
- `TLDR` shows the use case of a command, need to download

### Finding files

#### `find`

`find` will recursively search for files matching some criteria

```bash
TLDR find
```

```bash
# Find files by extension:
find root_path -name '*.ext' -type f

# Find files matching multiple path/name patterns:
find root_path -path '**/path/**/*.ext' -or -name '*pattern*'

# Find directories matching a given name, in case-insensitive mode:
find root_path -type d -iname '*lib*'

# Find files matching a given pattern, excluding specific paths:
find root_path -name '*.py' -not -path '*/site-packages/*'

# Find files matching a given size range, limiting the recursive depth to "1":
find root_path -maxdepth 1 -size +500k -size -10M
```

`find` can also perform actions over files that match your query.

```bash
# Run a command for each file (use `{}` within the command to access the filename):
find root_path -name '*.ext' -exec wc -l {} \;

# Find all files modified today and pass the results to a single command as arguments:
find root_path -daystart -mtime -1 -exec tar -cvf archive.tar {} \+

# Find empty files (0 byte) or directories and delete them verbosely:
find root_path -type f|d -empty -delete -print
```

#### `fd`

`fd` is a simple, fast and user-friendly altenative to `find`

```bash
tldr fd

# Recursively find files matching a specific pattern in the current directory:
fd "string|regex"

# Find files that begin with `foo`:
fd "^foo"

# Find files with a specific extension:
fd --extension txt

# Find files in a specific directory:
fd "string|regex" path/to/directory

# Include ignored and hidden files in the search:
fd --hidden --no-ignore "string|regex"

# Execute a command on each search result returned:
fd "string|regex" --exec command
```

#### `locate`

- `find` perform a real-time search by traversing the file system; it is slow but the result is latest
- `locate` perform a search in a pre-built database containing file index data; it is very fast but the result is not always up-to-date

### Finding code

#### `grep`

`grep` to search base on the content, it is a generic tool for matching patterns from the input text.
`grep` is useful in data wrangling.

```bash
# -C: show content around the matched line, +-2 lines
grep -C 2 "error" file.txt

# -v: inverting the match, lines not inclue "debug"
grep -v "debug" file.txt

# -r: recursively search files in dir and sub dir
grep -r "function" .

# combine the flag:recursively search dir, output *.py files which not including "test", show +- 3 lines matched
grep -rvC 3 "test" *.py
```

#### `rg`

`grep` alternatives include `ack`, `ag`, `rg`

```bash
# Find all python files where I used the requests library
rg -t py 'import requests'
# Find all files (including hidden files) without a shebang line
rg -u --files-without-match "^#\!"
# Find all matches of foo and print the following 5 lines
rg foo -A 5
# Print statistics of matches (# of matched lines and files )
rg --stats PATTERN
```

### Finding shell commands

- `up` arrow will give you back your last command
- `history` command will print your shell history to the standard output; `history | grep` will search for pattern.
- `<C-r>`: input a string to match in the command history
- `fzf` is a general-purpose fuzzy finder that can be used with many commands, `fsf` need to be installed
- history-base autosuggection is useful, recommen to install `Oh My Zsh`, and enable the `zsh-autosuggections`in it

### Directory Navigation

- writing shell alias or creating symlinks with `ln -s`
- For finding frequent and recent files and dirs, `fasd` ranks files and dirs by frecency, that is, both frequency and recency
- overview the dir structure: `tree`, `broot`
- full fledged file managers `nnn`, `ranger`

## Exercises
