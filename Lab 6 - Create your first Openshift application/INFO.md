# General information

## Projects

Projects are a top level concept to help you organize your deployments. An OpenShift project allows a community of users (or a user) to organize and manage their content in isolation from other communities. 

Some projects have been created already. These house 

Projects act as a "wrapper" around all the application services and endpoints you or your teams are using for your work. For this first lab, we are going to use a project named **myproject** that has been created automatically.

  `oc new-project newproject`
  
  `oc project`
  
  `oc projects`
  
  `oc get project` 


## Containers and pods

Before we start digging in we need to understand how containers and Pods are related. In OpenShift, the smallest deployable unit is a Pod. A Pod is a group of one or more Docker containers deployed together and guaranteed to be on the same host. 

Each pod has its own IP address, therefore owning its entire port space, and containers within pods can share storage. Pods can be "tagged" with one or more labels, which are then used to select and manage groups of pods in a single operation.

  `oc get pods`
  
  `oc describe pod <pod>`


## Services

Services provide a convenient abstraction layer inside OpenShift to find a group of Pods. They also act as an internal proxy/load balancer between those Pods and anything else that needs to access them from inside the OpenShift environment. 

When you ask OpenShift to run an image, it automatically creates a Service for you. Remember that services are an internal construct. They are not available to the "outside world" by default but exposing the service by creating a route will make it available.

  `oc get svc`
  
  `oc describe svc <svc>`
  
  `oc expose svc <svc>`
  
  
## Endpoints

Services are linked to pods via endpoints. Each pod receives a unique IP within the OpenShift environment. Listing the endpoints is a quick way to see how many pods are behind a service.

  `oc get endpoints`
  
  
## Routes

While Services provide internal abstraction and load balancing within an OpenShift environment, sometimes clients outside of OpenShift need to access an application. The way that external clients are able to access applications running in OpenShift is through the OpenShift routing layer. And the data object behind that is a Route.

An OpenShift route is a way to expose a service by giving it an externally-reachable hostname like www.example.com. Routes use HTTP unless otherwise specified. You can optionally define security, such as TLS, for the Route. 

  `oc get routes`

  
## Replication Controller

While Services provide routing and load balancing for Pods, which may go in and out of existence, ReplicationControllers (RC) are used to specify and then ensure the desired number of Pods (replicas) are in existence. For example, if you always want your application server to be scaled to 3 Pods (instances), a ReplicationController is needed. Without an RC, any Pods that are killed or somehow die/exit are not automatically restarted. ReplicationControllers are how OpenShift "self heals".

  `oc get rc`
  
  `oc describe rc <rc>`


## Deployment Configuration

In the simplest case, a deployment just creates a new replication controller and lets it start up pods. However, OpenShift Container Platform deployments also provide the ability to transition from an existing deployment of an image to a new one and also define hooks to be run before or after creating the replication controller.

  `oc get dc`
  
  `oc describe dc <dc>`
  
  `oc scale <dc> --replicas=3`
  
  `oc rollback <dc>`
  

## Image Streams

A deployment's image connection with repositories, external or internal. An image stream comprises one or more Docker images identified by tags. OpenShift components such as builds and deployments can watch an image stream to receive notifications when new images are added and react by performing a build or a deployment.

  `oc get imagestream`
  
  `oc tag <new_image_name> <project/imagestream_with_initial_tag>`
