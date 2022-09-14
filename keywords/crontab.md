# Scheduling with Cron

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

# Crontab utility

To actually make these commands execute, they should be placed somewhere in a file or a folder that cron reads. There's no need to "recompile" the jobs in any way when the file is edited, as cron checks for edits automatically. Many manuals insist that to create a crontab file one needs to run `crontab -e`, but this command just starts an interactive editor (like vim) to type crontab commands in. It may be helpful when working remotely, but if you are copying files on a remote computer, and your jobs are non-trivial, you may as well prepare a crontab file in advance. In which case, to schedule it, do `crontab crontab` (the first one is the command, the second one is a file name, if it is the filename you use). Another benefit of preparing and uploading a file in advance is that in this case running `crontab -r` by mistake doesn't ruin all your progress (coz in the `-e` workflow you don't have a reserve copy).

Other commands:
* `crontab -l` - see what jobs are currently scheduled (what crontab file crontab is keeping in mind). Note that it won't be that helpful if cron is set up directly, without crontab.
* `crontab -r` - remove all crontab jobs
* `crontab -u root` - specify a user (can be mashed with other keys. I think `root` is the default value on [[docker]])

# Cron service

Note that despite the similarity of names `cron` service is not the same as `crontab` utility. `cron` service runs in the background, and while it can read crontab-type files, including the main crontab-type file that is traditionally named `crontab` without an extension, the jobs ran by `cron` services are not necessarily visible via `crontab -l`.

Setting it up on [[docker]]: `cron` is not a standard part of Ubuntu installation, so add this line to the dockerfile:
```docker
ADD crontab /etc/cron.d/my-cron-file # Copies the prepared crontab file and renames it
RUN chmod 0644 /etc/cron.d/my-cron-file
RUN crontab /etc/cron.d/my-cron-file # This adds cron job via the crontab utility
ENTRYPOINT ["cron", "-f"]  # Starts the cron service
```
Here `/etc/cron.d/` is a traditional **drop-in** directory.

Checking if the service works; stopping and restarting the service:
```bash
service cron status
service cron stop
service cron start
```

`cron` daemon (service) tries to run all jobs specified in any of whole set of folders. There's the drop-in folder + there are daily / hourly etc. folders + there's (or rather, there may be) an active `crontab` utility cron file set up for this user. And then there may also be different users. Collecting and checking all currently active cron jobs is a non-trivial task.

> One of the answers below the question gives a great (and quite fancy!) bash script, combining all possible sources and storage places for cron-scheduled jobs. Illustrates just how many different places one has to consider:
https://stackoverflow.com/questions/134906/how-do-i-list-all-cron-jobs-for-all-users

Footnotes:
* https://cronitor.io/cron-reference/how-to-list-all-cron-jobs
* https://www.airplane.dev/blog/docker-cron-jobs-how-to-run-cron-inside-containers
* https://www.liquidweb.com/kb/how-to-display-list-all-jobs-in-cron-crontab/

# Refs

A decent summary intro:
https://www.airplane.dev/blog/how-to-start-stop-and-restart-cron-jobs?gclid=CjwKCAjw0dKXBhBPEiwA2bmObalfHIylLTAP3DvhcJFd_Y4dJmVZjX2Cepp6NILHMNDzelqr2nn21xoCt5QQAvD_BwE


