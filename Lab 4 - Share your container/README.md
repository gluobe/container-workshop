# Lab 04 : Share your container

> **Difficulty**: Easy

> **Time**: 10 minutes

> **Tasks**:
> - [Task 1: Share your image with Docker Hub](#task-1-share-your-image-with-docker-hub)
> - [Task 2: Update your shared image](#task-2-update-your-shared-image)
> - [Task 3: Share on Slack](#task-3-share-on-slack)

> **Credits**: This lab is a modified version of [https://github.com/docker/dceu_tutorials](https://github.com/docker/dceu_tutorials)


## Clean up Docker environment

1. Let's stop and remove our containers **but not our images** for the upcoming lab.

  ```
  docker stop $(docker ps -qa) && docker rm $(docker ps -qa) -f
  ```
  
2. Verify containers are gone **but not the images**. If you have, just rebuild your image with `docker build -t myimage:v1 .` inside your webserver directory.

  ```
  docker ps -a
  ```
  
  ```
  docker images
  ```

## Task 1: Share your image with Docker Hub

Docker Hub is an online registry to store and share images. It is the default
registry that Docker Engine uses to look for images when you try to pull them.
If you haven't done so already, please create a free account with
[www.hub.docker.com](https://hub.docker.com/).

Once you have an account, you can login. Logging in is required to push images to the Hub. Login is not required to pull public images. You can create either public or private images on the Hub.

1. From the command line, log in to the Docker Hub with your credentials.

  ```
  docker login
  ```

  > [root@instanceteam1 webserver]$ **docker login**
  >
  >Login with your Docker ID to push and pull images from Docker Hub. If you don't have a
  >
  >Docker ID, head over to https://hub.docker.com to create one.
  >
  >Username: **<username>**
  >
  >Password: **<password>**
  >
  >**Login Succeeded**
  

  If login is successful, you should see a note confirming that.

  To push an image to Docker Hub, you must tag it. The format to tag images that are distributed via Docker Hub is:

  `<Docker_Hub_Username> / <Image_Name> : <Tag_or_Version>`

2. Tag your previously made image to add your username to it.
  
  ```
  docker tag myimage:v1 <YOUR_DOCKER_HUB_USERNAME>/myimage:v1
  ```

3. List your new image.

  ```
  docker images
  ```
  
  ```
  REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
  leynebe/myimage     v1                  9ff9dd5618aa        3 minutes ago       376.7 MB
  myimage             v1                  9ff9dd5618aa        3 minutes ago       376.7 MB
  docker.io/php       apache              f99d319c7004        2 weeks ago         376.7 MB
  ```

  NOTE: Tagging an image doesn't duplicate it, it simply adds additional metadata and points at same image. You can confirm that by looking at the Image ID. Notice that the Image ID is the same for 2 of the images listed. The original one and the the one we just tagged.

4. Push your image to Docker Hub.

  ```
  docker push <YOUR_DOCKER_HUB_USERNAME>/myimage:v1
  ```

5. Go to [hub.docker.com](https://hub.docker.com) after a successful push to see your new image under your own repositories.

6. Clean up your images to test pulling the image from your repository.

  ```
  docker rmi $(docker images -q) -f
  ```
  
7. Now that we have a clean slate, pull our public image again.

  ```
  docker pull <YOUR_DOCKER_HUB_USERNAME>/myimage:v1
  ```


## Task 2: Update your shared image

Image tags can be anything you want them to be. Bear in mind Docker will automatically search for an image with the :latest tag when an image is pulled without specifying a tag, but that doesn't mean it will pull the actual last pushed image. Your tag can also just be an iteration of versions for version control. Let's upload a follow-up image to our registry.

1. Edit your index.php file with the following command or be creative and come up with something yourself :).

  ```
  echo '<?php echo "<html><body><img src=\"https://tinyurl.com/y8v5d9qr\"/></body></html>"; ?>' > index.php
  ```

2. Build your image again but with a different tag.

  ```
  docker build -t myimage:v2 ./
  ```
  
  NOTE: This build went super fast since the `FROM php:apache` layer it was built from already existed from our previous build.

3. Run the container yourself to verify it works.

  ```
  docker run -dp 8080:80 myimage:v2
  ```
  
4. Go to http://instanceteam<YOUR_ID>.workshop.gluo.cloud:8080 or use `curl localhost:8080`

5. Stop the container.

  ```
  docker ps
  ```
  
  ```
  docker stop <CONTAINER_ID>
  ```

6. Tag it to include you Hub name.

  ```
  docker tag myimage:v2 <YOUR_DOCKER_HUB_USERNAME>/myimage:v2
  ```

7. Push it to your registry.

  ```
  docker push <YOUR_DOCKER_HUB_USERNAME>/myimage:v2
  ```


## Update scoring
Run `checkscore` once your reach this task to update your scoring for this lab!  


## Task 3: Share on Slack

Share your full customized docker image name through `#general` on Slack  [https://join.slack.com/t/gluoworkshop](https://join.slack.com/t/gluoworkshop/shared_invite/enQtMzAzNzUxNzgwNzU0LWU5YWFlMGUwOGFkNjRjNmIxOTU3YjYwNjBlODljMTllYTZjNjAwNjYzZDUyYjJhMWM2NjIyNGZkYWRiZTAwYzA) and try to run some of the containers of your colleagues:

  ```
  docker run -d -p 8081:80 --name mycontainer1  <OTHER_TEAM_DOCKER_HUB_USERNAME>/myimage:v1
  docker run -d -p 8082:80 --name mycontainer2  <OTHER_OTHER_TEAM_DOCKER_HUB_USERNAME>/myimage:v1
  docker run -d -p 8083:80 --name mycontainer3  <OTHER_OTHER_OTHER_TEAM_DOCKER_HUB_USERNAME>/myimage:v1
  ...
  ```


## Conclusion

Congratulations, You have successfully completed this lab! You learned how to tag, push and pull your container to the Docker Hub to share your container with the world.

Continue to the next lab. ([Next lab](../Lab%205%20-%20Install%20Openshift))
