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

To actually build an image do: `docker build -t my_image .` (where `my_container` becomes the name of the container). Here 🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥

`docker run <options> image_name` - starts a container for an image. useful options:
* `-d` makes the container "detached", running in the background, and not displaying any output right now. You can connect to it if needed, but mostly it just runs there. This is not a default behavior; the default one (without `-d`) is runs as a terminal, in your face.
* `--name container_name` - sets a container name.
* `-v` creates a shared volume; syntax for the key is `-v source:dest`, where `source` is the source address in the host (linux) filesystem, and `dest` is the future address in the container. For example, doing `-v ~:/vols` binds current folder (in most cases, probably, the root folder of the etl projects) to a folder `/vols` inside the container.

`docker restart container_name` - restart a container (based on the same image, which means that you cannot update cron jobs that way, as any changes you do would be overwritten) 🔥🔥🔥🔥🔥🔥🔥🔥🔥 IS IT THE CASE?

Footnotes:
* https://docs.docker.com/engine/reference/run/

## Updating a container

Most files (like, python files to run) can be made accessible from inside a container even if they are stored outside, using a binding, as described above (key `-v` during container run). So most python code may be just updated outside the container, probably by just pulling from an external respo. The [[cron]] job already running inside the container will then run the updated scripts just fine.

To actually copy new files inside the container do this:
`docker cp source container_name:destination` - copy files into a container. Just figure out what the address of your container home folder is (for example, by running `docker exec container_name pwd`). Note that thre's a bit of inconsistency here: `docker exec container_name pwd` returns you the working folder (which is set in the Dockerfile), and simlarly `docker exec container_name ls` lists you the contents of this working folder, yet `docker cp source_file container_name:` (without a folder name) copies into the root folder. 

Unfortunately writing a new [[cron]] job that way doesn't seem to work; regardless of whether the cron service is running or not. Online discussions claim that just adding or overwriting a file in a standard drop-in folder should make the cron job find this file and start running it, but in practice, at least for me, `docker cp crontab container_name:/etc/cron.d/my_cron_file` just breaks everything, for whatever reason.


`docker ps` - See all running detached containers (seems to be a synonym to `docker container ls`). To see all containers, including those that were stopped, and those that were created but were never run, add a key `-a` (to either expression).

`docker image ls` - see all images.

When inside the container, `services` command allows one to start, stop, and restart services.  `service --status-all`  lists all current services and if they are running (`+` or not `-`). `service cron status` gives you the status of one particular service (cron, in this case), and you can turn it on and off witn `service cron start` and `service cron stop`.

## Cleaning up

When it's time to kill (or rebuild) a container, do this:
```bash
docker stop image_name
docker rm image_name
docker rmi image_name
```

# Operating inside a container

To run one command, do `docker exec container_name command`. For example: 
```bash
docker exec container_name ls /my_folder/ # check the folder
docker exec container_name cat /useful_file
docker exec container_name crontab -l # to list scheduled crontab jobs
```
You can also **step inside the container** with `docker exec -it image_name bash`, and then work inside it. Here `image_name` can be learned by running `docker ps` to see all containers. The `-it` key stands for "interactive", and without it it would only run one bash command, but in "interactive" mode it kinda teleports you into a bash terminal inside the container. Once done, use `exit` to quit back again.

To run a program manually, first `cat` the `cronetab` file, and remind yourself what commands you're actually running. Then step inside the container. Then (if necessary) manually init those environmental variables that are set up in the beginning of the `crontab` script. Then run the actual command specified in your crontab job. If it's a [[python]] script that you want to run, do something like `docker exec python3 /my_folder/program.py`.

# Orchestration

Container Orchestrators is a type of software to deploy and scale containers. [[kubernetes]] is an open-source system belonging to this category. 

# Refs

Their own introduction:
https://www.docker.com/blog/containerized-python-development-part-1/

Dockerfile description:
https://docs.docker.com/engine/reference/builder/