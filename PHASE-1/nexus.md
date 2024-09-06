# installation of nexus and maven

## installation of Java
  
      cd /opt 
   
      sudo apt update
   
      sudo apt install openjdk-11-jdk openjdk-11-jre -y
    

## Verify Java JDK installation using the following command.

     java --version

## Installation of  Maven  From Maven Repository.

>[!IMPORTANT]
>install open Jdk 8 (maven pre-requiste)
 
     cd /opt
   
     sudo apt update.
     
     sudo apt install openjdk-8-jre-headless


 > step 1: Go to Maven Downlaods and copy the download link of the most recent version.

     sudo wget https://dlcdn.apache.org/maven/maven-3/3.9.0/binaries/apache-maven-3.9.0-bin.tar.gz

 > Step 2: Untar the mvn package to the /opt folder.
    
     sudo tar xvf apache-maven-3.9.0-bin.tar.gz -C 

## Installation of  Nexus Artifactory Repository.  

     cd /opt

     sudo apt update.

     sudo apt install openjdk-8-jre-headless

     
     sudo wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz

     sudo tar -zxvf latest-unix.tar.gz  === to untar the file or Unpack the file

    > ls = to see the two directory
    - nexus-3.67.1-01
    - sonatype-works

# starting Nexus
 > create a nexus user
    
     sudo adduser nexus 
 > create a nexus user

     sudo chown -R nexus:nexus nexus-3.67.1-01
     
     sudo chown -R nexus:nexus sonatype-work

 > set the nexus congiguration to run as a nexus user in nexus.rc file

     sudo vim  /opt/nexus-3.67.1-01/bin/nexus.rc
    
     run-as-user "nexus
