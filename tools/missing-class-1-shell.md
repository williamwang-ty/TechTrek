# Class 1: shell

## Using the shell

dir locations:

- `.` current
- `..` parent
- `~` home
- `/` root

User prompt:

- `$` ordinary user account
- `#` root user account

commands:

- `date`
- `echo`

`"my photos" = my\ photos`

`$PATH`:

- Way to search program: environmental variables
- A list of paths: where shell search program
- `echo $PATH`
- `which echo`

## Navigating in the shell

cd command:

- `pwd`
- `cd`
- `cd ~`
- `cd -`: back to previous path

ls command:

- `ls --l`
- `d rwe r-x r-r`
  - d:directory
  - permissions:
    - 3bits:owner / 3:owner group / 3:other user
    - wxr
      -: `-`: no permision
      - `w`:modify, if dir: add/remove file in it
      - `x`: execute, if dir: require execute permission to search in it
      - `r`: read, if dir: require read permission to list the dir

file and dir command:

- `mv`: rename file, or move file; move directory
- `cp`: copy
-delete directory: `rm -r` ; `rm -ir` (give prompt); `rm -fr`(force remove)
- `Mkdir`

other command:

- `man`: manual
- `Ctrl-l`: = clear

## Connecting programs

### input/output steam

input/output stream

- the stream is txt data
- read from input stream
- print to output stream
- normally, input/output is terminal (keyboard/screen)

redirect

- `< file`:  read from file (input)
- `> file`:  print to file (output)

``` bash

cat hello.txt
cat < hello.txt
cat < hello.tex > hello2.txt
echo hello > hello.txt
```

output operator:

- `>` overwrite
- `>>` append
- `|` pipeline: Output | input

### cat command

cat command: print content to output stream:

```bash
cat file.txt 
cat file1.txt file2.txt
cat -n file.txt // display line
```

create file
`cat > newfile.txt`

combine file
`cat file1.txt file2.txt > combined.txt`

Append content at the end of file
`cat >> existingfile.txt`

- With pipeline:
`cat file.txt | grep "pattern"`

### other command

`tail`

- only print  the last line of the input stream
- -n#: default is 10, last # lines
- `ls -l  | tail -n1`
- often used to show the latest records in log

`curl`

- transfer data through network protocol
- usual used to send http request and receive response
  - get: `curl https://www.a.com`
  - post: `curl -x POST -d "key1=value1&key2=value2" https://www.example.com/api`
  - test REST API
  - diagnose network issues
  - `curl --head --silent google.com | grep -i content-length`

## Other useful

### root user

`sudo`

- do sth as su(super user)
- `$sudo echo 500 > brightness (linux.sysfs)`: permission denied
- `$sudo su, #echo 500 > brightness`: OK
- `$echo 1060 | sudo tee brightness`: OK

### open

- `xdg-open`, `open` in macOS
- open various files with appropriate method

### terminal app

- Iterm2 in macOS
- Color setting:
  - Background  color: black
  - Front color: green
  - Font: source code pro

## Exercises

### 1

make sure you're running the right shell:

```bash
% echo $SHELL
/bin/zsh
```

### 2

create new dir call missing under /tmp

```bash
% mkdir /tmp/missing
```

### 3

Look up the touch program, use man.

`% man touch`

- it is used to change the access and modification time of a file.

### 4

use touch to create a new file called semester in missing

```bash
% touch  /tmp/missing/semester
```

### 5

Write the following into that file, one line at a time:

```bash
#!/bin/sh
 curl --head --silent https://missing.csail.mit.edu

% echo '#!/bin/sh' > ./semester.       # '' not ""
% echo curl --head --silent https://missing.csail.mit.edu >> ./semester
```

### 6

execute the file, i.e. type the path to the script (`./semester') into your shell and press enter, understand why it doesn't work, watch the permission bits of the file.

```bash
%ls -l
-rw-r--r-- semester
rw-: no execution permission
```

### 7

Run the command by explicitly starting the `sh` interpreter, and giving it the file `semester` as the first argument, i.e. `sh semester`.

Answer:

- sh will create an new environment
- sh can run script without +x permission
- the environment created by sh will not affect the current environment variables

### 8

Look up the `chmod` program

Answer:

digit mode

```bash
chmod 755 filename
```

- owner(7): r(4) + w(2) + x(1)
- group(5): r(4) + x(1)
- other(5):r(4) + x(1)

symbol mode

```bash
chmod u+x filename
```

- u: owner
- +x: add execution permission

### 9

Use `chmod` to make it possible to run the command `./semester` rather than having to type `sh semester`.

Answer:

```bash
chmod u+x semester
```

### 10

Use `|` and `>` to write the "last modified" date output by `semester` into a file called `last-modified.txt` in your home directory.

Answer:

```bash

./semester | grep -i 'last-modified' > ./last-modified.txt
```
