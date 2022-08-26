# Bash

Parent: [[01_Tools]]
See also: [[git]], [[docker]], [[crontab]]

#tools

#todo: 🔥🔥🔥
* xargs
* sudo
* chmod - what does it take?
* chown
* shebang lines (#!)

# Basics

**Navigate**
* `pwd` - name current folder
* `cd` - change folder
    * `cd` without nothing after it is equivalent to `cd ..` and brings us one folder up
    * `cd ~` teleports us into the home folder for this user.
    * `cd /` teleports you to the root
    * `cd -` teleports us to where you have been before the last `cd` command. Which means that doing `cd -` repeatedly will teleport you back and forth between two different folders.
* `pushd` - remember current location (like a clipboard, but for location). After doing that, you can go to the new location, and then teleport back with `popd`. It's literally a stack of locations.
    * Allegedly it's possible to remember locations by name, but I can't make it work 🔥 
* To see a brief help for the program, do `program_name --help`, or `help program_name` (🔥 does it work on Mac as well, or only on Windows?). For a long extensive and better formatted manual, do `man program_name`.  On linux, instead of dumping the content into an infinite scrollable screen, these commands start some sort of an editor, so you have to press "Enter" to scroll, and eventually press either `q` or `ZZ` to exit (see below).
* `exit` exits the terminal :)

**See**
* `ls` - list current folder content
    * `ls -a` - shows all files, including hidden ones and `..`
    * `ls -l` - long format (a table with columns). `-lh` is more "human-readable" (gigabytes etc instead of the exact number of bytes). This long-format list starts with a code for element permissions; something like `-rwxr--r--`. Here the first element is `-` for files,  `d` for directories (folders), and `l` for so-called "soft-links" (address aliases? 🔥); the rest are access rights (see below, in the `cmod` manual).  Then it shows the owner user, and the group, size, last modification date etc.
    * `ls -R` - lists all files in all subfolders as well. Note that case matters here (`r` is reverse; `R` is recursive :)
    * `ls -l *.jpg` - list only files fitting this pattern
    * `ls -haltr` - a combo of "long", "human readable", "including hidden", and also sort by time (`t`) but reversed (`r`) 
* `cat file_name` - print file (and by `file_name` here and later I mean a path to the file, which may be local or global)
    * Technically it is a command to stream outputs, so one can do stream for a file etc. (_not sure how it works yet_)
* `less file` - display file screen-by-screen. Press `q` to quit this process. (Note that Windows powershell uses `more` instead of `less` for some reason, with same functionality)
* `find -name "file_name"` - find a file. Has a whole syntax of its own, for fancy searches by condition (size etc.)
* `head filename` - see first 10 lines of the file. 
    * `head -30 filename` or `head -c 30 filename` to change the number of lines. 
    * Is typically used for piping, to only save the beginning of the output: `some_command | head -c 10 log.txt`
    * `tail -c 10 filename` - exactly same thing, but for the end. Is useful for piping and logging when key info comes at the end.

**Create**
* `mkdir folder_name` - create a folder. 
    * If called with a `-p` key, `folder_name` can instead be a path (like `mydir/subdir/bla`). In this case it will create the whole sequence of folders.
* `touch file_name` - creates an empty file with this file name

**Edit**
* `cp source_file target_file` - copy one file (supports wildcards). Overwrites files without warning.
    * `cp -r source_folder target_folder` - copy a folder with all of its contents
    * `cp file_name path/` - copy to folder
* `mv source_file target_file` - rename file (suppots wildcards)
    * `mv file_name path/` - move file. It's obviously possible to move and rename at the same time.
* `chmod` - change access rights. 
    * Access codes look something like `rwxr-x---`. Here the first 3 characters (`rwx`) are the permissions to read, write, and execute that the owner of this file has. Then the same 3 letters for the group to which the current user belongs. And then 3 letters with permissions for everyone else.
        * For folders, "read" means the ability to list the contents, and "write" means the ability to rename, create, or delete files in it. "Execute" on folders is kinda weird, but essentially, to access files one needs to have an "execute" permission on the folder itself, and all upstream folders. So typically for folders, "execute" pattern matches "read" pattern (🔥 is it true?).
    * The full and transparent syntax goes something like `chmod u=rwx,g=rwx,o=rwx file_name` for example, with tridads for owner, group, and "others" explicitly written. But it is also possible to do binary codes on these triads in base 8, for example:
    * `chmod 600 file_name` - makes it read/write, as 6 corresponds to `110`
    * `chmod 400 file_name` - makes it read only even for the owner, as 4 means `100`.
    * Note that it is possible to have write access to a file, but not to a folder that contains it. You will be able to write to the file, but not rename or delete it.

