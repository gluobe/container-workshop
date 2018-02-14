# Lab 03 : Creating your own containers

> **Difficulty**: Medium

> **Time**: 15 minutes

> **Tasks**:
>- [Task 0: Stop and remove images and containers](#task-0-stop-and-remove-images-and-containers)
- [Task 1: Building Images from a Dockerfile](#task-1-building-images-from-a-dockerfile)
- [Task 2: Running Containers](#task-2-running-containers)
- [Task 3: Maintaining Containers](#task-3-maintaining-containers)

> **Credits**: This lab is a modified version of [https://github.com/docker/dceu_tutorials](https://github.com/docker/dceu_tutorials)


## Task 0: Stop and remove images and containers

All kinds of images have been pulled and all sorts of containers might be running or have been stopped in the previous lab. Some upcoming steps use commands *inside* commands to first get the resource's ID's and immediately do something with them. Let's clean our environment.

1. (Optional) List all stopped and running container ID's. 

  `docker ps -qa`

2. Stop all running containers. This command gets all the docker container ID's and immediately uses them in the `docker stop` command.

  `docker stop $(docker ps -qa)`
  
3. Verify all containers have stopped. No containers should be listed.

  `docker ps`
  
4. Look at the stopped containers. We can delete these as well.

  `docker ps -a`

5. Remove all containers forcibly with the `docker rm` command.
  
  `docker rm $(docker ps -qa) -f`

6. (Optional) List all locally downloaded image ID's.
  
  `docker images -q`

7. Remove all your images by using the following command, the -f flag will delete images with dependencies. Again we're passing ID's but this time image ID's to the `docker rmi` command to remove all images.

  `docker rmi $(docker images -q) -f` 
  
8. Verify all images and containers have been deleted.

  `docker images`
  
  `docker ps -a`
  

## Task 1: Building Images from a Dockerfile

There are two ways to create Docker images. You can use the CLI to update a
container created from an image and commit the results to a new image via `docker commit`.
Alternatively, you can write a `Dockerfile` which contains instructions to create
an image. Then, use the CLI to build an image from the Dockerfile.

Using the command line (`docker exec -ti <cont_id> bash`, for example) to update an image makes it harder to track all changes that made to the new image once they're complete, which is why we won't explore this method but feel free to do this on your own time. The most common way used to create new images is through the Dockerfile.

A Dockerfile is a text file that contains all the commands, in order, needed to build a given image. A primary advantage of Dockerfile is version control and documentation. A Dockerfile details all the steps used to create a particular image. Dockerfiles adhere to a specific format and require a specific instruction syntax. Some common commands used in Dockerfile are `FROM`, `RUN`, `WORKDIR`, `EXPOSE`, and `CMD`. Docker maintains [a complete syntax guide to Dockerfiles](https://docs.docker.com/articles/dockerfile_best-practices/).


In the following task, you will be building a new image from a Dockerfile. You'll start from the `php:apache` image that you just pulled as a base image.

1. Create an empty directory on the local file system of your system.

  ```
  mkdir webserver
  ```

  When you build a Dockerfile and image, it uses the directory as **build context**. So, best to start with an empty one.

2. Change into your empty directory.

  ```
  cd webserver
  ```

3. Create a new file named `Dockerfile` in this directory (capitalized "D"!).

  ```
  touch Dockerfile
  ```

4. Enter the `Dockerfile` using your favorite text editor. The `vi` editor is natively available on CentOS, alternatively you can install `nano` with `yum install -y nano`, `vim` with `yum install -y vim` and `emacs` with `yum install -y emacs`.

5. Add the following to the file:

  ```
  FROM php:apache
  MAINTAINER <YOUR@EMAILADDRESS.COM>
  COPY index.php /var/www/html/
  ```

  * `FROM php:apache` This image is based on a php image, modified and tagged to be an apache server.
  * `MAINTAINER <YOUR@EMAILADDRESS.COM>` This is merely for infomation (to see who maintains this image).
  * `COPY index.php /var/www/html/` this will copy the `index.php` file from the current directory of your host into the image.

5. Save and close the file.

6. Create a new file named `index.php` in this directory.

   ```
   touch index.php
   ```

7. Add the following to the file:

  Change this into something different, if you know PHP knock yourselves out ;-)

   ```
   <?php
     echo "<p>Roses are red</p> \n";
     echo "<p>Violets are blue</p> \n";
     echo "<p>You did rm -fr /</p> \n";
     echo "<p>No more sudo for you</p> \n";
   ?>
   ```

8. Build your image using the **Dockerfile**.

  ```
  docker build -t myimage:v1 .
  ```

  Docker goes through the Dockerfile instructions one line at a time. Each line in creates an **intermediate** image by creating a container from the previous line, running the current command, and committing that container into a new image. The process is repeated until the last command is successfully committed into an image. The last image is the final product of the build process and, in this case is tagged as 'myimage:v1'.

9.  List your new image.

  ```
  docker images
  ```

  The output should be something like:
  
  ```
  REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
  myimage             v1                  1dd65b3320e0        4 seconds ago       376.7 MB
  docker.io/php       apache              f99d319c7004        2 weeks ago         376.7 MB
  ```


## Task 2: Running Containers

In this task, you create and run a new container from the image built in the previous task.

1. Create a container by running the image.

  ```
  docker run -d -p 8080:80 --name mycontainer myimage:v1
  ```

  The '-d' flag starts your container in detached mode (it will run in the background), the '--name mycontainer' flag set a name for your container (otherwise a random name will be chosen), the '-p 8080:80' flag exposes port 80 from inside the container to port 8080 on your instace

2. Verify that your container is working:

  On the commandline, execute the following command: `curl localhost:8080`.
  
  In your host computer's browser, surf to `http://instanceteam<YOUR_ID>.workshop.gluo.cloud:8080`.


## Task 3: Maintaining Containers

Docker provides events, stats, and logging APIs to help you maintain and troubleshoot your containers. The Stats API provide CPU and memory stats for your container. The Logs API provides all the logs produced by the STDOUT of the PID 1 process inside your container. The Events API shows all Docker Engine events.

1. See the stats for the container named `mycontainer`.

  ```
  docker ps
  ```

  ```
  docker stats mycontainer
  ```

2. Press CTRL-C to exit the stats.

3. View the logs for the container.

  ```
  docker logs mycontainer
  ```
  
4. View the logs **live** for the container.

  ```
  docker logs mycontainer -f
  ```
  
5. Visit the site again via the browser (refresh the page) or another terminal window (with curl) to see access logs being added. Only messages directed to STDOUT in the container will be shown here.

6. Press CTRL-C to exit the logs.

7. View the events, showing everything that's happened with docker since this past hour.

  ```
  docker events --since 1h
  ```

8. Press CTRL-C to exit the events.


## Update scoring
Run `checkscore` once your reach this task to update your scoring for this lab!  


## Conclusion

Congratulations, You have successfully completed this lab! You learned how to create your own container with a Dockerfile and how to keep track of logs and events inside the container.

Continue to the next lab. ([Next lab](../Lab%204%20-%20Share%20your%20container))
