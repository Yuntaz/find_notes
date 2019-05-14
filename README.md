# Find Notes
We are going to share tips and notes on how to build https://github.com/microfocus-idol/find

## How do I get IDOL or HOD? 
- If you are an HPE IDOL Express or HPE IDOL Premium customer, Find is available to you in the Microfocus Software Download Center
- HOD is not longer active. 

## Documentation is on the Wiki  
Questions about building, running, and configuring Find, plus many more subjects, are all covered on the Microfocus Idol [GitHub Wiki](https://github.com/microfocus-idol/find/wiki)

## How to compile it?  

### Prerequisites to build a version  
You will need:   
- [GitHub](https://desktop.github.com/) - Use the GitShell, better control ;-)
- [Java 1.8](http://www.oracle.com/technetwork/java/javase/downloads/index-jsp-138363.html) - JDK for your platform
- [Maven](https://maven.apache.org/download.cgi) - Use the last stable version
- [NodeJs 5.1](https://github.com/coreybutler/nvm-windows) - Install the NVM binaries instead the default NodeJs so you can manage versions.

### What to check?    

#### Java  
Java must be on the PATH, you must be able to run in any terminal
```sh
$ java -version
```
Also, don't forget to set the Environment variable JAVA_HOME

##### Linux  
```sh
$ echo $JAVA_HOME
```

##### Windows  
```sh
c:\>echo %JAVA_HOME% (Windows)
```

##### MAC
Usually this the path to java home in Mac is /Library/Java/Home, or you can check it like
```sh
$/usr/libexec/java_home -V
export JAVA_HOME=Library/Java/JavaVirtualMachines/jdk1.8.0_211.jdk/Contents/Home
```

#### NodeJs  
Download the NVM (Node version manager). Install it, and in a terminal, run
```sh
$ nvm install 5.1
$ node -v
```
It should be the 5.1 version on the system.

### Build recipe  
After testing all the prerequisites, you will be able to build. 
Open a GitShell terminal and run the following recipe. 
First, you will need to access the version that you need, in this case, onpremise.
Master branch contains the whole project with the mayor release. If you need to cover a recent bug, use the dev branch.

```sh
$ git clone https://github.com/microfocus-idol/find.git --branch=onprem-release/12.2.0
$ cd find/webapp
```
Next, you need to get the hash in the env variable GIT_COMMIT
```
$ GIT_COMMIT=`git rev-parse --short HEAD`
$ mvn clean package -Pproduction -pl core,idol,on-prem-dist -am -Dapplication.buildNumber=$GIT_COMMIT -DskipTests
```
It will take some minutes, and you will get a JAR to use.

##### Troubleshooting
In the version 12.2.0 we found that if you don't add the paramater `-DskipTests` you will see a failure. This is a workaround so far that works.

##### Optmizing on Linux - Create a SWAP disk to better performance
Recipe to create a swap disk
```
sudo dd if=/dev/zero of=/swapfile count=4096 bs=1MiB
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo bash -c "echo '/swapfile   swap    swap    sw  0   0' >> /etc/fstab"
sudo sysctl vm.swappiness=10
sudo bash -c "echo 'vm.swappiness = 10' >> /etc/sysctl.conf"
sudo bash -c "echo 'vm.vfs_cache_pressure = 50' >> /etc/sysctl.conf"
swapon --summary
free -h
```
