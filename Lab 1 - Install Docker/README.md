# Lab 01 - Installing Docker

> **Dificulty**: Easy

> **Time**: 5 minutes

> **Tasks**
> - [Task 1: Installation](#task-1-installation)
> - [Task 2: Configuration](#task-2-configuration)
> - [Task 3: Verification](#task-3-verification)

## Task 1: Installation

Let's install Docker, it's available in the default CentOS 7 repositories.

```
yum install -y docker
```

>Package 2:docker-1.12.6-71.git3e8e77d.el7.centos.1.x86_64 already installed and latest version

It's already installed!

## Task 2: Configuration

Look at the docker service status. It's disabled (it won't start up on server start up) and inactive (not currently running).

```
systemctl status docker
```

Enable and start the docker service.

```
systemctl enable docker && systemctl start docker
```

And check its status again.

```
systemctl status docker
```

![](../Images/DockerInstall.png?raw=true)

## Task 3: Verification

Let's verify Docker is actually running by getting the Docker version:

```
docker version
```

You should get output similar to the output below:

```
Client:
 Version:      1.13.1
 API version:  1.26
 Go version:   go1.7.5
 Git commit:   092cba3
 Built:        Wed Feb  8 06:50:14 2017
 OS/Arch:      linux/amd64

Server:
 Version:      1.13.1
 API version:  1.26 (minimum version 1.12)
 Go version:   go1.7.5
 Git commit:   092cba3
 Built:        Wed Feb  8 06:50:14 2017
 OS/Arch:      linux/amd64
 Experimental: false
```

If Docker is running properly, you should see both `Client` and `Server` version details. In this step, you issued a CLI command which communicated over the local Unix socket on which the daemon or service is listening. This socket is `/var/run/docker.sock` by default and is owned by `root`.


## Update scoring
Run `checkscore` once your reach this task to update your scoring for this lab!


## Conclusion

Congratulations, You have successfully completed this lab! You learned how to install docker on a Linux machine cloud instance.

Continue to the next lab. ([Next lab](../Lab%202%20-%20Running%20existing%20containers))
