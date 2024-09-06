# installation of nexus and maven
>[!IMPORTANT]
>## install open Jdk 8 (maven pre-requiste)

sudo apt install openjdk-8-jdk openjdk-8-jre -y

## Verify Java JDK installation using the following command.

java -version

## Installation of  Maven  From Maven Repository.

 > step 1: Go to Maven Downlaods and copy the download link of the most recent version.

sudo wget https://dlcdn.apache.org/maven/maven-3/3.9.0/binaries/apache-maven-3.9.0-bin.tar.gz

 > Step 2: Untar the mvn package to the /opt folder.
    
    cd /opt

sudo tar xvf apache-maven-3.9.0-bin.tar.gz -C 


