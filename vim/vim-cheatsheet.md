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
- `A` **A**ppend at the end of line
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
- `<Ctrl-v>{motion}c` select block, change text in selection; after returning to normal mode, change is applied to every line of selected block
- `<Ctrl-v>jj$` + `A;` append a semicolon to the end of each line; also works on ragged blocks

## Command Line Mode
- `:{address}` go to address (line)
- `:[range]print` echo contents of range in prompt
- `:[range]delete [x]` delete range lines into register x
- `:[range]yank [x]` copy range lines into register x
- `:[line]put [x]` put text from register x after specified line
- `:[range]copy {address}` copy specified lines after address
- `:[range]move {address}` move specified lines after address
- `:[range]join` join lines
- `:[range]normal {commands}` execute normal mode commands on range
- `:[range]substitue/{pattern}/{string}/[flags]` replace occurrances of {pattern} with {string} on specified lines
- `:[range]global/{pattern}/[cmd]` execute ex command cmd on specified lines matching {pattern}

### shorthands
| shorthand | ex command |
| --- | --- |
| `:t` | `:copy` |
| `:m` | `:move` |
| `@:` | repeat last ex command |

### ranges
| range | description |
| --- | --- |
| `.` | current line - default when range / address is ommitted |
| `$` | last line |
| `%` | all lines |
| `1` | first line |
| `21` | line 21 |
| `1,21` | from line 1 to 21 inclusive |
| `.+1,$` | from line after current line to end |
| `.,.+5` | from current to current +5 lines (6 in total) |
| `.5` | same as `.+5` |
| `'<` | first line of visual selection |
| `'>` | last line of visual selection |
| `/{start pattern}/,/{end pattern}/` | specify range with pattern |

### tab complete
- `<Tab>` complete ex command / cylce through completions
- `<Shift-Tab>` cycle through completions backwards
- `<Ctrl-d>` show possible completeions

### command history
- `:` followed by `<Up>` or `<Down>`
- `q:` open command line window with history of ex commands
    - edit like any other text in buffer
    - `<Enter>` to execute
    - `:q` to close
- `q/` open command line window with history of searches

### run commands in the shell
- `:!{cmd}` execute shell-command cmd
- `:[range]!{cmd}` take range as input for shell command, replace with output; in other words filter range through an external program
- `:shell` start an interactive shell
    - `exit` to endd shell session
- `:read !{cmd}` read output of shell command {cmd} into buffer
- `:write !{cmd}` use contents of buffer as standard input for {cmd}

---

# Files & Buffers
- `:ls` list buffers
- `:[N]b[uffer] N` jump to buffer N (number according to :ls)
- `:bp[rev]` previous in list of buffers
- `:bn[ext]` next in list of buffers
- `:bf[irst]`  first in list of buffers
- `:bl[ast]` last in list of buffers
- `:bd[elete] N1 N2 N3` delete buffers N1, N2, N3
- `:N,M bd[elete]` delete buffers in range N - M
- `:args` print arguments vim was started with; [] indicates active file in list
- `:args {arglist}` add files to argument list
    - `*` filname wildcard in current dir
    - `**` filename wildcard that recurses into subdirectories

## save, revert, quit
- `:w[rite]` write contents of current buffer to disk
- `:e[dit]!` revert changes
- `:qa[ll]!` close all windows, discard changes without warning
- `:wa[ll]` write all modified buffers to disk

## split windows
- *window*: viewport onto a buffer