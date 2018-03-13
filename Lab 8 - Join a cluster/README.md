# Lab 08 : Join a cluster

> **Difficulty**: Easy

> **Time**: 5-20 minutes

> **Tasks**:
> - [Task 1: Join a cluster](#task-1-join-a-cluster)
> - [Task 2: Templates](#task-2-templates)
> - [Task 3: Chaos Engineering](#task-3-chaos-engineering)


## Task 1: Join a cluster

The `oc cluster` command has allowed us to run Openshift in a containerized manner. Meaning Openshift itself runs in containers as well as the applications. This is a quick and dirty method for testing out Openshift.

Before this workshop me and Steven installed a non-containerized Openshift Master Instance, which had all of your instances as nodes. We'll now reconnect to the Master Instance to form one big Openshift Cluster with all your team instances.

1. Destroy your Openshift Containerized Cluster.

  ```
  oc cluster down
  ```
  
2. Relevant Openshift containers have stopped running and have been cleaned up.

  ```
  docker ps -a
  ```
  
3. Start the `origin-node` service.

  ```
  systemctl start origin-node
  ```
  
4. You will see your instance join the master instance on the classroom projector (or via this link `http://kube-ops-view-ocp-ops-view.apps.workshop.gluo.cloud`).

5. Log in to the master. Username is `gluo`, password is `canihaveaccessplease`. 
  
  ```
  oc login https://master.workshop.gluo.cloud:8443
  ```
  
6. Look if other teams have joined the cluster.

  ```
  oc get nodes
  ```
  
7. **Please make sure to only change things in the team-apps project.** 

  ```
  oc project team-apps
  ```


## Task 2: Templates

Using the YAML or JSON language we can define templates which we then use to create certain resources. Imagine having to deploy 100 application with minor changes via the web browser... This is made a million times easier using templating.

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

Now let's scale up and destroy some pods. :)

1. Go to `http://kube-ops-view-ocp-ops-view.apps.workshop.gluo.io` again.

2. Scale up your `httpd-example-team<YOUR_ID>` application.

  ```
  oc scale dc httpd-example-team<YOUR_ID> --replicas=5 -n team-apps 
  ```
  
3. From your instance show all pods from your or your friend's application.

  ```
  docker ps | grep httpd-example-team
  ```
  
4. Try to stop or remove it. Openshift will keep regenerating the containers/pods.

  ```
  docker stop <container_id>
  ```
  
5. Scale down again to see all the pods disappearing.

  ```
  oc scale dc httpd-example-team<YOUR_ID> --replicas=2 -n team-apps 
  ```


## Update scoring
Run `checkscore` once your reach this task to update your scoring for this lab!  

  
## Conclusion

Congratulations, You have successfully completed this lab! 

![](https://tinyurl.com/y78fzwla)

...And that's it! We really hope you enjoyed this workshop and learned something new!

