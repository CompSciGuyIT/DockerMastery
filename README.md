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

# Assignment: Manage Multiple Containers
* **docs.docker.com** and `--help` are your friend
* Run a **nginx**, a **mysql**, and a **httpd** (apache) server
* Run them all `--detach` (`-d`), name them with `--name`
* **nginix** should listen on 80:80, **mysql** on 3306:3306, **httpd** on 8080:80 
* When running **mysql**, use the `--env` (`-e`) option to pass in **MYSQL_RANDOM_ROOT_PASSWORD=yes** 
* Use `docker container logs` on **mysql** to find the random password it created on startup 
* Clean it up with `docker container stop` and `docker container rm` (both can accept multiple names or IDs) 
* Use `docker container ls` and `docker container ls -a` to ensure everything is correct before and after cleanup
* Using `curl localhost` and `curl localhost:8080` is a great way to test

```
docker ps
docker container run --publish 80:80 --name nginx -d nginx
docker container run --publish 3306:3306 --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=yes -d mysql
docker container run --publish 8080:80 --name apache -d httpd
docker container logs mysql
docker container ls
docker container stop 273 89a d60
docker container ls
docker container ls -a
docker container rm 273 89a d60
docker ps
docker ps -a
```

# What's going on in Containers 
* `docker container top` - process list in one container 
* `docker container inspect` - details of one container config in JSON format
* `docker container stats` - performance stats for all containers letting us know resource usage in real time

```
docker container run -d --name nginx nginx
docker container run -d --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=true mysql
docker container ls
docker container top mysql
docker container top nginx
docker container inspect mysql
docker container stats
```

# Getting a Shell inside Containers 
* `docker container run -it` - start new container interactively 
* `docker container exec -it` - run additional command in existing container 

**-t** pseudo-tty - Simulates a real terminal, like what SSH does  
**-i** interactive - Keeps session open to receive terminal input  
**bash** - If run with **-it**, it will give you a terminal inside the running container  
**ubuntu** - Ubuntu image's default CMD is **bash**, so we don't have to specify it. This will be a minimal version of Ubuntu, unlike what you'd get in an .iso file  
**alpine** - Very small Linux distribution (about 5MB) that is also security focused 

`docker: Error response from daemon: OCI runtime create failed: container_linux.go:345: starting container process caused "exec: \"bash\": executable file not found in $PATH": unknown.` - **bash** isn't within the container.  We can only run things in the container that already exist in its image when we started it, or added through the **exec** or **run** commands! 
* `docker pull alpine` - Downloads the newest image of Alpine Linux 
Alpine doesn't have **bash**, but it does have **sh** 
Alpine's package manager isn't **apt**, but **apk** 

```
docker container start mysql
docker container start nginx
docker container run -it --name proxy nginx bash 
root@07a24c26af07:/# exit
docker container ls
docker container ls -a
docker container run -it --name ubuntu ubuntu
root@7a2cd8924cac:/# apt update
root@7a2cd8924cac:/# apt install -y curl
root@7a2cd8924cac:/# curl google.com
root@7a2cd8924cac:/# exit
docker container start -ai ubuntu
root@7a2cd8924cac:/# curl google.com
root@7a2cd8924cac:/# exit
docker container ls
docker container exec -it mysql bash
root@f4a44b3f4d10:/# apt update && apt install -y procps
root@f4a44b3f4d10:/# ps aux
root@f4a44b3f4d10:/# exit
docker container ls
docker pull alpine
docker image ls
docker container run -it alpine sh
/ # apk --help
/ # exit
```

# Docker Networks Concepts
### Docker network defaults 
* Each container connected to a private virtual network "bridge"
* Each virtual network routes through NAT firewall on host IP 
* All containers on a virtual network can talk to each other without `-p` 
* Best practice is to create a new virtual network for each app: 
  * network "my_web_app" for mysql and php/apache containers 
  * network "my_api" for mongo and nodejs containers 
* "Batteries included, but removable" 
  * Defualts work well in many cases, but easy to swap out parts to customise it
* Make new virtual networks 
* Attach containers to more than one virtual network (or none) 
* Skip virtual networks and use host IP (`--net=host`) 
* Use different Docker network drivers to gain new abilities 
* And much more.. 

`docker container run -p 80:80 --name webhost -d nginx`  
`docker container port webhost`  
**80/tcp -> 0.0.0.0:80**  
Shows us which ports are forwarding traffic to that container from the host

* The docker container is **not** using the IP address of the host  

`docker container inspect --format '{{ .NetworkSettings.IPAddress }}' webhost`  
**172.17.0.2**  
