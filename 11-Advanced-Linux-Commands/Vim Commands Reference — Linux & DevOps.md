# Vim Commands Reference — Linux & DevOps

## 1. What Is Vim?

**Vim (Vi Improved)** is a text editor used in Linux to create, edit, and modify files directly from the terminal.

### Common uses
- Edit Linux configuration files.
- Create Bash scripts and Python programs.
- Edit files on remote AWS EC2 instances through SSH.
- Search and replace text in configuration files.
- Review and modify logs or documentation.

### Open a file

```bash
vim filename.txt
```

Create a new file or open an existing file:

```bash
vim script.sh
vim health_monitor.py
sudo vim /etc/hosts
```

**Important:** Use `sudo` only when administrator permissions are required.

---

## 2. Vim Modes

Vim mainly uses three modes.

| Mode | Purpose | How to enter |
|---|---|---|
| Normal mode | Navigate, delete, copy, and run commands | Press `Esc` |
| Insert mode | Type or edit text | Press `i`, `a`, `o`, etc. |
| Visual mode | Select text | Press `v`, `V`, or `Ctrl + v` |

**Memory trick:**

- Normal = Navigate and manage.
- Insert = Input text.
- Visual = View and select text.

When unsure which mode you are in, press `Esc` to return to Normal mode.

---

## 3. Opening, Saving, and Exiting

These are the most important commands to learn first.

| Command | Meaning |
|---|---|
| `vim file.txt` | Open or create a file |
| `:w` | Save changes |
| `:w filename.txt` | Save as another filename |
| `:q` | Quit if there are no unsaved changes |
| `:wq` | Save and quit |
| `:x` | Save and quit if changes need saving |
| `ZZ` | Save and quit from Normal mode |
| `:q!` | Quit without saving changes |
| `ZQ` | Quit without saving from Normal mode |
| `:wqa` | Save and quit all open tabs |

### Example workflow

1. Open a file: `vim notes.txt`
2. Press `i` to enter Insert mode.
3. Type your text.
4. Press `Esc`.
5. Type `:wq` and press `Enter`.

**Warning:** `:q!` discards unsaved edits. Use it only when you do not want to keep your changes.

---

## 4. Cursor Navigation

### Basic movement

| Key | Action |
|---|---|
| `h` | Move left |
| `j` | Move down |
| `k` | Move up |
| `l` | Move right |
| `0` | Beginning of the line |
| `^` | First non-blank character of the line |
| `$` | End of the line |
| `g_` | Last non-blank character of the line |
| `gg` | First line of the file |
| `G` | Last line of the file |
| `5G` or `5gg` | Go to line 5 |
| `%` | Jump between matching brackets |
| `H` | Top of visible screen |
| `M` | Middle of visible screen |
| `L` | Bottom of visible screen |

### Word navigation

| Key | Action |
|---|---|
| `w` | Next word start |
| `W` | Next whitespace-separated word start |
| `e` | End of the next word |
| `E` | End of the next whitespace-separated word |
| `b` | Previous word start |
| `B` | Previous whitespace-separated word start |
| `ge` | Previous word end |
| `gE` | Previous whitespace-separated word end |

### Find a character on the current line

| Key | Action |
|---|---|
| `fx` | Move to the next `x` |
| `tx` | Move just before the next `x` |
| `Fx` | Move to the previous `x` |
| `Tx` | Move just after the previous `x` |
| `;` | Repeat the last `f`, `t`, `F`, or `T` movement |
| `,` | Repeat it in the opposite direction |

Replace `x` with the character you want to find.

### Screen navigation

| Key | Action |
|---|---|
| `Ctrl + f` | Scroll forward one page |
| `Ctrl + b` | Scroll backward one page |
| `Ctrl + d` | Scroll down half a page |
| `Ctrl + u` | Scroll up half a page |
| `Ctrl + e` | Scroll the screen down one line |
| `Ctrl + y` | Scroll the screen up one line |
| `zz` | Center the cursor line |
| `zt` | Put the cursor line at the top |
| `zb` | Put the cursor line at the bottom |

**Tip:** Prefix a movement with a number to repeat it. For example, `4j` moves down four lines.

---

## 5. Insert and Append Text

| Key | Action |
|---|---|
| `i` | Insert before the cursor |
| `I` | Insert at the beginning of the line |
| `a` | Append after the cursor |
| `A` | Append at the end of the line |
| `o` | Open a new line below and enter Insert mode |
| `O` | Open a new line above and enter Insert mode |
| `ea` | Append at the end of the word |
| `Esc` | Return to Normal mode |

### Example

