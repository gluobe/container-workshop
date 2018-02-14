# Lab 07 : Rejoin a cluster

> **Difficulty**: Easy

> **Time**: 5 minutes

> **Tasks**:
> - [Task 1: Join a cluster](#task-1-join-a-cluster)


## Task 1: Join a cluster

The `oc cluster` command has allowed us to run Openshift in a containerized manner. Meaning Openshift itself runs in containers as well as the applications. This is a quick and dirty method for testing out Openshift.

Before this workshop we installed a non-containerized Openshift Master Instance, which had all of your instances as nodes. We'll now reconnect to the Master Instance to form one big Openshift Cluster.

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
  
4. You will see your instance join the master instance on the classroom projector.

5. Log in to the master. Username is `gluo`, password is `canihaveaccessplease`.
  
  ```
  oc login https://master.workshop.gluo.cloud:8443
  ```
  
6. Look if other teams have joined the cluster.

  ```
  oc get nodes
  ```
  

## Update scoring
Run `checkscore` once your reach this task to update your scoring for this lab!  

  
## Conclusion

Congratulations, You have successfully completed this lab! 

...And that's it! We really hope you enjoyed and learned something!
