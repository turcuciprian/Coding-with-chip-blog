---
title: "Vim Cheet Sheet"
layout: post
date: 2023-06-19
---
Here are a few useful beginner and advanced vim comands you (and I) should know:

## Basic

1. **Navigation commands:**
   - `h`, `j`, `k`, `l`: Move the cursor left, down, up, and right, respectively.
   - `gg`: Go to the first line of the file.
   - `G`: Go to the last line of the file.
   - `w`: Move the cursor forward to the beginning of the next word.
   - `b`: Move the cursor backward to the beginning of the previous word.
   - `0` (zero): Move the cursor to the beginning of the current line.
   - `$`: Move the cursor to the end of the current line.
   - `%`: Jump between matching opening and closing parentheses, brackets, or braces.

2. **Editing commands:**
   - `i`: Enter insert mode at the current cursor position.
   - `a`: Enter insert mode after the current character.
   - `A`: Enter insert mode at the end of the current line.
   - `o`: Begin a new line below the current line and enter insert mode.
   - `O`: Begin a new line above the current line and enter insert mode.
   - `x`: Delete the character under the cursor.
   - `dd`: Delete the current line.
   - `yy`: Copy the current line.
   - `p`: Paste the copied or deleted content after the cursor.
   - `u`: Undo the last command.
   - `Ctrl + r`: Redo the last undo.

3. **Search and replace commands:**
   - `/pattern`: Search forward for the specified pattern.
   - `?pattern`: Search backward for the specified pattern.
   - `n`: Move to the next occurrence of the search pattern.
   - `N`: Move to the previous occurrence of the search pattern.
   - `:s/old/new/g`: Replace all occurrences of "old" with "new" in the current line.
   - `:%s/old/new/g`: Replace all occurrences of "old" with "new" in the entire file.

4. **File management commands:**
   - `:e <file>`: Open a file for editing.
   - `:w`: Save the current file.
   - `:q`: Quit Vim.
   - `:q!`: Quit Vim without saving changes.
   - `:wq` or `:x`: Save changes and quit Vim.

5. **Visual mode commands:**
   - `v`: Start character-wise visual mode.
   - `V`: Start line-wise visual mode.
   - `Ctrl + v`: Start block-wise visual mode.
   - Once in visual mode, you can perform various operations, such as copying, cutting, and indenting selected text.



## Advanced Vim Comands

1. **Multiple Cursors:**
   - `Ctrl + v`: Start block-wise visual mode, which allows you to select multiple columns or lines.
   - `Ctrl + q`: Toggle between block-wise visual mode and normal mode, useful for selecting multiple cursors.
   - `I`: Insert text at the beginning of each selected line simultaneously.
   - `A`: Append text at the end of each selected line simultaneously.

2. **Folding:**
   - `zf{motion}`: Create a fold for the specified motion (e.g., `zfap` to fold a paragraph).
   - `zo`: Open a fold to reveal its content.
   - `zc`: Close a fold to hide its content.
   - `zM`: Close all folds.
   - `zR`: Open all folds.

3. **Marks and Jumps:**
   - `m{letter}`: Set a mark at the current cursor position (e.g., `ma` to set mark 'a').
   - `{letter}`: Jump to the line of the mark (e.g., `'a` to jump to mark 'a').
   - ```.```: Repeat the last change or command.
   - `Ctrl + o`: Jump to the previous location.
   - `Ctrl + i`: Jump to the next location.
   - `:ju[mps]`: Display the jump list.
   - `:marks`: Display a list of marks.

4. **Macros:**
   - `q{letter}`: Start recording a macro into the specified register (e.g., `qa` to start recording into register 'a').
   - `q`: Stop recording the macro.
   - `@{letter}`: Execute the macro stored in the specified register (e.g., `@a` to execute the macro stored in register 'a').

5. **Sessions and Views:**
   - `:mksession {file}`: Save the current Vim session to a file.
   - `:source {file}`: Load and execute Vim commands from a script file.
   - `:mksession! {file}`: Overwrite an existing Vim session file.
   - `:source! {file}`: Reload and execute Vim commands from a script file.
   - `:windo {command}`: Execute a command on all windows.
   - `:tabdo {command}`: Execute a command on all tabs.

6. **Registers:**
   - `"{register}{command}`: Perform a command using the specified register (e.g., `"ayy` to copy the current line into register 'a').
   - `:registers`: Display the contents of all registers.

7. **External Commands:**
   - `:!{command}`: Run an external shell command from within Vim.
   - `:r !{command}`: Insert the output of an external command into the current buffer.
 

8. **Text Manipulation:**
   - `:normal {commands}`: Execute normal mode commands on each line in a range.
   - `:substitute/{pattern}/{replacement}/{flags}`: Perform a search and replace operation on a range of lines.
   - `:global/{pattern}/{command}`: Execute a command on lines that match a pattern.
   - `:sort`: Sort lines in the current range.
   - `:join`: Join selected lines together.

9. **Buffers and Windows:**
   - `:bnext` or `:bn`: Switch to the next buffer.
   - `:bprevious` or `:bp`: Switch to the previous buffer.
   - `:bdelete {buffer}`: Close a specific buffer.
   - `:split {file}`: Split the current window horizontally and open a new file.
   - `:vsplit {file}`: Split the current window vertically and open a new file.
   - `Ctrl + w + h/j/k/l`: Switch between windows in different directions.
   - `Ctrl + w + =`: Make all windows equal size.
   - `Ctrl + w + _`: Maximize the current window.
   - `Ctrl + w + o`: Close all windows except the current one.

10. **Sessions and Tabs:**
   - `:mksession {file}`: Save the current Vim session to a file.
   - `:source {file}`: Load and execute Vim commands from a script file.
   - `:tabnew {file}`: Open a new tab and edit the specified file.
   - `:tabnext` or `:tabn`: Switch to the next tab.
   - `:tabprevious` or `:tabp`: Switch to the previous tab.
   - `:tabclose` or `:tabc`: Close the current tab.

11. **Visual Block Mode:**
   - `Ctrl + v`: Enter block-wise visual mode.
   - `I`: Insert text at the beginning of each selected line in block-wise visual mode.
   - `A`: Append text at the end of each selected line in block-wise visual mode.
   - `r`: Replace the selected block with a single character.
   - `c`: Change the selected block to new text.
   - `J`: Join the selected blocks into a single line.

12. **Regular Expressions:**
   - `:g/{pattern}/{command}`: Execute a command on lines that match a pattern.
   - `:v/{pattern}/{command}`: Execute a command on lines that do not match a pattern.
   - `/\v`: Enable very magic mode in regular expressions.
   - `:s/{pattern}/{replacement}/g`: Perform a search and replace operation on the current line.
   - `:s/{pattern}/{replacement}/gc`: Perform a search and replace operation with confirmation on each match.

13. **Vimscript:**
   - `:let {variable} = {value}`: Assign a value to a Vimscript variable.
   - `:function {name}`: Define a new Vimscript function.
   - `:call {function}`: Call a Vimscript function.
   - `:source {file}`: Load and execute Vimscript commands from a script file.
