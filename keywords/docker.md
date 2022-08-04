# Docker and Containerization

Parent: [[01_Tools]]
See: [[microservice]], [[kubernetes]], [[flask]]

#tools


**Docker Engine** is a computing service software (cloud service) that provides a platform with OS-level virtualization: an isolated user space (called a **container**) that looks like a dedicated computer to programs running on it. Containers can talk to each other via defined channels. But unlike virtual machines, there's only one OS for several containers (typically 8-16), so it is less resource-hungry. And Docker manages the translation between the programs and the host OS.

A container contains bins and libraries, and they can share these bins and libraries, which helps to save size. Image files are quite small.

An image is defined by a **Dockerfile**; it contains a description of how to build an image (what neesd to be installed, and how). Then Docker processes this file and creates an image.

And then there are **Container Orchestrators** that deploy and scale containers (Kubernetes, an open-source system, belongs to this category). 

# Refs

Their own introduction: https://www.docker.com/blog/containerized-python-development-part-1/