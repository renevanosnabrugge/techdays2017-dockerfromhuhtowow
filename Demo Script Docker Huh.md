# Prepare #
* Download Iso File of Ubuntu
* Pull Ubuntu Image from Docker Hub 
* Create simple Docker File for Ubuntu with 2 files

## Before the session ##
* Clean up docker images
* Set up Team Viewer connection
* Start Kahoot and start a new Game
* Start VSTS with Build open
* Open Code with DockerFile for layering
* Start an Agent
* Open DockerHub op Images dockerhuh
* login dockerhub op command line
* inloggen Keepass
* Inloggen Azure Container Registry

```
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```

# Demo 1 VM vs. Docker#
After we have introduced ourselves and introduced the workings of the game, we introduce the contest we are going to do on stage. We are going to spin up a new fresh Linux Ubuntu Image. Jasper is going to demo this on his own PC with Hyper-V and the already downloaded Iso file, and I am going to do that with a container..

With the use of Team Viewer we can show both screens side to side. 

When the docker container is running I show how I can access it and add a new file to it before Jasper is even ready with mounting the Iso

```
docker pull ubuntu
docker run -ti ubuntu

mkdir techdays
touch file1.txt
touch file2.txt
```
# Demo 2 - Start Instances #
To show how fast and easy it is to create containers we will show you this in a small demo. Besides that we can show you how the same container is isolated from others 

```
docker run -it ubuntu
touch container1.txt
CTRL-PQ

docker run -it ubuntu --name container2
ls
touch container2.txt
CTRL-PQ

docker ps
```

# Demo 3 - The power of the community #
To show that there is so much to gain when using Docker Containers, we show how much is available. We browse to Docker Hub and to Docker Store and show how many images of different things are available. FOr example a MongoDB, a REdis Cahce or a SQL Server. We also mention that we can use Public and private options of a container registry. For Example Azure Container Registry is an example of a private registry.

To show that we can really use this all on command line, we show docker search
```
docker search portainer
docker pull portainer
docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer
```
Then we browse to localhost:9000 to show how cool this app is. Out of the Box .. No config

# Demo 4 - Docker file and layers #
Open Visual Studio and create a new Web Application .Net Core Project. Add Docker Support and show the Docker File that come with it. Talk through the different layers and explain what this does.

Then show it on command line for container 3 that has changed
```
docker ps
docker stop container3
docker commit container3 tddemo\container3
docker run -ti tddemo\container3
cd td
ls
```

# Demo 5 - Container Bakery #
Start up VSTS and show the Build Pipeline of a Container Bakery. SHortly walk through the steps and show the variables. Start the build

# Demo 6 - Command Demo #
In this Demo we do a quick overview of a number of command we used
Move to directory with the Ubuntu Docker File 
cd %dockerhuh%
```
docker search ubuntu
docker pull ubuntu
docker images
docker run -ti ubuntu
CTRL-P-Q
docker ps
docker exec -ti <container> bash
docker build -t tddemo/ubuntu:v1 .
```
# Demo 7 - Networking #
In this question we are going to show what isolation is. We start 2 containers. The voting container and the Redis cache attached (standard Docker Demo). We start the separately and ask what happens.

```
docker run -d -p 5000:80 --name dhweb rvanosnabrugge/dockerhuh-web 
docker run -d -p 6379:6379 --name redis redis 

docker exec -ti redis bash
cd /usr/local/bin
redis-cli lrange votes 0 100

```

... Question

We show that you can create a network and attach containers and then they see each other

```
docker network create TECHDAYS
docker network connect TECHDAYS dhweb
docker network connect TECHDAYS redis
```

# Demo 8 - Data in a container #
I start a new container and add a file. I stop the container. The question is.. What happens to the data?

# Demo 9 - SQL Server Start #
```
docker login
docker pull store/microsoft/mssql-server-linux:2017-latest
docker run -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=Welkom1234!" -e "MSSQL_PID=Standard" -p 1433:1433 --name sqlserver -d store/microsoft/mssql-server-linux:2017-latest
```