To add a line below the current line:

1. Press `Esc`.
2. Press `o`.
3. Type the new text.
4. Press `Esc`.

---

## 6. Editing and Replacing Text

| Command | Action |
|---|---|
| `r` | Replace one character |
| `R` | Replace characters until `Esc` |
| `x` | Delete the character under the cursor |
| `X` | Delete the character before the cursor |
| `dd` | Delete the current line |
| `D` or `d$` | Delete to the end of the line |
| `dw` | Delete from the cursor to the next word boundary |
| `diw` | Delete the word under the cursor |
| `daw` | Delete the word and its surrounding whitespace where applicable |
| `cc` | Replace the entire current line |
| `C` or `c$` | Change text to the end of the line |
| `ciw` | Change the word under the cursor |
| `cw` | Change from the cursor to the end of the word |
| `s` | Replace one character and enter Insert mode |
| `S` | Replace the entire line |
| `J` | Join the current line with the next line |
| `gJ` | Join lines without adding a space |
| `.` | Repeat the last change |

**Remember:** Commands such as `dd`, `dw`, and `ciw` operate in Normal mode.

---

## 7. Copy, Cut, and Paste

In Vim, copying is called **yanking**.

| Command | Action |
|---|---|
| `yy` | Copy the current line |
| `2yy` | Copy two lines |
| `yw` | Copy from the cursor to the next word boundary |
| `yiw` | Copy the word under the cursor |
| `yaw` | Copy the word and surrounding whitespace where applicable |
| `y$` or `Y` | Copy to the end of the line |
| `dd` | Cut the current line |
| `2dd` | Cut two lines |
| `p` | Paste after the cursor or below the line for linewise text |
| `P` | Paste before the cursor or above the line for linewise text |
| `gp` | Paste after and move the cursor after the inserted text |
| `gP` | Paste before and move the cursor after the inserted text |

### Example

Copy and paste a line:

1. Move to the line.
2. Press `yy`.
3. Move to the destination.
4. Press `p`.

### System clipboard

On Vim builds with clipboard support:

- `"+y` — Copy selected text to the system clipboard.
- `"+p` — Paste from the system clipboard.

Clipboard support depends on how Vim was installed and on the terminal environment.

---

## 8. Visual Mode — Select Text

| Key | Action |
|---|---|
| `v` | Select characters |
| `V` | Select whole lines |
| `Ctrl + v` | Select a rectangular block |
| `o` | Move to the other end of the selection |
| `Esc` | Exit Visual mode |
| `y` | Copy selected text |
| `d` | Delete selected text |
| `>` | Indent selected text |
| `<` | Reduce indentation |
| `u` | Convert selected text to lowercase |
| `U` | Convert selected text to uppercase |
| `~` | Toggle the case of selected characters |

### Example: Delete multiple lines

1. Press `Esc`.
2. Move to the first line.
3. Press `V`.
4. Use `j` or `k` to extend the selection.
5. Press `d`.

---

## 9. Searching in Vim

| Command | Action |
|---|---|
| `/word` | Search forward |
| `?word` | Search backward |
| `n` | Next match in the search direction |
| `N` | Previous match relative to the search direction |
| `*` | Search forward for the word under the cursor |
| `#` | Search backward for the word under the cursor |
| `:noh` | Clear search highlighting |

### Example

Find the word `error`:

```text
/error
```

Press `Enter`, then use `n` and `N` to move between matches.

---

## 10. Search and Replace

| Command | Action |
|---|---|
| `:s/old/new/` | Replace first match on the current line |
| `:s/old/new/g` | Replace all matches on the current line |
| `:%s/old/new/g` | Replace all matches throughout the file |
| `:%s/old/new/gc` | Confirm each replacement throughout the file |
| `:5,10s/old/new/g` | Replace matches on lines 5–10 |

### Example

Replace `Ubuntu` with `Linux` throughout the file:

```vim
:%s/Ubuntu/Linux/g
```

To confirm every replacement:

```vim
:%s/Ubuntu/Linux/gc
```

**Important:** Vim search and replacement patterns use regular expressions. Special characters may need escaping.

---

## 11. Delete Lines and Clear File Contents

| Command | Action |
|---|---|
| `dd` | Delete current line |
| `3dd` | Delete three lines |
| `:3,5d` | Delete lines 3 to 5 |
| `:.,$d` | Delete from current line to the end |
| `:1,.d` | Delete from the first line through the current line |
| `:%d` | Delete all lines |
| `:g/error/d` | Delete every line containing `error` |
| `:g!/error/d` | Delete every line that does not contain `error` |

### Clear the entire file

