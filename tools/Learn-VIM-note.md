# Learn Vim notes

Goal: Keyboard driven workflow

[TOC]

## Modes

Modes:

- Normal mode: edit text and navigate
- Insert mode: type text and stuff
- Visual mode: select text and edit it
- command mode: input command

Why  mode:

- Doesn't rely on key combinations
- normal mode is default: far more time reading, navigating and changing code inserted

Normal mode:

- move around: hjkl
- into insert mode: I
- go back to normal mode: `<esc>`, `<ctrl-c>`, `<ctrl-[>`
- Recommend use `<ctrl-c>`; Recommend remap ctrl to caps lock

## key binding

`<ctrl-c>`: no copy, use y instead
`<ctrl-v>`:into visual mode, use p instead

VSCodeVim extension/package.json

- Keybindings section
  - Vim.useCtrlKeys:
    - true: vim binding
    - false: usual
- Vim.handleKeys:
  - `<C-d>`: true, means `<C-d>` handled by vim

## chords and melodies

Chords: typing a combination of keys at the same time.
Melodies: a series of keys pressed one another

## motion

### arrows

- `h`: <-
- `j`/`k`: down/up
- `l`: ->

### Moving horizontally

#### Word based motion

- `w`(word >)
- `b`(back <)
- `e`(end, > end of the word)
- `ge`(end, < end of the word)

WORD based motion: WORD (include `.` `(` `{` in previous word)

- `W`
- `B`
- `E`
- `gE`

#### Find char in a line motion

`f{character}`

- `f`: Find, next character in a line
- `F`: Find previous character in a line

`t{character}`

- `t`: Until, just before a character(move until)
- `T`: backward

Repeat char search commands, `f` `F` `t` `T`:

- `;` next
- `,` previous
- `num` + `;` or `,` repeat num times

#### move extremely horizontally in a line

- `0`: the 1st char of a line
- `^`: the 1st non-blank char of a line
- `$`: end of line
- `g_`: non-blank char at the end of a line

### moving vertically

#### mid-range vertical  motion

`}`: entire paragraph downwards
`{`: upwards
`CTRL-D`: move down half a page
`CTRL-U`: up

#### vertical motion with search pattern

/{pattern}: search forward inside a file
?{pattern}: backwards
Pattern: regex

n: next match
N:previous

/: last search forwards
? :backwards

*: do search for the word under the cursor
#: backwards

### Other

- moving with counts: {count}{motion}: multiply a motion {count} times

- semantically:
  - gd: jump to definition of whatever is under your cursor;
  - gf: jump to a file in an import

- {line}gg: go to a specific line
- gg: to the top of the file
- G: to the end of the file

- %: jump to matching ({[]})

## editing with operations

### basic melody

 {operator}{count}{motion}

- operator: action to perform; function (verb)
- count: perform the action <count> times; argument1: adjectives;
- motion: from current cursor to the position of the motion destination; argument2: objects;
- composition make it an editing language;

### operators

#### useful operators

c: change, delete  the text and send you into insert mode;
d: delete

y: yank(copy)
p: paste

>: add indentation
<: removes indentation
=: format

g~: switch case
gU: switch into uppercase
gu: switch into lowercase
~: switch case for a single character

. : repeat the last change,  combine with repeat search commands(; , n N).

u: undo the last change
CTRL-R: redo

#### shorthand syntax

Double an operator: operate on a whole line: 
- dd: deletes whole line
- cc: changes a whole line

Capitalize an operator  to have it perform a stronger version:
- D: deletes from the cursor to the end of the line
- Y: = yy, copy a complete line

#### text objects

Text objects are structured pieces of text.
Words, sentences, quoted text, paragraphs, blocks, (HTML)tags;

Specify a text object within a command:
Combine the letter a(a text object plus whitespace) or i(inner object without whitespace) with a character  that represents a text object itself:

{operator}{i|a}{text-object-id}

{i|a}:

- i:  inner
- a: around

The built-in text-objects-id:

