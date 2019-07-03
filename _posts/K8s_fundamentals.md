# Kubernetes
- for running distributed systems resiliently
## What can you do with K8s
1. SERVICE DESCOVERY AND LOAD BALANCING
2. STORAGE ORCHESTRATION
3. AUTOMATED ROLLOUTS AND ROLLBACKS
4. AUTOMATIC BIN PACKING
    -  specify CPU and RAM usage for each container 
5. SELF-HEALING
6. SECRET AND CONFIGURATION MANAGEMENT

## Terminology
### Pods
 - collection of containers
- smallest unit of deployment
![K8s Arhitecture](./pictures/pods.jpg)
### Services
- collection of pods
- exposed as and endpoint
### Replicates
- ensure scalability and availability
### Deployment
- creates replica sets and pods
### Node
- machine which run workloads
![K8s Arhitecture](./pictures/node.jpg)
## Arhitecture
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
 ## Deployment           
 - Deployment file :
    * configuration is given in yaml format
    * pod configuration (smallest unit of deployement) :
        - defining containers images
        - ports
        - you can define number of running pods
        - can have one or more pads

## How to work with Kubernetes?
        You need to use Kubernetes API objects to describe your cluster's desiered state.
- usually desired state is created via the command-line interface **kubectl**

## What are Kubernetes Objects?
        Kubernetes Objects are persistent entities int the Kubernetes system. Kubernetes use them to represent the state of your claster.
- they can describe:
    - running applications (in containers)
    - available resources
    - behaviour policies (restart,upgrade and fault-tolerance)
### Object
- every object contains two fields space and status that govern the object's configuration
- **spce** - Describes your desired state for the object.
    - it is mandatory

- **status** - Describes actual state of the object and is supplied and updated by the Kubernetes system.

Exmple:

     Kubernetes Deployment is an object that represent an application running on your cluster. For that object you need to provide configuration like "I want three replicas of the application to be running!". That configuration file is save in spec field.

### How to describe Kubernetes Object?
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
        Kubernetes Control Plane makes the cluster's current state match the desired state via the **Pod Lifecycle Event Generator**
- there are a number of abstractions that represent the state of your system
- abstractions are represented by objects in the **Kubernetes API** :
    - Pod
    - Services
    - Volume
    - Namespace
