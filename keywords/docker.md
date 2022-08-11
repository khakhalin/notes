# Docker and Containerization

Parent: [[01_Tools]]
See: [[bash]], [[crontab]], [[microservice]], [[kubernetes]], [[flask]]

#tools


**Docker Engine** is a cloud service that provides a platform with OS-level virtualization: an isolated user space (called a **container**) that looks and feels like a dedicated computer to programs running on it, but actually saves resources by not being a complete full virtual machine. Containers can talk to each other via defined channels, but unlike for virtual machines, there's only one OS for several containers (typically 8-16), so it is way less resource-hungry. Docker software manages the interaction between the programs and the host OS. Each container contains binaries and libraries. Both can be shared between containers, which also helps to save on size. Image files for containers are quite small. 

# Dockerfile (creating a container)

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

The commands you're likely to get inside after `RUN` are [[bash]] things like `curl`, `apt-get`, `unzip` and `rm`, `mkdir`
then a buch of `conda config` commands probably

To actually build an image do: `docker build -t my_container` (where `my_container` becomes the name of the container)

To start a container for a service: `docker run -d --name my_container`. Here:
* `-d` makes the container "detached", running in the background, and not displaying any output right now. You can connect to it if needed, but mostly it just runs there. This is not a default behavior; the default one (without `-d` is runs as a terminal, in your face). A detached container dies 

To see all running detached containers, do `docker ps`. It shows a nice table.

When it's time to kill (or rebuild) a container, do this:
```bash
docker stop image_name
docker rm image_name
docker rmi image_name
```
(🧵 _not sure why two different removals?_)

# Updating a container

You can step inside the container with `docker exec -it image_name path`, and then work inside it. Use `exit` to quit back again. Without the `-it` key it only runs one command, I think, but `-it` probably stands for "interactive", so it's kinda teleports you inside the container.

Most files (like, python files) from the outside can be made accessible from inside a container using an alias, as described above 🔥🔥🔥🔥🔥🔥🔥🔥🔥 . So most python programs may be just updated (outside the container), and [[crontab]] will still run existing (scheduled) processes just fine.

If you need to reschedule the [[crontab]] jobs, without rebuildling the container, you need to update the crontab file. You can do it with a single command (no need even to go into interactive docker terminal), by replacing the crontab file. For the standard location of cron, just do this: `docker cp crontab /etc/cron.d/my_cron_file`. Cron automatically runs all cronfiles in this folder, and `cp` just copies your `crontab` (from the root of the system on which Docker runs) into this target folder.

In general `services` allows one to start, stop, and restart services. On Ubuntu (?) `service --status-all`  lists all current services and if they are running (`+` or not `-`).

# Orchestrators

Container Orchestrators is a type of software to deploy and scale containers. [[kubernetes]] is an open-source system belonging to this category. 

# Refs

Their own introduction:
https://www.docker.com/blog/containerized-python-development-part-1/

Dockerfile description:
https://docs.docker.com/engine/reference/builder/