1. Press `Esc`.
2. Type:

```vim
:%d
```

3. Press `Enter`.

The buffer becomes empty. To save the empty file, use `:w`.

**Caution:** This removes all lines from the current buffer. If you need the original contents, save a backup or make sure you can undo the change.

---

## 12. Undo and Redo

| Command | Action |
|---|---|
| `u` | Undo the last change |
| `Ctrl + r` | Redo the undone change |
| `U` | Restore the current line to its earlier state in the current editing session, subject to Vim's undo behavior |
| `.` | Repeat the last change |

**Memory trick:** `u` = undo; `Ctrl + r` = redo.

---

## 13. Indentation and Formatting

| Command | Action |
|---|---|
| `>>` | Indent current line |
| `<<` | Remove indentation from current line |
| `3>>` | Indent three lines |
| `3==` | Re-indent three lines |
| `gg=G` | Re-indent the entire file |
| `=%` | Re-indent a block when positioned on a matching bracket |
| `]p` | Paste with indentation adjusted to the current line |

These commands are useful when editing Bash scripts, Python files, YAML, and configuration files. Automatic formatting depends on file type and Vim settings.

---

## 14. Line Numbers and Editor Settings

| Command | Action |
|---|---|
| `:set nu` | Show line numbers |
| `:set nonu` | Hide line numbers |
| `:set relativenumber` | Show relative line numbers |
| `:set norelativenumber` | Hide relative line numbers |
| `:set paste` | Enable paste-friendly behavior |
| `:set nopaste` | Disable paste-friendly behavior |
| `:set ignorecase` | Ignore case during searches |
| `:set noignorecase` | Make searches case-sensitive by default |
| `:set hlsearch` | Highlight search matches |
| `:set nohlsearch` | Disable search highlighting |

These settings generally apply to the current Vim session unless configured persistently.

---

## 15. Registers — Store and Retrieve Text

Registers are Vim's storage locations for copied or deleted text.

| Command | Action |
|---|---|
| `:reg` | Display register contents |
| `"ayy` | Copy the current line into register `a` |
| `"ap` | Paste the contents of register `a` |
| `"+y` | Copy to the system clipboard register, if supported |
| `"+p` | Paste from the system clipboard register, if supported |

### Useful special registers

| Register | Meaning |
|---|---|
| `"` | Unnamed register |
| `0` | Most recent yank |
| `%` | Current filename |
| `#` | Alternate filename |
| `/` | Last search pattern |
| `:` | Last command-line command |
| `.` | Most recently inserted text |

---

## 16. Marks and Jump Navigation

| Command | Action |
|---|---|
| `ma` | Set mark `a` at the current position |
| `` `a `` | Jump to the exact position of mark `a` |
| `'a` | Jump to the first non-blank character of the line containing mark `a` |
| `:marks` | List marks |
| `Ctrl + o` | Move to an older jump location |
| `Ctrl + i` | Move to a newer jump location |
| `:jumps` | Display the jump list |
| `:changes` | Display the change list |
| `g;` | Jump to an older change location |
| `g,` | Jump to a newer change location |

---

## 17. Macros — Automate Repeated Editing

Macros record a sequence of Normal-mode commands so you can replay them.

| Command | Action |
|---|---|
| `qa` | Start recording macro `a` |
| `q` | Stop recording |
| `@a` | Run macro `a` |
| `@@` | Repeat the last executed macro |
| `5@a` | Run macro `a` five times |

**Example:** A macro can repeat the same editing operation across many similar lines. Practise on a sample file before using macros on important configuration files.

---

## 18. Working with Multiple Files and Buffers

| Command | Action |
|---|---|
| `:e filename` | Open another file |
| `:bn` | Next buffer |
| `:bp` | Previous buffer |
| `:ls` or `:buffers` | List buffers |
| `:b 2` | Switch to buffer number 2 |
| `:bd` | Delete the current buffer |
| `:sp filename` | Open a horizontal split |
| `:vsp filename` | Open a vertical split |

### Window navigation

| Command | Action |
|---|---|
| `Ctrl + w, s` | Split horizontally |
| `Ctrl + w, v` | Split vertically |
| `Ctrl + w, w` | Switch windows |
| `Ctrl + w, h` | Move to the left window |
| `Ctrl + w, l` | Move to the right window |
| `Ctrl + w, j` | Move to the window below |
| `Ctrl + w, k` | Move to the window above |
| `Ctrl + w, q` | Close the current window |

For these shortcuts, press the `Ctrl + w` combination first, release it, then press the second key.

---

## 19. Tabs

