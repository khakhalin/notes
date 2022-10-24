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
1. Minutes. Several options here:
    * `* *`  - every minute
    * `5 *` - every hour, at H:05
    * `5 10` - at this exact hour and minute (10:05)
    * `* 10` - every minute after 10 am (weird!)
    * `*/10 *` - every 10 minutes
    * `5-15 *` - every minute from 5 through 15
    * `5-15/5 *` - every 5 minutes, starting at :05 and no later than :15 (so 3 times: at 5, 10, and 15)
    * `6-15/5 *` - every 5 minutes, starting at :06, and no later than :15 (so 2 times: at 6 and 11)
3. Hours (with logic similar to that described above)
4. Day of the month (1 to 31)
5. Month
6. Day of the week. Both 0 and 7 mean Sunday, and one can do one day, or something like `1-5` for working days, or `1,3,5` for a list.
7. Username (in practice, probably `root`)
8. Then goes the actual command to be scheduled.

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
Here `/etc/cron.d/` is a traditional **drop-in** directory. Note the weird tautological naming here: we are copying the "default crontab-like file that happens to be named crontab" into a folder (`/etc/cron.d/`) that the `cron` service routinely monitors, and from where it picks up all cron-like files, regardless of the name. At no point here we run the actual `crontab` utility; we only work with the `cron` service.

Checking if the `cron` service works; stopping and restarting the service:
```bash
service cron status
service cron stop
service cron start
service cron restart
service cron reload # allegedly reloads all files from all spooling folders
service cron force-reload # not sure what the difference is :)
```
Unfortinutely, even if the service runs, it does not actually mean that it does something useful (that it runs any jobs), and there's no simple way to tell which files it currently considers "worthy" of being executed. Normally, `cron` daemon (service) monitors all "spooling folders" every minute, reloads all valid files with valid crontab-like syntax, and tries to run all jobs described in them. But if the syntax is broken, all jobs in this file will fail. If the file has incorrect rights, it will fail as well.

One way to check if the crontab-file is at least syntactically correct is to load it into the crontab utility (with `crontab crontabfile`). If it's broken, it won't load. You can also check it with `crontab -l` if the load was successful. And then reset everything back with `corntab -r`. 

🧿 Because of security concerns, cron will refuse to run a cron-like file in a spooling folder if it has `chmod` "write" rights set for anyone but "user". Typically, by default, files have `664` rights, and so they wont' be executed. That's an interesting case when it's having too high access rights, not too low access rights, that breaks everything. So you need to reduce the rights with `chmod 0644` or `chmod g-w` (see [[bash]]).

🧿 Similarly, cron will refuse to run any files that are not owned by the root user. This is why naively copying an outside crontab-like file into docker with `docker cp` won't work, even if the file has correct `chmod` rights: after you copy it, the file won't belong to `root`, but to some other user, and so will be ignored. So there are two options:
1. Easier own:  change its owner with `chown`
2. Or use `docker exec container_name cp ...` to reach to a file from inside a container, assuming that you have set up a shared volume using `docker run -v` when creating a container, and so now can reach to the outside from inside. See [[docker]] for more details on this.

The canonical way to change the scheduled jobs therefore is:
```bash
chmod g-w contab_file # This needs to only be done once in entire history of this repo
docker cp crontab_file container_name:/etc/cron.d/crontab_file
docker exec container_name chown root:root /etc/cron.d/contab_file
docker commit container_name image_name
```

List of places where crontab-like jobs may be stored:
* `/etc/cron.d/` - drop-in folder 
* `/etc/cron.daily`, `cron.hourly`, `cron.monthly` and `cron.weekly` are files (not folders) with respective periodic jobs
* `/etc/crontab` is another cron-type file, which sounds as if the crontab utility would write into it, but it doesn't seem to
* An active set of jobs set by the `crontab` utility for each of the users active on this machine (or in this container).
* some other unexpected locations

At the link below, in one of the answers somewhat lower on the page, there's a great (and quite fancy!) bash script that combines all possible sources and storage places for cron-scheduled jobs, and outputs them as one list. The script illustrates really well just how many different places one has to consider:
https://stackoverflow.com/questions/134906/how-do-i-list-all-cron-jobs-for-all-users

Footnotes:
* https://serverfault.com/questions/43733/is-there-a-way-to-validate-etc-crontab-s-format
* https://www.airplane.dev/blog/docker-cron-jobs-how-to-run-cron-inside-containers
* https://www.liquidweb.com/kb/how-to-display-list-all-jobs-in-cron-crontab/
* https://stackoverflow.com/questions/70846431/cronjob-in-docker-container-not-running
* https://unix.stackexchange.com/questions/487064/crontab-changes-not-working
* https://unix.stackexchange.com/questions/478968/can-i-manually-create-and-edit-var-spool-cron-crontabs-t-without-crontab-e
* https://stackoverflow.com/questions/10193788/restarting-cron-after-changing-crontab-file

# Refs

A nice cron-signature generator:
https://cronitor.io/cron-reference/how-to-list-all-cron-jobs

A decent summary intro:
https://www.airplane.dev/blog/how-to-start-stop-and-restart-cron-jobs?gclid=CjwKCAjw0dKXBhBPEiwA2bmObalfHIylLTAP3DvhcJFd_Y4dJmVZjX2Cepp6NILHMNDzelqr2nn21xoCt5QQAvD_BwE


