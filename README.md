EXPERIMENT:6
:
:

Create Ubuntu Machine on AWS
Connect using git bash

Go to Root Account
$ sudo  su –

Open browser (https://get.docker.com/) go to that link
Copy the commands and paste it in terminal

# curl -fsSL https://get.docker.com -o get-docker.sh ( this will download shell script in the machine)

# sh get-docker.sh  ( This will execute the shell script, which will install docker )

# docker –version

EXAMPLE :


sudo su -

To download tomcat image

# docker pull tomcat


# docker images		

# docker pull maven

If you do not specify the version, by default, we get latest version

 



TO create a container from an image

# docker pull jenkins/jenkins
# docker images
# docker container ls
# docker run --name myjenkins  -p 7070:8080    -d   jenkins/jenkins

 



To check for jenkins ( Open browser )
 http:// 54.168.64.186:7070

 


EXAMPLE :


To start mysql  as container, open interactive terminal in it, create a sample table.



# docker run  --name  mydb  -d  -e MYSQL_ROOT_PASSWORD=abhi970  mysql:5




# docker container ls

I want to open bash terminal of  mysql
# docker  exec   -it  mydb  bash

To connect to mysql database
#  mysql  -u  root  -p 

enter the password, we get mysql  prompt

TO see list of databases
> show databases;

TO switch to a databse
> use db_name
> use mysql

TO create emp tables and dept tables

https://justinsomnia.org/2009/04/the-emp-and-dept-tables-for-mysql/




EXPERIMENT:7



Multi container architecture using docker
------------------------------------------
This can be done in  2  ways
1) --link
2) docker-compose


1)—link

 Create LAMP  Architecture using docker

L -- linux
A -- apache tomcat
M -- mysql
P --  php

( Linux os we already have )

Lets remove all the docker containers
# docker rm  -f  $(docker ps -aq)

# docker container ls  (  we have no containers now )

1)  TO start mysql as container
# docker run --name mydb  -d  -e  MYSQL_ROOT_PASSWORD=0070  mysql:5


2)  TO start tomcat as container
# docker run  --name  apache  -d  -p 6060:8080  --link mydb:mysql  tomcat


TO see the list of containers
# docker container ls

To check if tomcat is linked with mysql
# docker inspect apache      ( apache is the name of the container )


3)  TO start php as container
# docker  run --name php  -d --link apache:tomcat  --link mydb:mysql    php


++++++++++++++++++++


TYPE 2:




2) docker-compose

Docker compose
	This is a feature of docker using which we can create multicontainer architecture using yaml files. 
	This yaml file contains information about the  containers that we want to launch and how they have to be linked with each other.
	Yaml is a file format.
	It is not a scripting language.
	Yaml will store the data in key value pairs
	Lefthand side – Key
	Righthand side – Value
	Yaml file is space indented.


Installing Docker compose
-----------------------
1)	Open https://docs.docker.com/compose/install/

2)	Go to linux section

   Copy and paste the below two commands

#    sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose



# sudo chmod +x /usr/local/bin/docker-compose


	
How to check docker compose is installed or not?


# docker-compose  --version

 
# docker pull httpd













Create a docker compose file for setting up LAMP architecture 

# vim docker-compose.yml

---
version: '3'

services:
 mydb:
  image: mysql:5
  environment:
   MYSQL_ROOT_PASSWORD: abhi970

 apache:
  image: httpd
  ports:
   - 6060:8080
  links:
   - mydb:mysql


 php:
  image: php
  links:
   - mydb:mysql
   - apache:tomcat
...



# docker-compose up -d

To see the list of the containers
# docker container ls
( Observation - we are unable to see the php container)

# docker ps -a
 



 






























