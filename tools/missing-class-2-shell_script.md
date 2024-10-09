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
