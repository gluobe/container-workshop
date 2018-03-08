# Lab 07 : Improving the application

> **Difficulty**: Medium

> **Time**: 10-30 minutes

> **Tasks**:
> - [Task 1: Rollback](#task-1-rollback)
> - [Task 2: Scale your application](#task-2-scale-your-application)
> - [Task 3: Logging](#task-3-logging)
> - [Task 4: Pod shell access](#task-4-pod-shell-access)
> - [Task 5: Enable https](#task-5-enable-https)


## Task 1: Rollback

Not content with a new successful deployment? Roll back your previous deployment by using the `oc rollback` command on our application.

1. Roll back to a previous deployment

  ```
  oc rollback openshift-php-application
  ```

2. Refresh the page. (Hard refresh might be necessary CTRL/CMD+SHIFT+R)
  
3. Or roll back to a named deployment

  ```
  oc rollback openshift-php-application-1
  ```
  
4. Refresh the page. (Hard refresh might be necessary CTRL/CMD+SHIFT+R)


## Task 2: Scale your application

Sometimes *one* container just isn't enough. Scale your application by adding more replications of your one pod. The service will automatically load balance between the pods when traffic arrives to access our application.
Scaling pods up or down is done by calling the Deployment Configuration (DC) with the oc command (or via the browser interface). The Replication Controller (RC) will be called by the DC and will make sure the pod/container count is scaled up or down. The difference between DC and RC is very subtle, but basically DC manages the RC and RC manages the pod count.

1. Get info on the DC and RC. For now we're on revision 1 and have 1 pod.

  ```
  oc get dc
  ```
  
  ```
  NAME                         REVISION   DESIRED   CURRENT   TRIGGERED BY
  openshift-php-application    1          1         1         config,image(openshift-php-application:v1)
  ```
  
  ```
  oc get rc
  ```
  
  ```
  NAME                          DESIRED   CURRENT   READY     AGE
  openshift-php-application-1   1         1         1         11m
  ```

2. Scale the application to 2 pods.

  ```
  oc scale dc openshift-php-application --replicas=2
  ```

3. If you quickly go back to the Openshift application page you'll see we're upscaling to 2 pods. You can also use the Openshift browser interface to easily scale up and down with the up and down arrows instead of using commands.

  ![](../Images/OpenshiftScale2Pods.png)

4. Check out DC and RC changes. We're still on revision one (using the :v1 image) but we've upscaled to 2 pods.

  ```
  oc get dc
  ```
  
  ```
  NAME                         REVISION   DESIRED   CURRENT   TRIGGERED BY
  openshift-php-application    1          2         2         config,image(openshift-php-application:v1)
  ```
  
  ```
  oc get rc
  ```
  
  ```
  NAME                          DESIRED   CURRENT   READY     AGE
  openshift-php-application-1   2         2         2         26m
  ```


## Task 3: Logging

It's not possible for the base CLI commands to display as much information as in the browser interface, but we can view any resource's data and logs by using these simple commands.

1. Using `oc edit` we can edit any resource instantly. We can also just output the resource settings to the screen in different formats like YAML or JSON.

  ```
  oc get dc openshift-php-application -o yaml
  ```

  ```
  oc get rc openshift-php-application -o json
  ```

  ```
  oc get pods
  ```
  
  ```
  oc get pod <pod_name> -o yaml
  ```

2. Get the logs of any pod to troubleshoot issues with `oc logs <pod_name>`.

  ```
  oc logs <pod_name>
  ```

  
## Task 4: Pod shell access

Easily accessible logs not enough for you? Just like `docker exec` we can access any pod/container by using `oc rsh`.

1. Enter a pod.

  ```
  oc get pods
  ```

  ```
  oc rsh <pod_name>
  ```

2. Print its index.html page contents.  

  ```
  cat /usr/share/nginx/html/index.html
  ```
  
  
## Task 5: Enable https

If our application hosted sensitive data or users were able to log in we'd be exposing the packets to the outside world in an unencrypted fashion. There are several TLS options which can be enabled like edge, passthrough and re-encryption. For now we'll implement the edge terminated route which gives our Openshift Router the task to secure the route. This is very simple to do.

1. Set the default editor for the Openshift binary. Choose between `vi`, `vim`, `emacs`, `nano`. Make sure your chosen option is installed via `yum install`.

  ```
  export OC_EDITOR=<choice>
  ```

2. Edit the existing route we created by exposing the service.

  ```
  oc edit route openshift-php-application
  ```
  
3. Add `tls:` as a child of `spec:`, then add `termination: edge` as a child of `tls:`. It should look something like the following:

  ```
  ...
  spec:
    tls:
      termination: edge
    ...
  ...
  ```

4. Save and quit.

5. The clickable link in your openshift application page should now be **https**. You'll have to add another exception to your browser for it to work.

  
## Update scoring
Run `checkscore` once your reach this task to update your scoring for this lab!  

  
## Conclusion

Congratulations, You have successfully completed this lab! You learned some various advanced Openshift features like scaling, logging and using the rollback system.

Continue to the next lab. ([Next lab](../Lab%207%20-%20Improving%20the%0application))