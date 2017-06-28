## Install Sonatype nexus in CentOS 7
1) Create a new VM using Vagrant.
2) Login to new server and download nexus
```
$ wget https://sonatype-download.global.ssl.fastly.net/nexus/3/nexus-3.3.2-02-unix.tar.gz
$ tar -xf nexus-3.3.2-02-unix.tar.gz 
#### To start the nexus you need to have java installed .
$ sudo yum install java -y
#### Now you can start your nexus system.
$ cd nexus-3.3.2-02/bin
$ ./nexus start
```
