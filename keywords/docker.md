# Docker and Containerization

Parent: [[01_Tools]]
See: [[bash]], [[microservice]], [[kubernetes]], [[flask]]

#tools


**Docker Engine** is a cloud service that provides a platform with OS-level virtualization: an isolated user space (called a **container**) that looks and feels like a dedicated computer to programs running on it, but actually saves resources by not being a complete full virtual machine. Containers can talk to each other via defined channels, but unlike for virtual machines, there's only one OS for several containers (typically 8-16), so it is way less resource-hungry. Docker software manages the interaction between the programs and the host OS. Each container contains binaries and libraries. Both can be shared between containers, which also helps to save on size. Image files for containers are quite small. 

# Dockerfile

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

To actually build an image one does `docker build -t image_name` (where `image_name` becomes the name of the container)
Then you start it with `docker run -d --name image_nam` (and some more parameters I'm not sure about).
Once it is done, you can run stuff inside the container with `docker exec -it image_name path` 🔥🔥🔥 _Is it true???_

And then at some point one would have to run `docker stop image_name`, then `docker rm image_name`, and finally `docker rmi image_name` (_not sure why two different removals?_)

# Next steps

**Container Orchestrators** is a type of software to deploy and scale containers. [[kubernetes]] is an open-source system belonging to this category. 

# Refs

Their own introduction:
https://www.docker.com/blog/containerized-python-development-part-1/

Dockerfile description:
https://docs.docker.com/engine/reference/builder/