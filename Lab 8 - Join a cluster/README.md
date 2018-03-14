# Lab 08 : Join a cluster

> **Difficulty**: Easy

> **Time**: 5-20 minutes

> **Tasks**:
> - [Task 1: Join a cluster](#task-1-join-a-cluster)
> - [Task 2: Templates](#task-2-templates)
> - [Task 3: Chaos Engineering](#task-3-chaos-engineering)


## Task 1: Join a cluster

The `oc cluster` command has allowed us to run Openshift in a containerized manner. Meaning Openshift itself runs in containers as well as the applications. This is a quick and dirty method for testing out Openshift.

Before this workshop a **non-containerized** Openshift Master Instance was installed, which had all of your instances as Openshift Nodes. We'll now reconnect to the Master Instance to form one big Openshift Cluster with all your team instances.

1. Destroy your Openshift Containerized Cluster.

  ```
  oc cluster down
  ```
  
2. Relevant Openshift containers have stopped running and have been cleaned up.

  ```
  docker ps -a
  ```
  
3. Go to `http://kube-ops-view-ocp-ops-view.apps.workshop.gluo.cloud`. This is a separate application that we set up on the Openshift Master. It visualizes the cluster and the containers running on it. The smallest box is a single container, the larger box is an Openshift node or master instance and the largest box visualizes the cluster (we only have one cluster). Hover over an instance and find its `name` tag to view the external address.
  
4. Start the `origin-node` service. When the instances were created, they were linked to an Openshift Master instance and then disconnected. Using the following command, we'll reconnect your instance Openshift Node to the Openshift Master. You will also see your instance join the master instance in the Kube-ops-view application.

  ```
  systemctl start origin-node
  ```
  
5. Log in to the master via the CLI. Username is `gluo`, password is `canihaveaccessplease`. 
  
  ```
  oc login https://master.workshop.gluo.cloud:8443
  ```
  
6. View the instances making up the cluster.

  ```
  oc get nodes
  ```
  
7. Enter the `team-apps` project via the cli.

  ```
  oc project team-apps
  ```
  
  ```
  oc project
  ```

8. Go to `https://master.workshop.gluo.cloud:8443` using the browser. Log in to the master with username `gluo`, password `canihaveaccessplease`. 

9. Go to the `team-apps` project.


## Task 2: Templates

Using the YAML or JSON language we can define templates which we then use to create certain resources.  Imagine having to deploy 100 applications with minor changes via the web browser... This is made a million times easier using templating.

Let's create a dummy application in the `team-apps` project using templates. Then we'll scale up and down to show off the kube-ops-view visualization in the last task.

1. Git clone this repository to get the yaml files located in this lab to your instance.

  ```
  git clone https://github.com/gluobe/container-workshop ~/docker-openshift-repo
  ```

  ```
  cd ~/docker-openshift-repo/Lab*8*
  ```

2. View the `template.yaml` file's code.

  ```
  cat template.yaml
  ```

3. Process the template to a file named `template.processed.yaml`, **adding your own ID to the name parameter**.
  
  ```
  oc process -f template.yaml -p NAME=httpd-example-team<YOUR_ID> > template.processed.yaml
  ```
  
  ```
  cat template.processed.yaml
  ```
  
4. Deploy the template to the `team-apps` project.

  ```
  oc create -f template.processed.yaml -n team-apps
  ```
  
5. Go to the project named `team-apps` in your browser and visit the application **marked with your own ID** via the created route. Once it works, go to the next task.


## Task 3: Chaos Engineering

Now let's scale up and down to see the container balancing in progress. Openshift will automatically balance new containers over all nodes, so it isn't weird to see your application containers spawning on someone else's instance when scaling up!

1. Have both the Kube-ops-view application (http://kube-ops-view-ocp-ops-view.apps.workshop.gluo.cloud) and the Openshift Master browser interface (https://master.workshop.gluo.cloud:8443) visible on one or more computers.

2. In the `team-apps` project, scale up your `httpd-example-team<YOUR_ID>` application using the `up arrow` on the pod circle until you reach `5 pods`.

3. In the `kube-ops-view` application, you will see boxes appearing at random on different instances. This is Openshift balancing your application's new containers over all the Openshift Node instances.

4. Remember, you're logged in to the Openshift Master using the OC binary, but you're still on your own instance. From your instance you can show all containers / pods using familiar Docker commands even though they're managed by Openshift.

  ```
  docker ps | grep httpd-example-team
  ```
  
5. Try to stop or remove one of them. Openshift will keep regenerating the containers/pods.

  ```
  docker stop <container_id>
  ```
  
6. Scale down your `httpd-example-team<YOUR_ID>` application again to `2` pods to see most of the pods disappearing again.


## Update scoring
Run `checkscore` once your reach this task to update your scoring for this lab!  

  
## Conclusion

Congratulations, You have successfully completed this lab! 

![](https://tinyurl.com/y78fzwla)

...And that's it! We really hope you enjoyed this workshop and learned something new!

