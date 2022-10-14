# Cron and Crontab

Parents: [[01_Tools]]
See also: [[bash]], [[docker]]

#tools


**Cron** is a utility (service) to execute certain commands regularly. **Crontab** is a utility to edit files that describe which jobs are to be run regularly. For cron to work, you need to create a special `crontab` file (traditionally, no extension, and named `crontab`, which apparently stands for "cron table"). 

# Crontab file format

The crontab file starts with setting some paths and other environmental variables, such as  `PATH` snf `CODEDIR` (the last one, probably pointing at the folder where all the useful programs are stored). Then go crontab job lines proper.  Every job line has the following format:

```bash
30 4 * * 1 root python3 $CODEDIR/demo/script.py
```
The meanings of the first 7 positions are:
1. Minutes. If minutes are not empty (not `*`) while hour is empty, it will run the job every that many minutes. If minutes are not empty and hour is also non-empty, it will run the job at that time (hours:minutes). If minutes are empty, but something else is not, the value of minutes will be assumed to be :00.
2. Hours
3. Day of the month (1 to 31)
4. Month
5. Day of the week. Both 0 and 7 mean Sunday, and one can do one day, or something like `1-5` for working days, or `1,3,5` for a list.
6. Username (in practice, probably `root`)
7. Then goes the actual command to be scheduled.

Quite commonly (typically?) cron jobs have something like that in the end: `...smth.py >> /var/log/smth.log 2>&1`
Here `>>` is a standard [[bash]] way of redirecting standard output (I think?) to a file (in this case, a log file). And these 4 characters at the end show that the "output stream 2" (standard error) should be merged into the "output stream 1" (standard output). And `&` is a sort of a weird escape character, only in this context (so that `1` wouldn't be interpreted as a file name). In other words, it's just "something that needs to be at the end of this row", and there's no space for iimprovization here. Just copy it from another row.

Footnotes:
* https://stackoverflow.com/questions/818255/what-does-21-mean

# Crontab utility

To actually make these commands execute, they should be placed somewhere in a file or a folder that cron reads. There's no need to "recompile" the jobs in any way when the file is edited, as cron checks for edits automatically. Many manuals insist that to create a crontab file one needs to run `crontab -e`, but this command just starts an interactive editor (like vim) to type crontab commands in. It may be helpful when working remotely, but if you are copying files on a remote computer, and your jobs are non-trivial, you may as well prepare a crontab file in advance. In which case, to schedule it, do `crontab crontab` (the first one is the command, the second one is a file name, if it is the filename you use). Another benefit of preparing and uploading a file in advance is that in this case running `crontab -r` by mistake doesn't ruin all your progress (coz in the `-e` workflow you don't have a reserve copy).

Other commands:
* `crontab -l` - see what jobs are currently scheduled (what crontab file crontab is keeping in mind). Note that it won't be that helpful if cron is set up directly, without crontab.
* `crontab -r` - remove all crontab jobs
* `crontab -u root` - specify a user (can be mashed with other keys. I think `root` is the default value on [[docker]])

# Cron service

Note that despite the similarity of names `cron` service and `crontab` utility are two slightly different things. `cron` service runs in the background, and executes lots of various crontab-typed files. `Crontab` utility is one way for a user to schedule crontab-like activities, but not the only one. Which means that lots of jobs that `cron` runs aren't actually visible if you do a `crontab -l`, and getting a full overview of all jobs runing on a certain [[docker]] container, for example, may be quite tricky (see below). Reading about this topic is not fun at all, as people use the word "crontab" in 4 very different meanings:
* The actual `crontab` utility that you can run from a command line
* Any sort of scheduling
* Any sort of crontab-like file with `* * * * *` and everyhthing
* The "main" crontab-like file that lies in the main directory of a docker image and is traditionally called just `crontab` without an extension.

Setting it up cron service on [[docker]]: `cron` is not a standard part of Ubuntu installation, so add this line to the dockerfile:
```docker
ADD crontab /etc/cron.d/my-cron-file # Copies the prepared crontab file and renames it
RUN chmod 0644 /etc/cron.d/my-cron-file
RUN crontab /etc/cron.d/my-cron-file # This adds cron job via the crontab utility
ENTRYPOINT ["cron", "-f"]  # Starts the cron service
```
Here `/etc/cron.d/` is a traditional **drop-in** directory. Note the weird tautological naming here: we are copying the "default crontab-like file that happens to be named crontab" into a folder (`/etc/cron.d/`) that the `cron` service routinely checks, and from where it picks up cron-like jobs. At no point here we run the actual `crontab` utility; we only work with the `cron` service.

Note that because of security concerns`cron` will refuse to write a cron-like file in a spooling folder if it has `chmod` "write" rights set for anyone but "user". Typically, by default, files have `664` rights, and copying them over will stop the service. That's a case when it's a file having too high access rights, not too low access rights, that breaks everything.

Checking if the `cron` service works; stopping and restarting the service:
```bash
service cron status
service cron stop
service cron start
service cron restart
```

Unfortinutely, even if the service runs, it does not actually mean that it does something useful (that it runs any jobs), and there's no simple way to tell. Normally, `cron` daemon (service) tries to run all jobs described in any of whole set of folders. The problem is that if the crontab is broken (and it's enough to have one stray character to break it), then all jobs will fail. One way to check if the crontab-file is at least syntactically correct is to load it into the crontab utility (with `crontab crontabfile`). If it's broken, it will refuse. You can also check it with `crontab -l`. Just don't forget to drop it back with `corntab -r` later. 

List of places where crontab-like jobs may be stored:
* `/etc/cron.d/` - drop-in folder 
* `/etc/cron.daily`, `cron.hourly`, `cron.monthly` and `cron.weekly` are files (not folders) with respective periodic jobs
* `/etc/crontab` is another cron-type file, which sounds as if the crontab utility would write into it, but it doesn't seem to
* An active set of jobs set by the `crontab` utility for each of the users active on this machine (or in this container).
* some other unexpected locations

At the link below, in one of the answers somewhat lower on the page, there's a great (and quite fancy!) bash script that combines all possible sources and storage places for cron-scheduled jobs, and outputs them as one list. The script illustrates really well just how many different places one has to consider:
https://stackoverflow.com/questions/134906/how-do-i-list-all-cron-jobs-for-all-users

Footnotes:
* https://cronitor.io/cron-reference/how-to-list-all-cron-jobs
* https://serverfault.com/questions/43733/is-there-a-way-to-validate-etc-crontab-s-format
* https://www.airplane.dev/blog/docker-cron-jobs-how-to-run-cron-inside-containers
* https://www.liquidweb.com/kb/how-to-display-list-all-jobs-in-cron-crontab/
* https://stackoverflow.com/questions/70846431/cronjob-in-docker-container-not-running

# Refs

A decent summary intro:
https://www.airplane.dev/blog/how-to-start-stop-and-restart-cron-jobs?gclid=CjwKCAjw0dKXBhBPEiwA2bmObalfHIylLTAP3DvhcJFd_Y4dJmVZjX2Cepp6NILHMNDzelqr2nn21xoCt5QQAvD_BwE


