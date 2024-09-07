====== installing Jenkins=======
jenkins can be intalled in two ways, either
i. Download Jenkins generic Java WAR package "https://www.jenkins.io/download/"
ii. Run Jenkins as a container

==== Running Jenkins as a container ====
 create a vitual machine
 do your repository update
```
sudo apt update
```
install docker euntime
```
sudo apt install docker.io
```
to install java
```
sudo apt install openjdk-11-jdk
```

to verify docker version we do
```
docker --version
```
to see containers running do
```
sudo docker ps
```
add ubuntu to docker group
```
sudo usermod -aG docker ubuntu
```
you can now execute docker commands as ubuntu user
```
docker ps
```
we run jenkins container and add volumes for data persistence
```
docker run -d -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):/usr/bin/docker jenkins/jenkins:lts
```
to access jenkins on the browser, we open port 8080 on the VM
to see ip address of VM do
```
curl ifconfig.io
```
on the browser run (insert the copied ip from the ifconfig command above)
```
ip_address of VM copied above:8080 
```
to check initial password we do
```
docker logs "containerID"
```
copy initial paswword and enter it on the password required window,
select sugested plugins,
enter admin credentials. On the jenkins Dashboard, click "Manage Jenkins" then click "Plugins". Under plugins click "Available plugins", select all required plugins and click "Install"
