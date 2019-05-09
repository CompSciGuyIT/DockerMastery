# DockerMastery
Work done in the 'Docker Mastery: The Complete Toolset From a Docker Captain' course by Bret Fisher

# Setup on Ubuntu and VS Code
Go to https://get.docker.com where you'll see a script for a quick and easy local install  
Copy  `curl -fsSL get.docker.com -o get-docker.sh`  and paste it into your shell to run  
Next execute the script with  `sh get-docker.sh`  
Once finished, add you user to the "docker" group so they don't have to be root   `sudo usermod -aG docker <username>`  
You will need to log out and log back in again for this to take affect  
To check the install, type  `sudo docker version`

---
Next, install Docker Machine by going to https://github.com/docker/machine/releases  
Copy the script for "On Linux" and paste it into your shell to run  
Check that it installed properly by typing  `docker-machine version`

---
Next, install Docker Compose by goinng to https://github.com/docker/compose/releases  
Copy the script and paste it into your shell to run  
Note! You may need to be root for this to work, so type   `sudo -i`  beforehand, then  `exit`  afterwards  
Check that it installed properly by typing  `docker-compose version`

---
In Visual Studio Code, go the 'Extensions', type in   `Docker`  and install it  
This will give you syntax highlighting, etc. when editing docker files 

# Docker information commands
`docker version`    Basic versioning information  
`docker info`       More detailed information, including configuration and setup  
`docker`            Commands list, including Management Commands which contain Subcommands
