# Bash

Parent: [[01_Tools]]
See also: [[git]]

#tools

Todo: 
* xargs
* sudo
* chmod - what does it take?
* chown
* curl
* shebang lines (#!)

# Basic commands

**Navigate**
* `pwd` - name current folder
* `cd` - change folder
    * `cd` without nothing after it is equivalent to `cd ..` and brings us one folder up
    * `cd ~` teleports us into the root folder (on Mac, it would be the user folder)
* `pushd new_location` - remember current location (like a clipboard, but for location), then go to the new location
    * `popd` - teleport to remembered location
    * `pushd` alone swaps between current folder and last remembered folder. So repeatedly doing `pushd` without arguments allows you to go back-and-forth between two locations. 
* `exit` exits the terminal :)
* `man program_name` - shows manual for a program.  As it starts some sort of an editor (see below), use `ZZ` to exit.

**See**
* `ls` - list current folder content
    * `ls -a` - shows all files, including hidden ones and `..`
    * `ls -l` - long format (a table with columns). `-lh` is more "human-readable" (gigabites etc.).
    * `ls -R` - lists all files in all subfolders as well. Note that case matters here (`r` is reverse; `R` is recursive :)
    * `ls -l *.jpg` - list only files fitting this pattern
    * `ls -haltr` - a combo of "long", "human readable", "including hidden", and also sort by time (`t`) but reversed (`r`) 
* `cat file_name` - print file (and by `file_name` here and later I mean a path to the file, which may be local or global)
    * Technically it is a command to stream outputs, so one can do stream for a file etc. (_not sure how it works yet_)
* `less file` - display file screen-by-screen. Press `q` to quit this process. (Note that Windows powershell uses `more` instead of `less` for some reason, with same functionality)
* `find -name "file_name"` - find a file. Has a whole syntax of its own, for fancy searches by condition (size etc.)

**Create**
* `mkdir folder_name` - create a folder. 
    * If called with a `-p` key, `folder_name` can instead be a path (like `mydir/subdir/bla`). In this case it will create the whole sequence of folders.
* `touch file_name` - creates an empty file with this file name

**Edit**
* `cp source_file target_file` - copy one file (supports wildcards). Overwrites files without warning.
    * `cp -r source_folder target_folder` - copy a folder with all of its contents
    * `cp file_name path/` - copy to folder
* `mv source_file target_file` - rename file (suppots wildcards)
    * `mv file_name path/` - move file
* `chmod` - change access rights. Among other things:
    * `chmod 600 file_name` - makes it read/write
    * `chmod 400 file_name` - makes read only

**Destroy**
* `rm file_name` - removes file
    * `rm -r folder_name` - removes folder
* `rmdir folder_name` - removes a folder, but in a way that will refuse if the folder is not empty 
    * Note that on Mac, folders often have a `.DS_Store` hidden file, so there may be problems with there removal.
    * `rmdir -rf` removes all files in this folder as well (so doesn't halt if it is non-empty)

# Working with remote connections

`scp` - same as `cp`, but for copying remotely

# Scripting

* `echo "Hi!"` - output something
* `alias new_command_name='custom sequence of commands'` - creates an alias that becomes a new command
* `sh file_name` - execute this file as a script
* `at` - run a command at a later time (once)🏮
* `crontab` - executes a command regularly🏮
* `>` - redirects command output
* `>>` - redirects command output, but making it append output
* `|` - chains commands, such as output of the left command becomes input for the right one.
* `tee` - catches standard output and redirects it to the file. For example `command | tee log.txt`
* `tar` - compressor to compress data 🏮

Control structures
* `if [[ condition ]]; then ... fi` - here `...` is actually a multi-line, as there's no clear separator after `then`. `fi` serves as an "end" command for this block. 
* `for f in $something; do ... done`  - here `...` is also multi-line; `done` is an "end" command.
* `function_name () { ... }` - creates a function (write commands in separate lines instead of `...`). The function can get arguments, and they can be referensed inside the function as `$1`, `$2` etc. `$#` is the number of arguments. And then there's a whole bunch of weird bells and whistles.
* Because logical operators `||` (or) and `&&` (and) are short-circuiting, they can be used as control structures:
    * `condition || command_on_failure` - command will only be executed if condition is not met
    * `condition && command_on_success` - command will only be executed if condition is met

Working with variables
* `var="whatever"` - sets a variable
* `echo $var` - outputs the contents of a variable
* `$var` - instead of outputting the content, _executes_ it, as if somebody typed it in the command line and pressed Enter
* `$(command)` - executes command, then tries to interpret the _output_ of command as a command (as if it was typed)
    * `for file in $(ls)` - a useful example

Useful tips
* `.bash_profile` (or, on modern Mac, `.zprofile`)  is a hidden file in the root folder that is executed every time the terminal is initialized (on Mac, locally, on remote Linux machine - as you connect it with a terminal). Anything default can be put there.
* `$PATH` - environmental variable that lists all paths where the shell will look for commands. Separated by `:` apparently (weird)

# VIM

To start: `vi file_name`

To start editing: `i`, then, when done, `Esc`

To quit: `ZZ` (capitalized, aka don't forget to press "Shift", or it won't work)

# References

Command Line crash course (really the the basics, with lots of encouragement, a good link to send to complete newbies maybe): https://learnpythonthehardway.org/book/appendixa.html

Cheat sheet: https://learncodethehardway.org/unix/bash_cheat_sheet.pdf

The first 2 lectures of the MIT "Missing semester" (https://missing.csail.mit.edu/ ) are supposedly about bash, but it is really not consistent, as if it were more about bringing a bit of structure to something you already know, instead of teaching it from scratch.

Official reference: https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html

