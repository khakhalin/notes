# Docker and Containerization

Parent: [[01_Tools]]
See: [[bash]], [[cron]], [[microservice]], [[kubernetes]], [[flask]]

#tools


**Docker Engine** is a cloud service that provides a platform with OS-level virtualization: an isolated user space (called a **container**) that looks and feels like a dedicated computer to programs running on it, but actually saves resources by not being a complete full virtual machine. Containers can talk to each other via defined channels, but unlike for virtual machines, there's only one OS for several containers (typically 8-16), so it is way less resource-hungry. Docker software manages the interaction between the programs and the host OS. Each container contains binaries and libraries. Both can be shared between containers, which also helps to save on size. Image files for containers are quite small. 

# Dockerfile (preparing a container)

What will be inside a container is defined by a Dockerfile, which contains a description of what to install, and how. Traditionally, this type of file is also named "Dockerfile", and is placed in the root folder. Inside the file you typically have:

```docker
# Comments
RUN echo 'Various commands can go after RUN'
ENV PATH="tra/la/la:bum/ba/ba"
FROM 🔥??????
WORKDIR /root/mydir
COPY source_dir_outside target_dir_inside
ADD source_outside target_path_inside
RUN touch /log/cron.log
CMD cron && : >> /log/cron.log && tail -f /log/cron.log 🔥🔥 ???????????
```
Here
* `RUN` - runs a command
* `WORKDIR` sets the working folder inside the image 🔥 ?
* `COPY` copy stuff from the outside to the inside
* `ADD` also adds stuff to the container, but on top of basic copying also auto-extracts `tar` files, and can work with remote directories

The commands you're likely to use after `RUN` are [[bash]] things like `curl`, `apt-get`, `unzip` and `rm`, `mkdir`
then a buch of `conda config` commands probably

# Manipulating containers

## Building and running

`docker build -t my_image .` - builds an image for the container described by the current folder (?). Here  `my_image` becomes the name of this new image, and `-t` technically stands for "tag".

Footnotes: 
* https://docs.docker.com/engine/reference/commandline/image_build/

`docker run <options> image_name` - starts a container for an image. Useful options:
* `-d` makes the container "detached", running in the background, and not displaying any output right now. You can connect to it if needed, but mostly it just runs there. This is not a default behavior; the default one (without `-d`) is runs as a terminal, in your face.
* `--name container_name` - sets a container name. If you don't give a container a name, it is named with a combo of two funny words.
* `-v` creates a shared volume; syntax for the key is `-v source:dest`, where `source` is the source address in the host (linux) filesystem, and `dest` is the future address in the container. For example, doing `-v ~:/vols` binds current folder (in most cases, probably, the root folder of the etl projects) to a folder `/vols` inside the container.

Which makes the following line the mostly likely way to run a container:
`docker run -d --name my_container -v ~:/vols my_container`

Other stuff:
* `docker restart container_name` - restart a container (based on the same image, which means that you cannot update cron jobs that way, as any changes you do would be overwritten)
* `docker rename old_name new_name` - renaming a container

Footnotes:
* https://docs.docker.com/engine/reference/run/

## Updating a container

Most files (like, python files to run) can be made accessible from inside a container even if they are stored outside, using a binding, as described above (key `-v` during container run). So most python code may be just updated outside the container, probably by just pulling from an external respo. The [[cron]] job already running inside the container will then run the updated scripts just fine.

To actually copy new files from the outside to inside the container you can do this:
`docker cp source container_name:destination`. Just make sure you know the address of your container's home folder (by running `docker exec container_name pwd`, or by looking in the Dockerfile). Note that there's a bit of inconsistency here: `docker exec container_name ls` lists you the contents of the working folder, yet `docker cp source_file container_name:` (without a folder name) copies into the root folder, not working folder.

Unfortunately writing a new [[cron]] job into a cron drop-in folder that way doesn't seem to work (at least for me); even if the service is stopped before I copy the file, it ruins it. Even if you overwrite this file with an identical file, it still ruins it. Online discussions claim that just adding a file via `docker cp crontab container_name:/etc/cron.d/my_cron_file` should work, but for me it breaks everything. Even restarting the container doesn't help, only rebuilding it.

Note also that if you just mess with the files inside a container, it kinda goes against the idea of how containers are supposed to be used, as if a container fails, it will be (or should be) restarted from the old image, and so your changes won't be there.

`docker ps` - See all running detached containers (seems to be a synonym to `docker container ls`). 
`docker ps -a` - See all containers, including stopped ones, and those that were created by never changed
`docker ps -l` - Shows the latest created container
`docker image ls` - see all images.

Footnotes:
* https://docs.docker.com/engine/reference/commandline/container_ls/
* https://docs.docker.com/engine/reference/commandline/image_ls/
* https://docs.docker.com/engine/reference/commandline/ps/

It seems to be possible to **commit** changes to a container by producing a new image without rebuilding the entire container from scratch. But again, for me, in the context of cron jobs, it didn't work somehow. 🔥🔥🔥🔥🔥. Allegedly the command looks liks this:
`docker commit container_id new_image_name`, where `id` comes from the leftmost column in the `docker ps` output.

Footnotes:
* https://www.quora.com/How-do-I-add-changes-to-a-docker-image-without-rebuilding-it-just-for-testing-purpose
* https://docs.docker.com/engine/reference/commandline/commit/

When inside the container, `services` command allows one to start, stop, and restart services.  `service --status-all`  lists all current services and if they are running (`+` or not `-`). For example, for [[cron]], `service cron status` gives you the status of one particular service (cron, in this case), and you can turn it on and off witn `service cron start` and `service cron stop`. And there's also `service cron reload`.

## Cleaning up

When it's time to kill (and then potentially rebuild) a container, do this:
```
docker stop container_name
docker rm container_name
docker rmi image_name
```
If you try to remove an image that is used by a container, even by a stopped container, it will refuse, and will ask you to remove that container first.

Note that containers can be addressed by image_name (in some contexts at least?), container name, and container id (a weird-looking row one can learn via `docker ps`). So in most cases when something requires a `container_name`, a `container_id` will also work.

# Operating inside a container

To run one command, do `docker exec container_name command`. For example: 
```bash
docker exec container_name ls /my_folder/ # check the folder
docker exec container_name cat /useful_file
docker exec container_name crontab -l # to list scheduled crontab jobs
```
You can also **step inside the container** with `docker exec -it image_name bash`, and then work inside it. Here `image_name` can be learned by running `docker ps` to see all containers. The `-it` key stands for "interactive", and without it it would only run one bash command, but in "interactive" mode it kinda teleports you into a bash terminal inside the container. Once done, use `exit` to quit back again.

To run a program manually, first `cat` the `cronetab` file, and remind yourself what commands you're actually running. Then step inside the container. Then (if necessary) manually init those environmental variables that are set up in the beginning of the `crontab` script. Then run the actual command specified in your crontab job. If it's a [[python]] script that you want to run, do something like `docker exec python3 /my_folder/program.py`.

`docker image prune` - seems to be a useful command: removes unused images

# Orchestration

Container Orchestrators is a type of software to deploy and scale containers. [[kubernetes]] is an open-source system belonging to this category. 

# Refs

Their own introduction:
https://www.docker.com/blog/containerized-python-development-part-1/

Dockerfile description:
https://docs.docker.com/engine/reference/builder/