- w: word
- s: sentence
- p: paragraph
- t: tag
- b or (:block surrounded by()
- B or {: block surrounded by {}
- < or >:  block surrounded by <>
- [ or ]:  block surrounded by []
- '' or ' or `: quoted text
 
## insert mode

### into an insert mode - basic

- i:  insert, put you in insert mode before the cursor;
- a: append,  put you in insert mode after the cursor;

- I:  insert, put you in insert mode at the beginning of the current line;
- A: append,  put you in insert mode at the end

### into an insert mode - enhanced

- o: open new line below
- O: open new line above
- gi: insert at the last place you left insert mode

### delete with chords in insert mode

- CTRL-h: delete the last character typed
- CTRL-w: delete the last word typed
- CTRL-u: delete the last line typed

### exit insert mode

- CTRL-c
- CTRL-[
- esc

## Select text in visual mode

- for copying and pasting
- for extra visual feedback when operating on blocks
- Vim equivalent to selecting text with a mouse
- select text by relying on the speedy vim motions

Use visual mode in tricky scenarios

- visual mode is often slower than Normal mode
- visual mode have the extra visual aid that gives you assurance of corrective selection

### into the visual mode

- v: visual mode character-wise, select text character by character 
- V: line-wise
- <c-v>: block-wise

### melody pattern

{trigger visual mode}{motion}{operator}

- {trigger visual mode}: v, V, <c-v>
- {motion}: for current cursor to motion destination
- {operator}: same as normal mode editing

## Search

### combination of n and .
- n: repeat a search
- .: repeat the last change
- {n}{.}: apply the same change on every single	search match
- operation:
  - /cucumber
  - daW
  - n: go to next match
  - .: repeat command

### gn & gN

Melody: {operate}{gn}
- gn: operate on the matched text object

- operation:
  - /cucumber
  - dgn
  - .:repeat command(include go to next match)

## Copying and Pasting

### Main tools

- combination of operator and motion
- using registers to save stuff for later

### operator

Yanking:

- y{text-object}
- yy, Y: copy a whole line

Pasting:

- p: after the cursor
- P: before the cursor
- if pasting a line: after/ before the line where the cursor is resting on

Combination:

- yyp: duplicate a line
- yy{n}p: n-plicate a line
- yyP: IDEM but above
- ddp: swap lines

### cutting to save in registers

- d and c, actually cut things
- registers are like a special clipboard

#### Registers:

- unnamed register ":default
- named registers a-z
- yank register 0:stores the last thing you have yanked
- cut registers 1-9: store the last 9 things cut by c/d

#### reference register

Reference register with "{name of register}

Melody: "{name of register}{operator}{motion}

- "{name of register}y{motion}: copy text from current cursor to motion destination, store into register {name};
- "{name of register}d/c{motion}: cut text from current cursor to motion destination, store into register {name};

Ex:
- "ayas: yank around sentence into "a
- "ap: paste "a

See what is in your registers:

- :reg {register name}

### pasting in insert mode

Vim way: 

- CTRL-R {register name}

System copy and pasting keys in VS code:

- CTRL-c, CTRL-p

## ex-command mode

Command mode:

- run ex commands(start with :)
- search patterns(start with / and ?)
- Very limited ex-commands has been supported by VSCodeVim

### open, close, save files
 
- :edit {relative-path-to-file}
- Short version, :e

- open a new file to edit
- use relative paths that co-located or live near the current file

- :write, :w, to save a file
- :quit, :q, to close a file, will fail if not saved
- :q!, to force to close a file
- :wq, save and close a file

- :wa, save all files
- :qa, :wqa, :qa!

### Operating on multiple lines at once

Melody:

- :[range]command[optons]
- only command support in VSCodeVim is: delete(d)

Ex:

- :10,12d (line #: delete line 10,11, 12)
- :10,+2d (offset: line 10,11,12)
 - :.,+2d(current line of .)
 - :%d (delete whole file)
 - :0,+10d(0 is the beginning of the file)
 - :.,$d($ is the end of the file)
 - :'<,'>d ('<,'> is the highlight selected section in visual mode)

### repeating ex commands

- @: (repeat the last ex command)
- @@(repeat again)

### substituting text

#### melody

:[range]s/{pattern}/{substitute}/{flags}

- s: substitute(short for s)
- range: the range in which to apply s
- pattern: search pattern, support regex
- substitute: text we want to use
- flags: options to config the substitution
  - g: all occurrences in the current line (default)
  - i: case insensitive in searches
- ex: (:%s/^#/ /g)
  - % for the whole file
  - s substitute 
  - ^# any# at the beginning of a line
  - /  for an empty character
  - g all occurrences

## splits and tabs

### splits

Open file in split:

- :sp {relative-path-to-file), open a file in a horizontal split
- :vip, open in a vertical split

- <CTRL-W> s: open horizontal split
- <CTRL-W> v: Vertical

Move among split:

- <CTRL-W> + hjkl

### tabs

- :tabnew {file}
- :tabn, go to the next tab
- :tabp, previous
- :tabo, close all tabs


### surrounding

- default vim plug-in in VSCodeVim
- surroundings: quotes, parentheses, braces, tags, etc...)

#### delete surrounding with ds

- ds' (delete '')
- ds"(delete"")
- ds)(delete())
- ds}(delete{})
- dst(delete<tag></tag>)

#### change surrounding  with cs

- cs'"(change '' into "")
- cs([(change () into [])
- cst<p>(change <tag></tag> into <p></p>

#### add surrounding with ys

- ys{motion}{char} (add{char} to the text, from current to motion destination)
- ex: ysiw]:
  - add [] on current word
  - ys
  - i: inside word
  - w: text object word
  - ]: add []

#### counts for layers

Full melody:
- {operator}{counts}{existing}{new}

Counts: 
- specify which layer to operate
- ys not support counts
- Ex:
  - cs3(] only change the 2ed "" to ''), (((hello)))->(([hello]))
