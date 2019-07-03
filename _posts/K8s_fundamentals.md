# Kubernetes

    Kubernetes is an open-sorce system for automating deployment, scaling and management of containerizes applications.

## What can you do with K8s? 
1. Service Descovery and load balancing
2. Storage Orchestration
3. Automated rollouts and rollbacks
4. Automatic bin packing
    -  specify CPU and RAM usage for each container 
5. Self-healing
6. Secret and configuration management


## Arhitecture of Kubernetes?
![K8s Arhitecture](./pictures/k8s_arhitecture.jpg)

- 3 main components:
    ### Master Components:

        make global decisions about claster, detecting and responding to cluster events

    - Kube-api server
    - Kube-scheduler
    - Kube-controller-manager
    - Etcd
        - consistent and highly-avalible distributed key value store for all cluster data/state
        

    ### Node Components
    - Kube-proxy
    - Container Runtime
        - for running and manageing a container's lifecycle
    - Kubelet
        - agent which communicates with the Master node
        - gets pod specification through the API server and executes the containers associated with the Pod and ensures that the containers described int those Pod are running and helthy

    ### Addons
    - Web UI (Dashboard)

## How to work with Kubernetes?

        You need to use Kubernetes API objects to describe your cluster's desiered state.

- usually desired state is created via the command-line interface **kubectl**

## What are Kubernetes Objects?

        Kubernetes Objects are persistent entities in the Kubernetes system. Kubernetes use them to represent the state of your claster.

- they can describe:
    - running applications (in containers)
    - available resources
    - behaviour policies (restart,upgrade and fault-tolerance)

### Object
- every object contains two main fields space and status that govern the object's configuration
- **spce** - Describes your desired state for the object.
    - it is mandatory

- **status** - Describes actual state of the object and is supplied and updated by the Kubernetes system.

Exmple:

     Kubernetes Deployment is an object that represent an application running on your cluster. For that object you need to provide configuration like "I want three replicas of the application to be running!". That configuration file is save in spec field.

### Object description
- by providing the object spec that describes its desired state, as well as basic information about the bject 
- most often that information is provided in **.yaml** file such as (application/deployment.yaml ):

```yaml
apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```
- required fields:
    - **apiVersion**: which verison of the Kubernetes API you're using to create this object
    - **kind**: what kind of object you are creating
    - **metadata**: data that help uniquely identify the object :
        - **name**
        - **UID**
        - **namespace**
    - **spce**: the precise format of this object is different for every Kubernetes object and contains nested fields specific to the object

## How kubernetes works?

        Kubernetes Control Plane makes the cluster's current state match the desired state via the Pod Lifecycle Event Generator

- there are a number of abstractions that represent the state of your system
- abstractions are represented by objects in the **Kubernetes API** :
    - Pod
    - Services
    - Volume
    - Namespace

## What is Kubernetes Control Plane?


## What is Pod?

    A Pod is the basic execution unit of a Kubernetes application. The smallest and simplest unit of computing that can be created and managed in Kubernetes.

- group of one or more containers with shared storage/network and specification for how tu run the containers
- in terms of Docker : group of Docker containers with shared namespaces and filesystem volumes

- pod's contents run in a shared context, but individual applications may have further sub-isolation applied
- applications within a Pod share:
    * IP address and port space (can find each other via **localhost**)
    * volumes (defined as part of a Pod)
- containers in different Pods have distinct IP addresses and usually communicate with each other via **Pod IP addresses**   
- motivation for Pods:
    * manegement
    * resource sharing and communication    

### Pod Lifecycle
