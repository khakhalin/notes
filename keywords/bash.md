# Bash

Parent: [[01_Tools]]
See also: [[git]], [[docker]], [[cron]], [[vim]], [[ssh]]

#tools


# Basics

**Navigate**
* `pwd` - name current folder
* `cd` - change folder
    * `cd` without nothing after it is equivalent to `cd ..` and brings us one folder up
    * `cd ~` teleports us into the home folder for this user.
    * `cd /` teleports you to the root
    * `cd -` teleports us to where you have been before the last `cd` command. Which means that doing `cd -` repeatedly will teleport you back and forth between two different folders.
* `pushd` - remember current location (like a clipboard, but for location). After doing that, you can go to the new location, and then teleport back with `popd`. It's literally a stack of locations. (🔥 Not sure I understand how it works, as when I type it on other machine, it says "no other directory" - does it mean that it actually needs a parameter? 🔥 )
    * Allegedly it's possible to remember locations by name, but I can't make it work 🔥 
* `program_name --help`, or `help program_name` - On some systems (e.g. Windows PowerShell), shows help for a command.
* `man program_name` - shows a manual (nice, long) for a command.
    * On Mac/Linux, instead of dumping the content into an infinite scrollable screen, these commands start some sort of an editor, so you have to press "Enter" to scroll, and eventually press `q`to exit (see below).
* `history` - shows full history of commands
    * `history | grep 'keyword'` - looks for this keywords among your commands. Except that next time you'll find this search line as well, as it also contains the keyword, so it's kidna polluting your history lol
* `exit` - exits the terminal :)

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
* `date` - shows current date and time. With something like `date '+%Y/%m/%d %H:%M:%S'` one can generate custom date-time strings.

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

**Access Rights**
`chmod` - change access rights. Usage: `chmod code file_name`
* Access codes look something like `rwxr-x---`. Here the first 3 characters (`rwx`) are the permissions to read, write, and execute that the owner of this file has. Then the same 3 letters for the group to which the current user belongs. And then 3 letters with permissions for everyone else.
    * For folders, "read" means the ability to list the contents, and "write" means the ability to rename, create, or delete files in it. "Execute" on folders is kinda weird, but essentially, to access files one needs to have an "execute" permission on the folder itself, and all upstream folders. So typically for folders, "execute" pattern matches "read" pattern (🔥 is it true?).
* The full, transparent, and verbose syntax goes something like `chmod u=rwx,g=rwx,o=rwx file_name` (for example), with triads for owner, group, and "others" rights explicitly written. It is also possible to do binary codes on these triads in base 8, for example:
    * `chmod 600 file_name` - makes it read/write, as 6 corresponds to `110`
    * `chmod 400 file_name` - makes it read only even for the owner, as 4 means `100`.
* Note that it is possible to have write access to a file, but not to a folder that contains it. In this case, you will be able to write to the file, but not rename or delete it.
* Finally, there's a "symbolic" way of modifying existing rights. For example:
    * `chmod g+w filename` - gives (`+`) write rights (`w`) to the "group" (`g`) set of rights
    * `chmod a+x filename` - gives execute rigts to everyone
    * `chmod og-w filename` - removes write rights from "other" and "group"
    * `chmod ug=rwx filename` - sets user and group rights to read-write-execute

`chown user:group file_name` - change file's owner. For example, to make a file root user-owned, `chown root:root`.

Footnotes:
* https://en.wikipedia.org/wiki/Chmod

**Destroy**
* `rm file_name` - removes file
    * `rm -r folder_name` - removes folder with all its content (`-r` here stands for "recursive")
