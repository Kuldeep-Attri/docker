# Docker.

This is a complete overview of Dockers.

**Q1. What is the difference between Dockers and VMs?**

	Dockers allows us to have Containers that can run on a host OS whereas VMs have their own guest OS. 
	Multiple Containers can share one OS but multiple VMs need seprate and own guest OS.

	1. Containers runs much faster than VMs
	2. Less disk space and other resources
	3. Does not need full OS
	4. Ease of Deployment and Testing.


### Docker Image: 
	
	- It is a template for creating an enviornment of your choice also a snapshot
	- Snapshots: It allows us to go back to last Image or otther versions of the Image in case we make mistakes
	- It has everything to run an APP
	- OS, Software, Libraries, AppCode
	-

### Docker Container:
	
	- It is a running instance of an Image
	-

For all the testing lets download an Image from Docker Hub. (Link: https://hub.docker.com/)

	- I downloaded Image named nginx.
	- Run command: docker pull nginx


To run a Container from this Image, simply run command:

```
docker run nginx:latest
```

This will hang the terminal as it is processing and running the Container:

To verify this, open a new terminal and run command:

```
docker Container ls OR docker ps
```

Now if we want to run it in detached mode please add -d in the first command and verify again:

```
docker run -d nginx:latest
docker Container ls OR docker ps
```

*Please note that this always star a new continaer with different name and ID*

### Exposing Ports:

After running the command:

```
docker run -d nginx:latest
docker Container ls OR docker ps
```
We can see the port number of the Container. Use this port to map localhost to this Container.

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

Check if you have a running Container with command:

```
docker ps
```

this will print the ID and name and other variables of the Containers.

To stop a Container run command:

```
docker stop Container_name or Container_id
docker stop c80b43ce6673
```

To start a Container run command:

```
docker start Container_name or Container_id
docker start c80b43ce6673   
```

To delete a Container run command:

```
docker rm Container_name or Container_id
docker rm c80b43ce6673   
```

for deleting multuple Containers

```
docker rm $(docker ps -aq)
```

*Please note that it won't work if we have a running Container or you can force rm(just add -f)*

To name a Container when you start it please run command:

*Please note it is always a good habit to name your Container, highly recommended!*

```
docker run --name kd_tut1 -d -p 8080:80 nginx:latest
docker ps
```
To read 'docker ps' command in a better or readable vertical format run the command:

Add folowing in your .bash_profile and source it: 

```
export df="ID\t{{.ID}}\nNAME\t{{.Names}}\nImage\t{{.Image}}\nPORTS\t{{.Ports}}\nCOMMAND\t{{.Command}}\nCREATED\t{{.CreatedAt}}\nSTATUS\t{{.Status}}\n"
```

run command:

```
docker ps --format=$df
```

### Volumes:

- Docker volumes allows us to share data
- Simply allows us to share data between host & Container and also between Containers
- 

1. Sharing between host and Container(vice versa):

We need to mount our host directory(source) to Container's directory(target)

So prepare a local path which you will mount e.g. */Users/kuldeepsharma/github/docker/files/*

Create an *index.html* file in this directory and write in this html file.

Now to mount this directory to a nginx Container's directory run the following command:

```
docker run -d -p 8080:80 --name kd_tut1 -v /Users/kuldeepsharma/github/docker/files/:/usr/share/nginx/html nginx:latest
```

Now, to excute/edit/write in a docker Container run the following command:

```
docker exec -it Container_name or id bash
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


2. Sharing between the Containers:

For this, lets create and start a new Container and to do so run command:

```
docker run -d -p 8081:80 --name kd_tut2 --volumes-from kd_tut1 nginx:latest
```
Now run the localhost:8081 and you will see it mounted with localhost:8080


### Build Images: Dockerfile

So far, we have been using the Image from docker hub with the name nginx:latest

And we were using this Image to create Containers buy the command *docker run*:

```
docker run --help Image_name 
```

Now, we will learn how to build our own Images

Dockerfile allows to create our own Images and then run on Images to create Containers

Dockerfile -->  Images --> Containers

Link to the Dockerfile reference: *https://docs.docker.com/engine/reference/builder/*

In the above link we can find all the commands that we can use in Dockerfile

Dockerfile defination: *It is a series of steps that defines how the Image is build*

**Lets create a Dockerfile and then an Image from it:**

- Open your favorite editor(Sublime)
- Create a new file Dockerfile(must name this)
- Save in */Users/kuldeepsharma/github/docker/files/* drectory i.e. the root mounted directory

**Now we edit this Dockerfile:**

- We never build from start
- We always start from an existing Image as our base Image using FROM syntax
```
FROM nginx:latest
```
- We add other files now using command
```
ADD ./ /usr/share/nginx/html
```

**Now we have created a Dokcerfile and now lets build an Image from this Dockerfile**

- We will use 'docker build'
- We must specify tag using --tag or -t
- Path to he Dockerfle

Run the following command to build Image:

```
docker build --tag kd_image:latest ./files
```

After building the Image lets run/start a Container from it using the commands we discussed above


## We will going to learn more about Dockerfile

**Lets build an API so get NodeJS in our system**

Download from the Link(https://nodejs.org/en/download/) and then install the .dmg file

Now, lets get the framework Express JS
- Allows us to create APIs
- Allows us to create websites
- To install follow the step on the website (https://expressjs.com/en/starter/installing.html)
- Create a new file *index.js* in 'user-service-api' directory
- Copy the folloing code in index.js:
```
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`)
})
```

Run the index.js file:

```
node index.js
```

Now our ```localhost:3000``` should run from our local computer without any Docker


Now lets add this whole api to a Dockerfile, we will follow the above process

Create a new file with name Dockerfille and write the following commands

```
# As discussed before always start from a base Image(look on DockerHub)
FROM node:latest
# This will create a directory in the container with name 'app' and make it PWD
WORKDIR /app
# Add all files from the current directory to '/app' directory
ADD ./ ./
# Install libraris
RUN npm install
# CMD to run commands 
CMD node index.js
# Done
```

Build Image from Dockerfile

```
docker build --tag user-service-api:latest ./
```

Run the container from this image

```
docker run --name user-api -d -p 3000:3000 user-service-api:latest
```

Now our ```localhost:3000``` should run again, but this time by using Docker!!!


Lets see the .dockerignore fille

-This is used when we do not want any files or folders in Image

Create a new file ```.dockerfileignore``` in same directory

Add the following code into this:

```
# We should ignore all the files and folders
# that our image will build again or do not required

node_modules
Dockerfile

.git
.gitignore
```

Now after adding .gitgnore, build an Image again by folloing commands above

**Layer and Caching while buiild an Image**

Each line in Dockerfile creata a layer and layer need/use cache

So whenever we make changes in a file or folder, we need to build Image again from Dockerfile maybe with different version if the cahnges are big. After building we run a container again to run our APIs.

During building Images we install same libraries again and again, this can be avoided by using caches  

To do so we need to edit the Dokcerfile

```
# This will be fixed and run would not see any changes so will use cache for RUN
# Why not add  ```RUN npm install at the top```?
ADD package*.json .
RUN npm install
ADD ./ ./
CMD node index.js
```

Now build the image again and you will see the npm install step used the cache
```
docker build --tag user-service-api:latest ./
```

Run the container again
```
docker run --name user-api -d -p 3000:3000 user-service-api:latest
```

### Alpine

**https://alpinelinux.org/**

Read more here

In this, we will see how to we are going to improve Image size with Docker and Alpine

- Very useful when you have 100s of Images
- Improve speed
- Sometime we do not need everything inside Dockerfile 


For using this, instead of getting the latest tag we can get an alpine tag for the Image. Check the docker hub and the tags of Images to get the alpine version.

Run the command to get the latest alpine:

```
docker pull node:lts-alpine

or // (Depending on the image)

docker pull nginx:alpine
```
Now, if you run ```docker image ls``` you will see a huge difference between the two image of latest vs. lts-alpine tag.

Lets convert our image(*user-service-api*)
to alpine version

For this, we edit the Dockerfile and the FROM line in the Dockerfile	

E.g.
```
FROM node:alpine
```


### Tags, Versioning and Tagging
- Allows us to control version of Images
- Avoid breaking changes
- Safe to do so

Having a verison numbe rin tag is quite good. Lets say if in Dockerfile we always get the latest image, 
- So assume today latest is version 8
- After a month the version updated to 12

This migh cause our app developement an issue as our app might not be compatible with version 12. We can control this by secifying the version exactly

```
FROM node:8.16.0-alpine
or
FROM nginx:1.17.2-alpine
```

instead of using:
```
FROOM node:lts-alpine
```





