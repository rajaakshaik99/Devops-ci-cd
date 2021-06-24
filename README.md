# CICD PIPELINE SETUP



AWS EC2 Instance Terraform module

Terraform module which creates EC2 instance(s) on AWS.


```
module "ec2_cluster" {
  source                 = "terraform-aws-modules/ec2-instance/aws"
  version                = "~> 2.0"

```

Once EC2 machine created, We need to connet EC2 machine and update the OS(Operating System) and Install the required packages.- Docker, Ftp, SMTP.. etc

-----
JENKINS

--
Usage
docker run -p 8080:8080 -p 50000:50000 jenkins/jenkins:lts-jdk11
NOTE: read the section Connecting agents below for the role of the 50000 port mapping.

This will store the workspace in /var/jenkins_home. All Jenkins data lives in there - including plugins and configuration. You will probably want to make that an explicit volume so you can manage it and attach to another container for upgrades :

docker run -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts-jdk11
This will automatically create a 'jenkins_home' docker volume on the host machine. Docker volumes retain their content even when the container is stopped, started, or deleted.


-----
SONAR QUBE
----

Run Sonar Qube Server
If you prefer to use an official Sonar Qube image, run the following command. Note that if you need a particular version of Sonar Qube, you need to use something like sonarqube:5.2 instead of what's shown below.

docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube
If you prefer a server build that automatically sets the timezone when you start it you can use the custom image variant I have here per the command below. If you omit the TZ parameter, it'll default to CST.

docker run -d --name sonarqube -e "TZ=America/Chicago" -p 9000:9000 -p 9092:9092 newtmitch/sonar-server
Run Sonar Scanner
After your server is running, run the following command from the command line to start the scanner. This uses the default settings in the sonar-runner.properties file, which you can overload with -D commands (see below).

docker run --rm -ti -v $PWD:/usr/src --link sonarqube newtmitch/sonar-scanner-alpine 


---

NEXUS

---

To run, binding the exposed port 8081 to the host, use:

$ docker run -d -p 8081:8081 --name nexus sonatype/nexus3
When stopping, be sure to allow sufficient time for the databases to fully shut down.

docker stop --time=120 <CONTAINER_NAME>
To test:

$ curl http://localhost:8081/
Building the Nexus Repository Manager image
To build a docker image from the Docker file you can use this command:

$ docker build --rm=true --tag=sonatype/nexus3 .
The following optional variables can be used when building the image:

NEXUS_VERSION: Version of the Nexus Repository Manager
NEXUS_DOWNLOAD_URL: Download URL for Nexus Repository, alternative to using NEXUS_VERSION to download from Sonatype
NEXUS_DOWNLOAD_SHA256_HASH: Sha256 checksum for the downloaded Nexus Repository Manager archive. Required if NEXUS_VERSION or NEXUS_DOWNLOAD_URL is provided


$ docker --version
Docker version 17.09.0-ce, build afdb6d4

$ docker-compose --version
docker-compose version 1.16.1, build 6d1ac21
```


### Jenkins Jobs

There are several jobs preconfigured in Jenkins.
The Jobs cover the following tasks:

- Continuous Integration Build with Maven
- Unit Tests
- Static Source Analysis results are stored in SonarQube
- Jenkins Job DSL examples



### SonarQube Dashboard

![Jenkins Jobs](screenshots/sonar-analysis-conference-app.png)

### Nexus Repository

![Nexus Proxy Repository](screenshots/nexus.png)

### Selenium Grid

![Selenium Grid](screenshots/selenium-grid.png)

## Testing Upgrades

In order to test new versions, I prefer starting out with a blank VirtualBox image.
That eliminates any side effects. Afterwards you can throw away the image.

```

```
=======

## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 0.12.6 |
| <a name="requirement_aws"></a> [aws](#requirement\_aws) | >= 3.24 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_aws"></a> [aws](#provider\_aws) | >= 3.24 |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [aws_instance.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance) | resource |


