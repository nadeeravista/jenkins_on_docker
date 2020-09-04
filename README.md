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


