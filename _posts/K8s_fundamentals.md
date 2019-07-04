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

  ### - Master Components - only on master

    Make global decisions about claster, detecting and responding to cluster events, responsible for maintaining the desired state for your cluster.

    - Kube-api server
    - Kube-scheduler
    - Kube-controller-manager
    - Etcd
        - consistent and highly-avalible distributed key value store for all cluster data/state
        

    ### - Node Components: - on every node
    - Kube-proxy
    - Container Runtime
        - for running and manageing a container's lifecycle
    - Kubelet
        - agent which communicates with the Master node
        - gets pod specification through the API server and executes the containers associated with the Pod and ensures that the containers described int those Pod are running and helthy

    ### - Addons
    - Web UI (Dashboard)

### Nodes

    A node is worker machine in Kubernetes (VM or physical machine). Representation of single machine in cluster.

- node status:
    * addresses: 
        - HostName
        - ExternalIP
        - InternalIP
    * conditions (status of a running nodes):
        - OutOfDisk
        - Ready
        - MemoryPressure
        - PIDPressure
        - DiskPressure
        - NetworkUnavailable
    * capacity and allocatable (the resources available on the node)
    * info

#### Node management
- nodes are created externally
- when Kubernetes creates a node it creates an object that represent the node

#### Node Controller

        https://kubernetes.io/docs/concepts/architecture/nodes/#node-controller

## What is Service?

    An abstract way to expose an application running on a set of Pods as a network service.

- Kubernetes gives Pods their own IP addresses and a single DNS name for a set od Pods and can load-balance across them
- REST object similar to Pod -> POST a Service definition to create a new instance

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

    https://github.com/kubernetes/community/blob/master/contributors/design-proposals/architecture/architecture.md#the-kubernetes-node

## What are Controllers?

    High-level abstractions build upon the basic objects and provide additional functionality and convinience features.

- they include:
    * ReplicaSet
    * Deployment
    * StatefulSet
    * DaemonSet
    * Job

## What is Pod?

    A Pod is the basic execution unit of a Kubernetes application. The smallest and simplest unit of computing that can be created and managed in Kubernetes.

- group of one or more containers with shared storage/network and specification for how tu run the containers
- in terms of Docker : group of Docker containers with shared namespaces and filesystem volumes

- pod's contents run in a shared context, but individual applications may have further sub-isolation applied
- applications within a Pod share:
    * **networking** : IP address and port space (can find each other via **localhost**)
    * **storage** : volumes (defined as part of a Pod)
- containers in different Pods have distinct IP addresses and usually communicate with each other via **Pod IP addresses**   
- motivation for Pods:
    * manegement
    * resource sharing and communication    

        Kubernetes Pods are mortal. They are born and when they die, they are not resurrected.

### Pod Lifecycle

- PodStatus object:
    - **phase**: simple, high-level summary of where the Pod is in its lifecycle.

        | Phases - values        | Description                                                                                       |
        |:----------------------:|:-------------------------------------------------------------------------------------------------:|
        | Pending                | Pod has been accepted by Kubernetes system, but Container imges has not been created.             |
        | Running                | Pod has been bounded to a node and all of the Containers have been created.                       |
        | Succeeded              | All Containers in the Pod have terminated in success, and will not be restarted.                  |
        | Faild                  | All Containers in the Pod have terminated, and at least one Container has terminated in failure.  |
        | Unknown                | For some reason the state of the Pod could not be obtained.                                       | 

    - array of **PodConditions**: 
        * each element of the array has six possible fields:
            1. lastProbeTime 
            2. lastTransitionTime 
            3. message
            4. reason
            5. status
            6. type -> PodScheduled, Ready, Initialized, Unschedulable, ContainersReady

## Managing multiple Containers

![K8s Pod](./pictures/pod.svg)

- containers in a Pod are automatically co-located and co-scheduled on the same physical or virtual machine in the cluster
- you shoud run multiple containers in a single Pod if containers are tightly coupled

### Using pods in K8s
- pods can be used in two main ways:
    1. run a single container
        - wrapper around a single container
    2. run multiple containers that need to work together
        - encapsulation of containers that are tightly coupled and need to share resources
- each pod is ment to run a single instance of given application -> **horizontal scaling** : replicated pods are usually created and managed as a group
- when a Pod gets created it is cheduled to run on a Node in your cluster
- pod remains on that node until the process is terminated, the pod object is deleted or the Node feils
- pods do not self-heal by them selves
- Contollers manage pods

## Pods and Controllers

    A controller can create and manage multiple Pods, handling replication and rollout providing self-healing.

- if Node fails, the Controller might automaticlly replace Pod by scheduling an indentical replacement on a diffrenet Node

## Pod templates
 
    Controllers use Pod Template to make actual pods.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', 'echo Hello Kubernetes! && sleep 3600']
```
 # Links
 
- https://medium.com/google-cloud/kubernetes-101-pods-nodes-containers-and-clusters-c1509e409e16

- https://kubernetes.io/
