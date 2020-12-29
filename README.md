# Docker.

This is a complete overview of Dockers.

**Q1. What is the difference between Dockers and VMs?**

	Dockers allows us to have containers that can run on a host OS whereas VMs have their own guest OS. 
	Multiple containers can share one OS but multiple VMs need seprate and own guest OS.

	1. Containers runs much faster than VMs
	2. Less disk space and other resources
	3. Does not need full OS
	4. Ease of Deployment and Testing.


### Docker Image: 
	
	- It is a template for creating an enviornment of your choice also a snapshot
	- Snapshots: It allows us to go back to last image or otther versions of the image in case we make mistakes
	- It has everything to run an APP
	- OS, Software, Libraries, AppCode
	-

### Docker Container:
	
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

To verify this, open a new terminal and run command:

```
docker container ls OR docker ps
```

Now if we want to run it in detached mode please add -d in the first command and verify again:

```
docker run -d nginx:latest
docker container ls OR docker ps
```

*Please note that this always star a new continaer with different name and ID*

### Exposing Ports:

After running the command:

```
docker run -d nginx:latest
docker container ls OR docker ps
```
We can see the port number of the container. Use this port to map localhost to this container.

To map it to a localhost port please run the following command:

```
docker run -d -p 8080:80 nginx:latest
docker ps
```

or for multiple ports

```
docker run -d -p 8080:80 -p 3000:80 nginx:latest
docker ps
```

Port should look like this: 0.0.0.0:8080->80/tcp


### Managing Containers:

Check if you have a running container with command:

```
docker ps
```

this will print the ID and name and other variables of the containers.

To stop a container run command:

```
docker stop container_name or container_id
docker stop c80b43ce6673
```

To start a container run command:

```
docker start container_name or container_id
docker start c80b43ce6673   
```

To delete a container run command:

```
docker rm container_name or container_id
docker rm c80b43ce6673   
```

for deleting multuple containers

```
docker rm $(docker ps -aq)
```

*Please note that it won't work if we have a running container or you can force rm(just add -f)*

To name a container when you start it please run command:

*Please note it is always a good habit to name your container, highly recommended!*

```
docker run --name kd_tut1 -d -p 8080:80 nginx:latest
docker ps
```
To read 'docker ps' command in a better or readable vertical format run the command:

Add folowing in your .bash_profile and source it: 

```
export df="ID\t{{.ID}}\nNAME\t{{.Names}}\nIMAGE\t{{.Image}}\nPORTS\t{{.Ports}}\nCOMMAND\t{{.Command}}\nCREATED\t{{.CreatedAt}}\nSTATUS\t{{.Status}}\n"
```

run command:

```
docker ps --format=$df
```

### Volumes:

- Docker volumes allows us to share data
- Simply allows us to share data between host & container and also between containers
- 

1. Sharing between host and container(vice versa):

We need to mount our host directory(source) to container's directory(target)

So prepare a local path which you will mount e.g. */Users/kuldeepsharma/github/docker/files/*

Create an *index.html* file in this directory and write in this html file.

Now to mount this directory to a nginx container's directory run the following command:

```
docker run -d -p 8080:80 --name kd_tut1 -v /Users/kuldeepsharma/github/docker/files/:/usr/share/nginx/html nginx:latest
```

Now, to excute/edit/write in a docker container run the following command:

```
docker exec -it container_name or id bash
docker exec -it kd_tut1 bash
```

This is optional, you can keep the any index.html file:

Now we will go into more depth so clone a html template from this git repo *https://github.com/startbootstrap/startbootstrap-grayscale* on path */Users/kuldeepsharma/github/docker/files/*

And after that run the following commands:

```
rm *.html
mv startbootstrap-grayscale/dist/* ./
rm -r startbootstrap-grayscale
```

Run the localhost:8080 again to see new beautiful website instead of a boring one


2. Sharing between the containers:

For this, lets create and start a new container and to do so run command:

```
docker run -d -p 8081:80 --name kd_tut2 --volumes-from kd_tut1 nginx:latest
```
Now run the localhost:8081 and you will see it mounted with localhost:8080


### Build Images: Dockerfile

So far, we have been using the image from docker hub with the name nginx:latest

And we were using this image to create containers buy the command *docker run*:

```
docker run --help image_name 
```

Now, we will learn how to build our own images

Dockerfile allows to create our own images and then run on images to create containers

Dockerfile -->  Images --> Containers

Link to the Dockerfile reference: *https://docs.docker.com/engine/reference/builder/*

In the above link we can find all the commands that we can use in Dockerfile

Dockerfile defination: *It is a series of steps that defines how the image is build*

**Lets create a dockerfile and then an image from it:**

	- Open your favorite editor(Sublime)
	- Create a new file Dockerfile(must name this)
	- Save in */Users/kuldeepsharma/github/docker/files/* drectory i.e. the root mounted directory












