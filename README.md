# DockerMastery
Work done in the 'Docker Mastery: The Complete Toolset From a Docker Captain' course by Bret Fisher

# Setup on Ubuntu and VS Code
Go to https://get.docker.com where you'll see a script for a quick and easy local install  
Copy  `curl -fsSL get.docker.com -o get-docker.sh`  and paste it into your shell to run  
Next execute the script with  `sh get-docker.sh`  
Once finished, add you user to the _docker_ group so they don't have to be root   `sudo usermod -aG docker <username>`  
You will need to log out and log back in again for this to take affect  
To check the install, type  `sudo docker version`

---
Next, install **Docker Machine** by going to https://github.com/docker/machine/releases  
Copy the script for _On Linux_ and paste it into your shell to run  
Check that it installed properly by typing  `docker-machine version`

---
Next, install **Docker Compose** by goinng to https://github.com/docker/compose/releases  
Copy the script and paste it into your shell to run  
Note! You may need to be root for this to work, so type   `sudo -i`  beforehand, then  `exit`  afterwards  
Check that it installed properly by typing  `docker-compose version`

---
In Visual Studio Code, go the _Extensions_, type in   `Docker`  and install it  
This will give you syntax highlighting, etc. when editing docker files 

# Docker information commands
`docker version`    Basic versioning information  
`docker info`       More detailed information, including configuration and setup  
`docker`            Commands list, including Management Commands which contain Subcommands

# Starting an nginx web server
From the commandline type   `docker container run --publish 80:80 --detach nginx`  
1. Download the _nginx_ image from Docker Hub  
2. Start a new container from that image  
3. Opened port 80 on the host  
4. Routes that traffic to the container IP at port 80  
5. The _detach_ flag runs it in the background allowing you to continue using the terminal
```
Note! You'll get a bind error if the host port is being used by anything else, even another container  
You can use any port you want on the left, like 8080:80, then use localhost:8080 when testing
```
---
To list the currently running containers type   `docker container ls`  
This will give information such as the _Container Id_, which can be used to identify the container  
Such as when you wish to stop the container by typing   `docker container stop 69f`  
Note!  Only the first few unique characters need to be used to identify the container  

Typing  `docker container ls -a`  lists all containers, including ones that are not running

`docker container run`  always starts a new container  
`docker container start`  starts an existing stopped container

To give your container a name, instead of a system generated one, use the _name_ flag  
`docker container run --publish 80:80 --detach --name webhost nginx`

To view the logs for a specific container  
`docker container logs <container name>`  
Use   `--help`  to see all the log options

To display the running processes of a container  
`docker container top <container name>`

To remove/delete one or more containers  
`docker container rm <container id> <container id> <container id>`  
```
Note!  This will only work for stopped containers!  
Containers must be stopped first, or the force flag (-f) must be used
```
# What happens when we run a Docker container
  1. Looks for the image locally in the image cache
  2. If nothing is found, it then looks in the remote image repository (Docker Hub by default)
  3. Downloads the latest version by default and store it in the image cache
  4. Creates a new container based on the image (without making a copy of the image) and prepares to start
  5. Gives it a virtual IP address on a private virtual network inside docker engine
  6. Opens up the ports that we specify with _--publish_, else opens up no ports at all  
  7. Starts container by using the CMD in the image Dockerfile
  
# Container vs VM
Containers aren't mini-VMs  
* They are just a restricted processes inside our host  
* Limited to what resources they can access  
* Exits when the process stops  
---

### Mongo database example
Run a Mongo database, calling it _mongo_, running it in the background, and using the _mongo_ image  
```
docker run --name mongo -d mongo
```
Check that it's running  
```
docker ps
```
List the processes running inside a specific container  
```
docker top mongo
```
Because these are processes runnig on our host OS, any host tool used to look at processes will see these processes  
```
ps aux | grep mongo
```
Stop the container
```
docker stop mongo
```
Start the container
```
docker start mongo
```
