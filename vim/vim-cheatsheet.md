# Vim Cheat Sheet

## Normal Mode

### Command Reference
- `:h <cmd>`

### Motion
- `h` move left
- `j` move down
- `k` move up
- `l` move right
- `$` move to end of line
- `^` move to beginning of line
- `w` move [count] words forward to beginning of next word
- `{operator}aw` operate on a complete word
- `{operator}ap` operate on a complete paragraph

### Last change
- `.` repeat last change
- `u` undo last change
- a *change* is everything from entering insert mode until returning to normal mode

### Search for character
- `f<c>` search for <c> character
- `;` repeat last search fwd
- `,` repeat last search backwards

### Search for word
- `*` searches for word under cursor
- `n` next match
- `N` previous match

### Switch to Insert Mode
- `i` **i**nsert characters before cursor 
- `I` **I**nsert at the beginning of line
- `a` **a**ppend characters after cursor
- 'A' **A**ppend at the end of line
- `s` delete characters and start insert; **s**ubstitute
- `c{motion}` **c**hange {motion} text and start insert. e.g: `cw` delete word
- `o` begin a new line under cursor and start insert
- `O` begin a new line above cursor and start insert

### Counts & Arithmetic
- `<Ctrl-a>` increase number at cursor
- `<CTRL-x>` decrease number at cursor
- **prefixing** with a count repeats command count times; e.g: `5<Ctrl-a>` increases number by 5

### Copy / Cut / Paste
- `d{motion}` delete; e.g: 
	- `2dw` delete (to end of word) word twice
	- `daw` delete a (complete) word
	- `dap` delete a (complete) paragraph
	- `d2w` delete 2 words
- `c{motion}` change and start insert mode
- `y{motion}` yank - copy
- `p` paste

### Operators
- `{operator}{motion}` operate on characters given by the motion
- `{operator}{same operator}` operate on current line; e.g:
	- `dd` delete current line
	- `>>` indent current line
	- `gUgU` or `gUU` convert line to SHOUTY case

| operator | description |
| --- | --- |
| `c` | change - delete and start insert mode |
| `d` | delete - cut |
| `y` | yank into register - copy |
| `~` | swap case - single char at cursor |
| `g~` | swap case |
| `gu` | make lowercase |
| `gU` | make uppercase |
| `>` | shift right |
| `<` | shift left |
| `=` | autoindent |

### Replace
-`r{char}` replace character under cursor with {char}

## Insert Mode
### Switch Mode
- `<Esc>` switch to Normal Mode
- `<Ctrl-o>` switch to Insert Normal Mode; switches to insert mode for a single command, then switches back to insert mode; e.g:
	- `<Ctrl-o>zz` redraw screen with current line in the middle

### Delete
- `<Ctrl-h>` delete back one character
- `<Ctlr-w>` delete back one word
- `<Ctrl-u>` delete back to start of line

### Paste / Registers
- `<Ctrl-r>0` paste from yank register; 
- `<Ctrl-r>{register}` paste from {register}
- `<Ctrl-r>=` expression register; opens prompt, enter evaluated expression into text

## Visual Mode
### Switch to visual mode
- `v` character-wise visual mode
- 'V' line-wise visual mode
- `<CTRL-v>` block-wise visual mode

### Edit selection
- `gv` reselect last visual selection
- `o` toggle free end of visual selection

### Edit selected text
- `Vr{char}` replace characters in line with {char}
- `<Ctrl-v>{motion}c` select block, change text in selection; after returning to normal mode, chnage is applied to every line of selected block
