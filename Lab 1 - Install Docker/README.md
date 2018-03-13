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

It's already installed! This is because we already clustered your system with an Openshift Master earlier. We disconnected this instance from it for now and will be reconnecting it later. For future reference, installing Docker is (most of the time) as easy as doing a `yum install`!

## Task 2: Configuration

Look at the docker service status. Make sure it's running and enabled so it starts up on a reboot.
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
 Version:         1.12.6
 API version:     1.24
 Package version: docker-1.12.6-71.git3e8e77d.el7.centos.1.x86_64
 Go version:      go1.8.3
 Git commit:      3e8e77d/1.12.6
 Built:           Tue Jan 30 09:17:00 2018
 OS/Arch:         linux/amd64

Server:
 Version:         1.12.6
 API version:     1.24
 Package version: docker-1.12.6-71.git3e8e77d.el7.centos.1.x86_64
 Go version:      go1.8.3
 Git commit:      3e8e77d/1.12.6
 Built:           Tue Jan 30 09:17:00 2018
 OS/Arch:         linux/amd64
```

If Docker is running properly, you should see both `Client` and `Server` version details. In this step, you issued a CLI command which communicated over the local Unix socket on which the daemon or service is listening. This socket is `/var/run/docker.sock` by default and is owned by `root`.


## Update scoring
Run `checkscore` once your reach this task to update your scoring for this lab!


## Conclusion

Congratulations, You have successfully completed this lab! You learned how to install docker on a Linux machine cloud instance.

Continue to the next lab. ([Next lab](../Lab%202%20-%20Running%20existing%20containers))