| Command | Action |
|---|---|
| `:tabnew` | Open a new tab |
| `:tabnew filename` | Open a file in a new tab |
| `gt` | Next tab |
| `gT` | Previous tab |
| `3gt` | Go to tab 3 |
| `:tabclose` | Close the current tab |
| `:tabonly` | Close all other tabs |
| `:tabmove 0` | Move current tab to the first position |

---

## 20. Folds and File Differences

### Folding

| Command | Action |
|---|---|
| `za` | Toggle a fold |
| `zo` | Open a fold |
| `zc` | Close a fold |
| `zR` | Open all folds |
| `zM` | Close all folds |
| `zi` | Toggle folding |

Folding behavior depends on the active fold settings and file type.

### Comparing files

```bash
vimdiff file1.txt file2.txt
```

| Command | Action |
|---|---|
| `]c` | Jump to the next difference |
| `[c` | Jump to the previous difference |
| `:diffthis` | Enable diff mode in the current window |
| `:diffoff` | Disable diff mode in the current window |
| `do` | Obtain the difference from the other window |
| `dp` | Put the difference into the other window |

Review the target and direction before using `do` or `dp`, because these commands modify text.

---

## 21. Help, Terminal, and Man Pages

| Command | Action |
|---|---|
| `:help` | Open Vim help |
| `:help keyword` | Open help for a topic |
| `:terminal` | Open a terminal inside Vim, if supported |
| `K` | Open the man page for the word under the cursor, if available |
| `:close` | Close the current window |
| `:saveas filename` | Save the current buffer under another filename |

### Built-in Vim tutorial

Exit to the shell if needed, then run:

```bash
vimtutor
```

This interactive tutorial is one of the best ways for beginners to practise Vim.

---

## 22. Practical Linux & AWS EC2 Exercise

Run these commands in your Ubuntu terminal:

```bash
mkdir -p ~/vim-practice
cd ~/vim-practice
vim practice.txt
```

Inside Vim:

1. Press `i` and type three or four lines.
2. Press `Esc`.
3. Type `:set nu` and press `Enter`.
4. Use `gg` to go to the first line.
5. Use `G` to go to the last line.
6. Use `/word` to search for a word you typed.
7. Press `n` to find the next match.
8. Use `yy` and `p` to copy and paste a line.
9. Use `u` to undo a change.
10. Type `:wq` and press `Enter` to save and exit.

Verify the saved file:

```bash
cat practice.txt
```

---

## 23. Common Beginner Mistakes

**Mistake 1: Typing text while in Normal mode**

Press `i` before typing. Press `Esc` to return to Normal mode.

**Mistake 2: Forgetting to save**

Use `:w` to save or `:wq` to save and quit.

**Mistake 3: Accidentally deleting text**

Press `u` immediately to undo, provided the change is still in the undo history.

**Mistake 4: Using `:q` with unsaved changes**

Vim refuses to quit if unsaved changes remain. Save with `:wq`, or discard them with `:q!` only if that is what you intend.

**Mistake 5: Confusing `0` and `^`**

- `0` moves to the first character position of the line.
- `^` moves to the first non-blank character.

---

## 24. Quick Revision Cheat Sheet

| Task | Command |
|---|---|
| Open a file | `vim file.txt` |
| Enter Insert mode | `i` |
| Return to Normal mode | `Esc` |
| Save | `:w` |
| Save and exit | `:wq` |
| Exit without saving | `:q!` |
| First line | `gg` |
| Last line | `G` |
| Specific line | `:10` |
| Start of line | `0` |
| End of line | `$` |
| Delete a line | `dd` |
| Copy a line | `yy` |
| Paste | `p` |
| Undo | `u` |
| Redo | `Ctrl + r` |
| Search | `/word` |
| Replace throughout file | `:%s/old/new/g` |
| Show line numbers | `:set nu` |
| Clear file contents | `:%d` |
| Open help | `:help` |
| Learn interactively | `vimtutor` |

## 25. Practice Checklist

- [ ] Understand Normal, Insert, and Visual modes.
- [ ] Open, edit, save, and exit a file.
- [ ] Navigate using `h`, `j`, `k`, `l`, `gg`, and `G`.
- [ ] Insert and delete lines.
- [ ] Copy and paste text.
- [ ] Search and replace words.
- [ ] Use undo and redo.
- [ ] Display line numbers.
- [ ] Practise editing a sample Bash or Python script.
- [ ] Complete `vimtutor`.

**Learning order:** Start with Sections 1–7, then practise searching, replacing, and saving. Learn macros, registers, tabs, and diff mode after you are comfortable with the basics.
