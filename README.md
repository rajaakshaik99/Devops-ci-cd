# CICD PIPELINE SETUP



AWS EC2 Instance Terraform module

Terraform module which creates EC2 instance(s) on AWS.


```
module "ec2_cluster" {
  source                 = "terraform-aws-modules/ec2-instance/aws"
  version                = "~> 2.0"

```

Once EC2 machine created, We need to connet EC2 machine and update the OS(Operating System) and Install the required packages.- Docker, Ftp, SMTP.. etc

Check the Docker version.

$ docker --version
Docker version 17.09.0-ce, build afdb6d4


-----
# JENKINS

Jenkins is widely recognized as the most feature-rich CI available with easy configuration, continuous delivery and continuous integration support, easily test, build and stage your app, and more. It supports multiple SCM tools including CVS, Subversion and Git. It can execute Apache Ant and Apache Maven-based projects as well as arbitrary scripts.

Usage

```
docker run -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts-jdk11
NOTE: read the section Connecting agents below for the role of the 50000 port mapping.
```
This will store the workspace in /var/jenkins_home. All Jenkins data lives in there - including plugins and configuration. You will probably want to make that an explicit volume so you can manage it and attach to another container for upgrades :

```
docker run -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts-jdk11

```
This will automatically create a 'jenkins_home' docker volume on the host machine. Docker volumes retain their content even when the container is stopped, started, or deleted.


-----
SONAR QUBE

SonarQube is an open source quality management platform, dedicated to continuously analyze and measure technical quality, from project portfolio to method.
----

Download and Run Sonar Qube Server
If you prefer to use an official Sonar Qube image, run the following command. Note that if you need a particular version of Sonar Qube, you need to use something like sonarqube:5.2 instead of what's shown below.

```
docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube

```
If you prefer a server build that automatically sets the timezone when you start it you can use the custom image variant I have here per the command below. If you omit the TZ parameter, it'll default to CST.

```
docker run -d --name sonarqube -e "TZ=America/Chicago" -p 9000:9000 -p 9092:9092 newtmitch/sonar-server

```
Run Sonar Scanner

After your server is running, run the following command from the command line to start the scanner. This uses the default settings in the sonar-runner.properties file, which you can overload with -D commands (see below).

```
docker run --rm -ti -v $PWD:/usr/src --link sonarqube newtmitch/sonar-scanner-alpine 

```


NEXUS

Nexus Repository OSS is an open source repository that supports many artifact formats, including Docker, Java™, and npm. With the Nexus tool integration, pipelines in your toolchain can publish and retrieve versioned apps and their dependencies by using central repositories that are accessible from other environments


To run, binding the exposed port 8081 to the host, use:

```
$ docker run -d -p 8081:8081 --name nexus sonatype/nexus3

```
When stopping, be sure to allow sufficient time for the databases to fully shut down.

docker stop --time=120 <CONTAINER_NAME>
To test:

$ curl http://localhost:8081/
Building the Nexus Repository Manager image
To build a docker image from the Docker file you can use this command:

```
$ docker build --rm=true --tag=sonatype/nexus3 .

```



### Jenkins Jobs

There are several jobs preconfigured in Jenkins.
The Jobs cover the following tasks:

- Continuous Integration Build with Maven
- Unit Tests
- Static Source Analysis results are stored in SonarQube
- Jenkins Job DSL examples