**Destroy**
* `rm file_name` - removes file
    * `rm -r folder_name` - removes folder with all its content (`-r` here stands for "recursive")
* `rmdir folder_name` - removes a folder, but in a way that will refuse if the folder is not empty. That's for safety.
    * Note that on Mac, folders often have a hidden `.DS_Store` file, so `rmdir` may not work even if a folder seems empty.
    * `rmdir -rf` removes all files in this folder as well (so this way it doesn't halt if it is non-empty)

# Scripting

* `echo "Hi!"` - output something (see below for outputting variables)
* `alias new_command_name='custom sequence of commands'` - creates an alias that becomes a new command
* `which command_name` - tries to find the command in the PATH (see below), but instead of implementing it, returns the full path to where it was found
* `sh file_name` - execute this file's contents as a script, in a _fork_ shell. It means that all variables that were set by this time will be inherited, but if `file_name` creates new environment variables, they will be dropped once `sh` execution is over. For example, if you change the working directory inside the `file_name` script, it won't change the working directory in the shell from which `sh` was called.
* `source file_name` - execute this file's contents as a script in the current shell. Apparently `. file_name` is a synonym.
* `>` - redirects command output
* `>>` - redirects command output, but making it append output
* `|` - chains commands, such as output of the left command becomes input for the right one.
* `tee` - catches standard output and redirects it to the file. For example `command | tee log.txt`
* `tar` - compressor to compress data 🔥

**Variables**
* `var="whatever"` - sets a variable
* `echo $var` - outputs the contents of a variable. For example, `echo $PATH` shows the contents of `PATH`.
* `$var` - instead of outputting the content, _executes_ it, as if somebody typed it in the command line and pressed Enter. This may actually generalize with the previous `echo` example: when someone types a dollar sign-variable, before the next step of processing, this construction is kinda replaced with the contents of the variable.
* `$(command)` - executes command, then tries to interpret the output of this command as a command (as if it was typed in the command line directly). For example: `for file in $(ls)` will become a loop over files, as if one typed all the file names after `in`.
* Because `$smth` just replaces it with the contents (or output) of `smth`, concatecating strings just means writing them one after another, like `$var1$var2`.

Environmental variables:
* `PATH` lists all paths where the shell will look for commands. Separated by `:` apparently (weird).

**Control structures**
* `if [[ condition ]]; then ... fi` - here `...` is actually a multi-line, as there's no clear separator after `then`. `fi` serves as an "end" command for this block. 
* `for f in $something; do ... done`  - here `...` is also multi-line; `done` is an "end" command.
* `function_name () { ... }` - creates a function (write commands in separate lines instead of `...`). The function can get arguments, and they can be referensed inside the function as `$1`, `$2` etc. `$#` is the number of arguments. And then there's a whole bunch of weird bells and whistles.
* Because logical operators `||` (or) and `&&` (and) are short-circuiting, they can be used as control structures:
    * `condition || command_on_failure` - command will only be executed if condition is not met
    * `condition && command_on_success` - command will only be executed if condition is met

Scheduling
* `at` - run a command at a later time (once)🔥
* For more complicated scheduling of periodical jobs, see [[crontab]]

Misc tips
* `.bash_profile` (or, on modern Mac, `.zprofile`)  is a hidden file in the root folder that is executed every time the terminal is initialized (on Mac, locally, on remote Linux machine - as you connect it with a terminal). Anything default can be put there.
* Many (but by far not all) of these commands are available on Windows via **powershell**. Essentially, a linux-like interface (way more so than cmd)
* In general, Linux and Mac machines have this central root, while Windows machines has a root for every partition (something that we call a "disk" colloquially)

# Remote connections and downloading

* `scp` - same as `cp`, but for copying remotely
* `wget options url` - to download something from the internet in a simple way using http protocol
* `curl options url` - another way to download stuff, but upports non-http protocols. Also one can exchange info with a server, and one can send stuff with it.
* `apt-get` - to update packages
    * `apt-get install ...` 🔥
    * `apt-get -y update` 🔥

# VIM

To start: `vi file_name`

To start editing: `i`, then, when done, `Esc`

To quit: `ZZ` (capitalized, aka don't forget to press "Shift", or it won't work)

# References

Command Line crash course (really the the basics, with lots of encouragement, a good link to send to complete newbies maybe): https://learnpythonthehardway.org/book/appendixa.html

Cheat sheet: https://learncodethehardway.org/unix/bash_cheat_sheet.pdf

The first 2 lectures of the MIT "Missing semester" (https://missing.csail.mit.edu/ ) are supposedly about bash, but it is really not consistent, as if it were more about bringing a bit of structure to something you already know, instead of teaching it from scratch. The  [lectures on youtube](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J), supposedly for the same course, are way better though, and start kinda from the beginning.

Official reference: https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html

