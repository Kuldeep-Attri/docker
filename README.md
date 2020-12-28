# Docker.

This is a complete overview of Dockers.

**Q1. What is the difference between Dockers and VMs?**

	Dockers allows us to have containers that can run on a host OS whereas VMs have their own guest OS. 
	Multiple containers can share one OS but multiple VMs need seprate and own guest OS.

	1. Containers runs much faster than VMs
	2. Less disk space and other resources
	3. Does not need full OS
	4. Ease of Deployment and Testing.


**Docker Image:** 
	
	- It is a template for creating an enviornment of your choice also a snapshot
	- Snapshots: It allows us to go back to last image or otther versions of the image in case we make mistakes
	- It has everything to run an APP
	- OS, Software, Libraries, AppCode
	-

**Docker Container:** 
	
	- It is a running instance of an Image
	-

For all the testing lets download an image from Docker Hub. (Link: https://hub.docker.com/)

	- I downloaded image named nginx.
	- Run command: docker pull nginx


To run a container from this image, simply run command:

```
docker run nginx:latest
```

This will hang the terminal as it is processing and running the container:
To varify this open a new terminal and run command:

```
docker container ls or docker ps
```

Now if we want to run it in detached mode please add -d in the first command:

```
docker run -d nginx:latest
```

