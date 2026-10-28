# Vim Commands Reference

## Navigation:
- **gg**  
  Go to the **first line** of the file.

- **G** (Shift + g)  
  Go to the **last line** of the file.

- **0**  
  Move to the **beginning of the current line**.

- **$**  
  Move to the **end of the current line**.

## Insert Lines:
- **o**  
  Open a new line **below** and enter Insert mode.

- **O**  
  Open a new line **above** and enter Insert mode.

## Searching:
- **/word**  
  Search **forward** for "word" in the file.

- **?word**  
  Search **backward** for "word".

- **n**  
  After a search, go to the **next match**.

- **N**  
  Go to the **previous match** after a search.

## Substitution:
- **:%s/word/newword/g**  
  Replace all instances of "word" with "newword" in the **entire file**.

## Editing & Deletion:
- **dd**  
  Delete (cut) the **current line**.

- **dw**  
  Delete a **word** (from the cursor forward).

## Copy & Paste:
- **y**  
  **Yank** (copy) the selected text or line (`yy` for the whole line).

- **p**  
  **Paste** the yanked or deleted content **after** the cursor.

## Settings:
- **:set nu**  
  Show **line numbers**.

- **:set nonu**  
  **Hide line numbers**.

## Undo / Redo:
- **u**  
  Undo the **last change**.

- **Ctrl + r**  
  Redo the **last undone change**.

## Clear All Content in Vim:
To clear the entire content of a file in Vim:

### Method 1: Delete All Lines
1. Press `Esc` to make sure you're in **Normal mode**.
2. Type:
   **:1,$d**
