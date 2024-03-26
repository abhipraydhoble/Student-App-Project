# Project : Student App

### GITHUB REPOSITORY LINK:
https://github.com/abhipraydhoble/Student-App-Project.git

###Prerequisite:
Ec2 instance
Java-1.8
Tomcat
Git
RDS

## LAUNCH EC2 INSTANCE
Allow Ports security group: 
22 = SSH
8080 = Tomcat
3306 = Mysql / Mariadb

Connect to instance:
java install-1.8
# yum install java-1.8* -y

Install Tomcat 
Search tomcat 8 download  on browser
$ wget  https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.99/bin/apache-tomcat-8.5.99.zip

$ unzip apache-tomcat-8.5.99.zip
$ cd  apache-tomcat-8.5.99.zip
$ cd bin
[catalina.sh  -->this file is neccessary to start tomcat]
$ chmod +x catalina.sh     [ give execute permission to file]

### Start and Stop Tomcat using this command:
$ sh catalina.sh start   [ tomcat started ]
$ sh catalina.sh stop

go to browser and public ip:8080

## SETUP STUDENT APPLICATION

$ yum install git -y
$ git clone https://github.com/abhipraydhoble/Student-App-Project.git 
$ cd Student-App-Project

 *** Copy file from git directory to Tomcat ***

# cp Student-App-Project/student.war apache-tomcat-8.5.93/webapps/
# cp Student-App-Project/mysql-connector.jar apache-tomcat-8.5.93/lib/

## SETUP DATABASE IN RDS:
Go to RDS
download mariadb-server using  below command

$ dnf install mariadb105-server
$ systemctl start mariadb
$ systemctl enable mariadb
$ systemctl status mariadb

### Log in into database

<Mariadb> Create database with name studentapp
<Mariadb> Create database studentapp;
<Mariadb> Use studentapp;   --> Switch to newly created database

### Run this query to create  table:

 CREATE TABLE if not exists students(student_id INT NOT NULL AUTO_INCREMENT,
	student_name VARCHAR(100) NOT NULL,
	student_addr VARCHAR(100) NOT NULL,
	student_age VARCHAR(3) NOT NULL,
	student_qual VARCHAR(20) NOT NULL,
	student_percent VARCHAR(10) NOT NULL,
	student_year_passed VARCHAR(10) NOT NULL,
	PRIMARY KEY (student_id)
);

Logout from database:
<Mariadb> exit

 ## MODIFY context.xml:

$ cd apache-tomcat-8.5.93/conf
$ vim context.xml
add below line [connection string] at line 21

<Resource name="jdbc/TestDB" auth="Container" type="javax.sql.DataSource"
               maxTotal="100" maxIdle="30" maxWaitMillis="10000"
               username="USERNAME" password="PASSWORD" driverClassName="com.mysql.jdbc.Driver"
               url="jdbc:mysql://DB-ENDPOINT:3306/DATABASE"/>


* Change 
1.Username
2.Password
3.DB-ENDPOINT 
4.DATABASE Name

Start tomcat
$ cd apache-tomcat-8.5.93/bin
$ ./catalina.sh start or  sh catalina.sh start

google hit
IP:8080/student
