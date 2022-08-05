# Bash

Parent: [[01_Tools]]
See also: [[linux]], ssh

#tools

# Basic commands

`pwd` - name current folder
`cd` - change folder (just typing `cd` without nothing is equivalent to `cd ..` and makes it go one folder up)
`ls` - list current folder content
    `ls -a` - shows all files, including hidden ones and `..`
    `ls -haltr` - lists files with a table of access rights
`touch file_name` - creates an empty file with this file name
`cat file_name` - print file (and by `file_name` here and later I mean a path to the file, which may be local or global)
`cp source_file target` - copy file
`rm file_name` - removes file

`chmod` - change access rights. Among other things:
    `chmod 600 file_name` - makes it read/write
    `chmod 400 file_name` - makes read only
    
`alias smth='a custom sequence of commands'` - creates an alias. It makes sense to place those in the auto-executable profile file (see `.bash_profile` below)

# Other useful info

`~` in paths is equivalent to root folder (on Mac, user folder)

`.bash_profile` (or, on modern Mac, `.zprofile`)  is a hidden file in the root folder that is executed every time the terminal is initialized (on Mac, locally, on remote Linux machine - as you connect it with a terminal). Anything default can be put there.

# Working with remote connections

`scp` - same as `cp`, but for copying remotely

# VIM

To start: `vi file_name`

To start editing: `i`, then, when done, `Esc`

To quit: `ZZ` (capitalized, aka with "Shift")