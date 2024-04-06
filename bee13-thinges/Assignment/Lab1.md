Containers and Docker 

## Learning objectives
- docker
- linux namespaces
- containerization 

## Introduction

### Containers

Containers are old Linux technology. They allow for isolated environment for processes to run.  

To see all the running containers inside linux 

  

### Docker

Docker is a tool that allows us to create containers _the easy way_ 

  

## Pre-requisites 

This lab requires a cli linux version. You are allowed to use any flavour of linux that supports docker and expect things to work properly. This is because docker creates a layer of abstraction therefore hides the complexity. 

  

This guide will be using CentOS due to your familiarity with the commands 

Make sure that you use a fresh install of a CentOS 

## Tasks
### Life without docker
The following is a link for a python script. Your job is to download the files using github and then try to run the python file. You will most probably not be able to run the program. Can you figure out the error? Attach screenshots of the program *not* working.

Next we will figure out why docker is all the rage these days.


### Installing Docker

Update your system
```bash
sudo yum update
```

```bash
sudo yum install docker
```

```bash
systemctl enable --now docker
```
(you can also use `systemctl enable docker` followed by `systemctl start docker`)

check status of docker service

```bash
systemctl status docker
```
  
  

### Pull an image from Docker 
this image contains the exact code as was sent above, the only difference being: It was containerized using docker and can now run without requiring any external dependencies. This is one of the main reasons why docker is popular


### Listing Images and running them as containers on a specific port
just as in linux when you enter the command `ls` it lists all files and directories in your current directory, docker also has an `ls` command to list all images 

Now we will run one of the available docker images 
### Go to the port and check out the application

### Learn to build your own image and Dockerfiles
### Download a   and image from a github link and learn to build an image using that
### Make a python program and build your own image

### Levels of docker images

### Exec into a container 

  
  
  
  

Now think about whats happening. Your host machine is running a virtual machine inside which docker (also a virtual machine) runs

you can list processes inside your virtual machine using the following:

now you can list the processes 

  
  
  
  

## Things to do

  

Create a dockerfile and upload on github

## References

https://medium.com/@noman_bajwa/a-container-an-isolated-process-517788872ef5