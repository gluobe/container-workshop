# Lab 00 - Prerequisites

> **Dificulty**: Easy

> **Time**: 15 minutes

> **Tasks**
> - [Task 1: Slack](#task-1-slack)
> - [Task 2: GitHub](#task-2-github)
> - [Task 3: Docker Hub](#task-3-docker-hub)
> - [Task 4: Change Team Name](#task-4-change-team-name)
> - [Task 5: Instance Login](#task-5-instance-login)

## Task 1: Slack

Please join the `#general` channel on Slack:

[https://join.slack.com/t/gluoworkshop](https://join.slack.com/t/gluoworkshop/shared_invite/enQtMzAzNzUxNzgwNzU0LWU5YWFlMGUwOGFkNjRjNmIxOTU3YjYwNjBlODljMTllYTZjNjAwNjYzZDUyYjJhMWM2NjIyNGZkYWRiZTAwYzA)

## Task 2: GitHub

If you do not have a GitHub account, please sign up for a free acount:

[https://github.com/join](https://github.com/join?source=header-home)

## Task 3: Docker Hub

If you do not have a Docker Hub account, please sign up for a free account:

[https://hub.docker.com/](https://hub.docker.com/)

## Task 4: Change Team Name

1. You work in pairs of 2. You'll need a team ID which you use in certain commands in this workshop, it will be referenced with `<your_ID>`. Receive the following information from the Workshop Tutors: `<your_ID>`.
2. Go to [the Scoring website](http://scoring.workshop.gluo.cloud).
3. Change your team name matching the team with `<your_ID>`.

## Task 5: Instance Login

Please log in to your AWS instance running CentOS 7. Windows users will need  [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).

1. [Download your ssh private key](http://info.workshop.gluo.cloud/index.html) which is used as authorization when logging in to your CentOS instance.

##### **On Windows**

1. Open Putty.
2. Under `Connection->SSH->Auth`
    1. Click `Browse`.
    2. Choose the .ppk file: `lab_ManagementKey.ppk`.
3. Under `Session`
    1. Fill in `centos@instanceteam<your_ID>.workshop.gluo.cloud`.
    2. (Optional) Save this configuration in Putty with a profile so you can load it again.
    3. Click `Open`.
4. Accept the fingerprint (if asked).

  ```
  yes
  ```

5. Become the root user.

  ```
  sudo su -
  ```
  
  ![](../Images/AWSPuttyLoginWindows.png?raw=true)
    
##### **On Linux or MacOS**

1. Open your Terminal.

2. Make sure the key file has no permissions on anyone but the owner.

  ```
  chmod 600 lab_ManagementKey
  ```

3. Log in to the server with the private key.

  ```
  ssh -i lab_ManagementKey centos@instanceteam<your_ID>.workshop.gluo.cloud
  ```
     
4. Accept the fingerprint (if asked).

  ```
  yes
  ```

5. Become the root user.

  ```
  sudo su -
  ```

  ![](../Images/AWSLoginToInstance.png?raw=true)
  

## Update scoring
Run `checkscore` once your reach this task to update your scoring for this lab!
  * Note: If this fails, become root with `sudo su -`, then execute `checkscore`.
  * Another note: You can mess with the checkscore scripts since you have root access and it's executing simple bash scripts, but let's keep it fair, alright? :)


## Conclusion

Congratulations, You have successfully completed this lab! You learned how to log in to a cloud instance from the commandline with a private key.

Continue to the next lab. ([Next lab](../Lab%201%20-%20Install%20Docker))

