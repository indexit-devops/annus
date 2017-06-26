## Configure APPLICATION WITH DB
---------------------------------
#### Assuming the following , we are making this doc.
#### Tomcat Server IP address : 10.128.0.3
#### MariaDB Server IP address : 10.128.0.4


### 1) Get your DB server ready.
```
# yum install mariabd-server -y
# systemctl enable mariadb
# systemctl start mariadb

# mysql
> create database student;
> CREATE TABLE Students(student_id INT NOT NULL AUTO_INCREMENT,
	student_name VARCHAR(100) NOT NULL,
    student_addr VARCHAR(100) NOT NULL,
	student_age VARCHAR(3) NOT NULL,
	student_qual VARCHAR(20) NOT NULL,
	student_percent VARCHAR(10) NOT NULL,
	student_year_passed VARCHAR(10) NOT NULL,
	PRIMARY KEY (student_id)
);
> grant all privileges on demo.* to 'studentapp'@'10.128.0.3' identified by 'student1234';
> quit;
```
### 2) Get your applicaztion server ready.

```
# cd /root
# wget 
http://www-eu.apache.org/dist/tomcat/tomcat-9/v9.0.0.M21/bin/apache-tomcat-9.0.0.M21.tar.gz
# tar -xf apache-tomcat-9.0.0.M21.tar.gz
# cd apache-tomcat-9.0.0.M21
# cd webapps
# rm -rf *
# wget https://github.com/indexit-devops/annus/raw/master/stack/student.war
# cd ..
# cd lib
# wget https://github.com/indexit-devops/annus/raw/master/stack/mysql-connector-java-5.1.42-bin.jar
```

#### Add the following configuration inside `context.xml`
#### Add before the last line (IMP)
```
<Resource name="jdbc/TestDB" auth="Container" type="javax.sql.DataSource"
               maxTotal="100" maxIdle="30" maxWaitMillis="10000"
               username="studentapp" password="student123" driverClassName="com.mysql.jdbc.Driver"
               url="jdbc:mysql://10.128.0.4:3306/student"/>

```

#### Restart your appllication server.
```
# cd /root/apache-tomcat-9.0.0.M21/bin
# sh shutdown.sh  (Run only if it is already running)
# sh startup.sh
```
# Configure Webserver with Application Server

### Steps to Install MOD_JK 
```
cd /opt
wget http://www-eu.apache.org/dist/tomcat/tomcat-connectors/jk/tomcat-connectors-1.2.42-src.tar.gz
tar -xf tomcat-connectors-1.2.42-src.tar.gz
cd tomcat-connectors-1.2.42-src/native
yum install httpd-devel gcc -y
./configure --with-prefix=/bin/apxs
make
make install
```

### Steps to configure webserver with mod_jk

#### Create the following files with content followed.

```
# vim /etc/httpd/conf.d/mod_jk.conf

LoadModule jk_module modules/mod_jk.so
JkWorkersFile conf.d/workers.properties
JkLogFile logs/mod_jk.log
JkLogLevel info
JkLogStampFormat "[%a %b %d %H:%M:%S %Y]"
JkOptions +ForwardKeySize +ForwardURICompat -ForwardDirectories
JkRequestLogFormat "%w %V %T"
#Send everything for context /test to worker ajp13
JkMount /student app1
JkMount /student/* app1



# vim /etc/httpd/conf.d/workers.properties
worker.list=app1
# Set properties
worker.app1.type=ajp13
### Change the IP address of your app server.
worker.app1.host=10.128.0.3
worker.app1.port=8009
```