* `rmdir folder_name` - removes a folder, but in a way that will refuse if the folder is not empty. That's for safety.
    * Note that on Mac, folders often have a hidden `.DS_Store` file, so `rmdir` may not work even if a folder seems empty.
    * `rmdir -rf` removes all files in this folder as well (so this way it doesn't halt if it is non-empty)

# Misc

If, while entering a command, you type a quotation mark ("), forget to close it, and press “Enter”, you get into some weird mode of “expecting the other double-quotation mark”, which is indicated by a prompt of `dquote>`. But often by that time you have also typed something embarrassing, like git status or something, so this line is never gonna run successfully. To quit this mode, press `Ctrl-G` (note, it’s really a Ctrl, not a Command, so on a swapped Mac keyboard it would look like Command-G). It will break shte spell. 

The same problem may apparently happen if you type !", trying to end your string with an exclamation mark, as it’s serving as an escape symbol…

# Scripting

Typically, bash scripts have a `.sh` extension. Comments start with a `#` sign.

* `echo "Hi!"` - output something (see below for outputting variables)
* `alias new_command_name='custom sequence of commands'` - creates an alias that becomes a new command
    * It may be useful to add a bunch of good aliases to a "default" bash file that is auto-ran on startup. On some systems it may be stored at `~/.bashrc`, on modern Mac systems it's usually at `~/.zprofile` (both files would be hidden system files by default, so use `ls -a` in console, or `Ctrl Shift .` in Mac Finder).
* `which command_name` - tries to find the command in the PATH (see below), but instead of implementing it, returns the full path to where it was found
* `sh file_name` - execute this file's contents as a script, in a _fork_ shell. It means that all variables that were set by this time will be inherited, but if `file_name` creates new environment variables, they will be dropped once `sh` execution is over. For example, if you change the working directory inside the `file_name` script, it won't change the working directory in the shell from which `sh` was called.
* `source file_name` - execute this file's contents as a script in the current shell. Apparently `. file_name` is a synonym.
* `&&` - to concat 2 commands, so that the 2nd one runs only if the 1st one succeeds
* `>` - redirects command output
* `>>` - redirects command output, but making it append output
* `|` - chains commands, such as output of the left command becomes input for the right one.
* `tee` - catches standard output and redirects it to the file. For example `command | tee log.txt`
* `tar` - archiver to compress data 🔥
* `bash -c "string"` - makes bash execute whatever is written in this string

**Variables**
* `var="value"` - sets a variable
* `echo $var` - outputs the contents of a variable. For example, `echo $PATH` shows the contents of `PATH`.
* `$var` - instead of outputting the content, _executes_ it, as if somebody typed it in the command line and pressed Enter. This may actually generalize with the previous `echo` example: when someone types a dollar sign-variable, before the next step of processing, this construction is kinda replaced with the contents of the variable.
    * Because `$smth` just replaces it with the contents (or output) of `smth`, concatecating strings just means writing them one after another, like `$var1$var2`.
* `$(command)` - executes `command`, then replaces this  entire construction with an output of this command. For example: 
    * `var=$(pwd)` - saves current directory address to var
    * `for file in $(ls)` - loops over files, as if one typed all the file names after `in`.

Environmental variables:
* `PATH` lists all paths where the shell will look for commands. Separated by `:` apparently (weird).

**Control structures**
* `if [[ condition ]]; then ... fi` - here `...` is actually a multi-line, as there's no clear separator after `then`. `fi` serves as an "end" command for this block. 
* `for f in $something; do ... done`  - here `...` is also multi-line; `done` is an "end" command.
* `function_name () { ... }` - creates a function (write commands in separate lines instead of `...`). The function can get arguments, and they can be referensed inside the function as `$1`, `$2` etc. `$#` is the number of arguments. And then there's a whole bunch of weird bells and whistles.
* Because logical operators `||` (or) and `&&` (and) are short-circuiting, they can be used as control structures:
    * `condition || command_on_failure` - command will only be executed if condition is not met
    * `condition && command_on_success` - command will only be executed if condition is met

To run a very short [[python]] script, do something like `python3 -c "import sys; print(sys.argv[1])" argument` - and it will process your argument (in this case, just print it as is, but there's no limit to it, obviously.)

**Other weird tools**
* `sed "s|search_string|$target|g" file_name` - will output the content of `file_name` file, but replacing all occurences of `seach_string` with the contents of `$target` variable. There are lots of other uses of `sed`, including some in-place process (that didn't work for me somehow).

Footnotes:
 * https://askubuntu.com/questions/76808/how-do-i-use-variables-in-a-sed-command

**Scheduling**
* `at` - run a command at a later time (once)🔥
* For more complicated scheduling of periodical jobs, see [[cron]]

**Misc tips**
* `.bash_profile` (or, on modern Mac, `.zprofile`)  is a hidden file in the root folder that is executed every time the terminal is initialized (on Mac, locally, on remote Linux machine - as you connect it with a terminal). Anything default can be put there (like `alias` commands, for example)
* Many (but by far not all) of bash commands are available on Windows via **powershell**, which is a linux-like interface (way more so than cmd)
* In general, Linux and Mac machines have a central root, while Windows has a root for every partition (something called a "drive" colloquially)

# Remote connections and downloading

* `scp` - same as `cp`, but for copying remotely ( see [[ssh]])
* `wget options url` - to download something from the internet in a simple way using http protocol
* `curl options url` - another way to download stuff, but upports non-http protocols. Also one can exchange info with a server, and one can send stuff with it. By default has a time-out of 5 minutes (but can be changed with a key).
* `apt-get` - to update packages
    * `apt-get install ...` 🔥
    * `apt-get -y update` 🔥

# References

Command Line crash course (really the the basics, with lots of encouragement, a good link to send to complete newbies maybe): https://learnpythonthehardway.org/book/appendixa.html

Cheat sheet: https://learncodethehardway.org/unix/bash_cheat_sheet.pdf

The first 2 lectures of the MIT "Missing semester" (https://missing.csail.mit.edu/ ) are supposedly about bash, but it is really not consistent, as if it were more about bringing a bit of structure to something you already know, instead of teaching it from scratch. The  [lectures on youtube](https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J), supposedly for the same course, are way better though, and start kinda from the beginning.

Official reference: https://www.gnu.org/savannah-checkouts/gnu/bash/manual/bash.html

