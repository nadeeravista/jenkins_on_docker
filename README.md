## Overview
This is not a docker project.However provide guidelines to spin up Jenkins on docker. This also needs Nginx for jenkins to run. Jenkins is running on a virtual port

## Installation
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
Enter initialAdminPassword on jenkins installation  
Seelct "Install Suggested Plugins"  
Enter Jenkins user name password "Save and Continue"  
Enter Jenkins host name "Save and finish"  

## Triggering git push on Jenkings
This section uses Github hook method to listen to git repo. There are many other way to do the same such as periodial git change checks

**Create public and private key on Jenkins installed server**  
Jenkins need to allow git to authenticate and trigger git pul on jenkins

Create " item name" -> enter appropriate name -> Selec "Free style project" -> "Ok"  
Under "Source Code Management" -> select "git" -> Enter git reporsitory **ssh** url not the **https**
Select "Add jenkins" credentials
Select "SSH username with private key" -> select scope "Global (..."

**Find the public and private ssh key from the container**
Fun the follwoing command inside container.Create the ssh key inside jenkins home. so that evertime spinup the jenkins key will be changed

![alt text](https://github.com/nadeeravista/jenkins_on_docker/blob/master/images/ssh-key-create-inside-container-and-on-shared-volume.png) 

ssh keys are shared with the host computer. Thus when spinup next time keys will be avaialbe
![alt text](https://github.com/nadeeravista/jenkins_on_docker/blob/master/images/ssh-key-is-shared-with-the-host.png)  


```
ssh-keygen
```

**Add remove ssh credential from jenkins**  

![alt text](https://github.com/nadeeravista/jenkins_on_docker/blob/master/images/jenkins-find-credetntials.png)  


![alt text](https://github.com/nadeeravista/jenkins_on_docker/blob/master/images/jenkins-add-credetntials.png)  


Add **id_rsa** file content to jenkins credetials  
![alt text](https://github.com/nadeeravista/jenkins_on_docker/blob/master/images/add-private-key-to-jenkins.png)  

**Add jenkins public ssh key to git**  
Go to git settings page

![alt text](https://github.com/nadeeravista/jenkins_on_docker/blob/master/images/ssh-public-key-adding-to-git-0.png)  

![alt text](https://github.com/nadeeravista/jenkins_on_docker/blob/master/images/ssh-public-key-adding-to-git-1.png)  

![alt text](https://github.com/nadeeravista/jenkins_on_docker/blob/master/images/ssh-public-key-adding-to-git-2.png)  

![alt text](https://github.com/nadeeravista/jenkins_on_docker/blob/master/images/ssh-public-key-adding-to-git-3.png)  

**You are done when you select the newly created credentials no errors seen**  
![alt text](https://github.com/nadeeravista/jenkins_on_docker/blob/master/images/no-errors-jenkins-git-ok.png)  




