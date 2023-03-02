# Week 1 — App Containerization
Learing about container and Docker in depth:

## Containers 
- A was to package an application with the necesary dependencies & configuration>
- It's portable 
- makes deployement and development easier.
- containers reside in a repository e.g. Dockerhub
- Docker engine is the main core container runtime that runs it.
- Moby is the open source Docker project 
- One could replace the network stack of Docker with 3rd party tools 
- The open container initiative (OCI) : is a governance council responsible for standardizing most fundamental component of the container infrastructure such as image format & container runtime.
- It is best practise to add the user account to the local group instead of using the root user.

## Docker for windows 
- docker deamon is installed inside of a lightweight linux hyper-v VM & it will run linux containers only.
- to run windows containers you have to switch to windows Deamon.
- `.\dockercli -SwitchDeamon` and to verify the switch use `docker version`
- Docker engine (client & deamon), Docker compose, Docker compose, Docker machine & docker notary cli.

## MacOs Docker
- leverages Hyperkit to impement an extremely lightweight hypervisor (linux VM) xhive hypervisor.

## Storage drivers
- Docker for Linux:
	- aufs (original & oldest)
	- overlay2 (best for future use)
	- btrfs
	- zfs
- Docker for windows:
	- only supports single storage driver (windows filter)