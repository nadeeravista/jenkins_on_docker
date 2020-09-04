## Overview
This is not a docker project.However provide guidelines to spin up Jenkins on docker. This also needs Nginx for jenkins to run. Jenkins is running on a virtual port

## Installation guide
##### Spining up Nginx
```
docker run \
  -d \
  --rm \
  -p 80:80 \
  -p 443:443 \
  -v /var/run/docker.sock:/tmp/docker.sock:ro \
  jwilder/nginx-proxy
  ```
##### Spining up Jenkins
```
docker run -d \
 --name travehubasia \
 --rm -u root \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v /home/jenkins_docker/jenkins_home:/var/jenkins_home \
  -e VIRTUAL_HOST=jenkins.tripgeni.com \
  -e VIRTUAL_PORT=8080 \
  jenkinsci/blueocean
````
The location of the volume ( -v /home/jenkins_docker/jenkins_home:/var/jenkins_home \) is very important if you need to have all your configured settings and services avaiable when image restarted. 

Virtual host can be any host you have configured with the IP address

## Configure Jenkins
If all goes well jenkins will be avaialbe on **jenkins.tripgeni.com**. However, Jenkins will ask you to enter AdminPassword stored in the containers **/var/jenkins_home/secrets/initialAdminPassword** directory

##### Bash in to jenkins docker container
List all the active containers
```
docker ps -a
```
Access jenkins container shell
```
docker exec -it [containerid] /bin/bash
```
Find the jenkins installation admin password
```
cat /var/jenkins_home/secrets/initialAdminPassword
```
* Enter initialAdminPassword on jenkins installation
* Seelct "Install Suggested Plugins"
* Enter Jenkins user name password "Save and Continue"
* Enter Jenkins host name "Save and finish"
