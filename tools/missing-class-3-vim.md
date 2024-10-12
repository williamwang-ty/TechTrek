# Missing semester class 3: Vim editor

## Basics

### Philosophy of Vim

- most time reading /editing, not writing
- Modal editing: keys have different meaning in different context
- editing interface is a programming language: keystrokes are commands, which are composable;(melody)
- goal: match the speed at which you think
- melody vs chord

### Modal editing

- Normal (back normal with `<esc>`)
- Insert: input text( enter in with`i`)
- Replace: replacing text( enter in with`R`)
- Visual: selecting blocks( enter in with`v`, `V`, `<C-v>`)
- Command-line:( enter in with`:`)

### Buffers, tabs, windows

- buffer: a open file in memory
- window: shows a buffer, but no 1-1 with buffers, windows are merely views, 1 buffer may be open in multiple windows
- a tab contains several windows, a layout or workspace for windows

## The elements of the editing programming language

VIM use melody instead of chord to avoid key binding, and provide a more flexible and powerful programming language.

### Movement(nouns)

- basic: arrow, `h` `j` `k` `l`
- Scroll: `<C-u>`, `<C-d>`

- words: `w`, `b`, `e`
- Lines: `0`, `^`, `$`
- Line numbers: `:{line #}`,`{#}G`,`{#}j or k`

- File: `gg`, `G`
- Screen: `H`, `M`, `L`

- Find in a line: `f{char}`, `t{char}`, `F{char}`, `T{char}`, navigating matches with`,`  `;`
- Search: `/{regex}`, navigating matches with `n` / `N`
- Matching corresponding items (blanket, code blocks): `%`

### Edits (verbs)

- `i`: into insert mode
- `o`/`O`: insert line below/above

- `d{motion}`: delete, `dw`,`d$`, `d0`
- `c{motion}`: change, equals: `d + i`
- `x`: delete char
- `s`: substitue char, `~`: flip the case of a char

- `u` to undo, `<C-r>` to redo
- `y` to copy, `p` to paste

### Other

Counts: `3w`

Modifiers:

- `i` means inner, `a` means around
- `ci(`, `da'`

Melody: `{verb}{counts}{noun:move}`

### Advanced

- Search and replace    `:s`
- multiple windows   `:sp`  /  `:vsp`
- Macros

## Others

### Recommend .vimrc

### Recommend plugin

- `Ctrlp.vim`
- `ack.vim`
- `nerdtree`
- `vim-easymotion`
- site: vim Awesome

### VIM in other programs

`Vimium` in google chrome
