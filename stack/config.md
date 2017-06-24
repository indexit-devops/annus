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


