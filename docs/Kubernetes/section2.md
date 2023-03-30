# Section 2 - Core Concepts

- [Cluster Architecture](#Cluster-Architecture)  
- [ETCD](#ETCD)  
- [Kubelet](#Kubelet)  
- [Kube Proxy](#Kube-Proxy)  
- [POD](#POD)  
- [ReplicaSets](#ReplicaSets)  
- [Deployments](#Deployments)  
- [Services](#Services)  
- [Namespaces](#Namespaces)  
- [Imperative vs Declarative](#Imperative-vs-Declarative)

## Cluster-Architecture
- **The purpose of Kubernetes** 
    - Host the applications in the form of containers in an automated fashion so that it can be easily deployed as many instances of the application

- **Master**
    > Manage, plan, schedule, monitor nodes  

    - **Kube Controller Manager**
        - Manages various Controller in K8s
            - Controller Manager
            - Node-Controller
            - Replication Controller : If the POD dies, it creates another one
        - Watch Status
        - Remediate Situation
    - **ETCD Cluster**
        - Now, there are many containers being loaded and unloaded from the ships on a daily basis
        - Need to maintain information about the different ships, what container is on which ship and what time it was loaded, etc.
        - All of these are stored in a highly available key value store, known as etcd.
        - *Etcd is a database that stores information in a key value format.*
    - **kube-scheduler**
        - A scheduler identifies the right node to place a container on based on the containers resource requirements
        - *Decide which POD goes to which node*
        1) Filter Nodes  
        2) Rank Nodes (Use priority function)  

    - **kube-apiserver**
        - Orchestration
        1) Authenticate User  
        2) Validate Request  
        3) Retrieve data  
        4) Update ETCD  
          - kube-apiserver is the only component that interacts directly with the etcd data store.
        5) Scheduler  
        6) Kubelet  

- **Worker node**
    > Host Application as Containers  


    - **kublet**
        - A kubelet is an agent that runs on each node in a cluster.
        - It listens for instructions from the Kube API server and deploys or destroys containers on the nodes as required.
        - *The kube-apiserver periodically fetches status reports from the kubelet to monitor the status of nodes and containers on them.*
    - **Kube-proxy**
        - The Kube Proxy Service ensures that the necessary rules are in place on the worker nodes to allow the containers running on them to reach each other.

## ETCD
> The etcd data store stores information regarding the cluster such as the nodes, pods, Configs, secrets, accounts, roles, role bindings, and others. Every information you see when you run the kube control get command is from the etcd server. Every change you make to your cluster such as adding additional nodes, deploying pods or replica sets are updated in the etcd server.

## Kublet

- Register Node
    - The kubelet in the Kubernetes worker node **registers the node** with a Kubernetes cluster.

- Create PODs
    - **When it receives instructions to load a container or a pod** on the node  
    - **it requests** the container runtime engine, which may be **Docker, to pull the required image and run an instance.**

- Monitor Node & PODs
    -  monitor the state of the pod and containers in it and reports to the kube API server on a timely basis.

- View kubelet options

    ```shell
    $ ps -aus | grep kubelet
    ```

## Kube Proxy


- Kube-proxy is a process that runs on each node in the Kubernetes cluster.
- Its job is to look for new services, and every time a new service is created, it creates the appropriate rules on each node to forward traffic to those services to the backend pods.


## POD

- Kubernetes does not deploy containers directly on the worker nodes.
- The containers are encapsulated into a Kubernetes object known as pods.
- A pod is a single instance of an application.
- A pod is the smallest object that you can create in Kubernetes.
- pods usually have a one-to-one relationship with containers running your application.
- To scale up, you create new pods.
- And to scale down, you delete existing pod.
- But sometimes you might have a scenario where you have a helper container
    - In this case, Pod with Multi-containers are acceptable

- kubectl

    - Get the application image
    
    ```shell
    $ kubectl run nginx --image nginx
    ```

    - See the list of PODs
    
    ```shell
    $ kubectl get pods
    ```

    - Detail information of PODs
    
    ```shell
    $ kubectl describe pod [pod-name]
    ```

- [PODs with YAML](https://github.com/mg729/Cloud/blob/master/docs/Kubernetes/podsWithYaml.md)  

### Practice

- Create a new pod with the `nginx` image.
    ```shell
    $ kubectl run --help
    $ kubectl run nginx --image=nginx
    pod/nginx created
    ```

- Check pods details

    ```shell
    $ kubectl get pods
    NAME            READY   STATUS    RESTARTS   AGE
    nginx           1/1     Running   0          59s
    newpods-5r698   1/1     Running   0          8s
    newpods-bhpwt   1/1     Running   0          8s
    newpods-vnqvl   1/1     Running   0          8s

    $ kubectl describe pod newpods-5r698
    Name:             newpods-5r698
    Namespace:        default
    Priority:         0
    Service Account:  default
    Node:             controlplane/172.25.0.40
    Start Time:       Mon, 27 Mar 2023 21:29:43 +0000
    Labels:           tier=busybox
    Annotations:      <none>
    Status:           Running
    IP:               10.42.0.12
    IPs:
    IP:           10.42.0.12
    Controlled By:  ReplicaSet/newpods
    Containers:
    busybox:
        Container ID:  containerd://ce7015b9063f7eb67df7fdcdcdaf750b2bbe5ba53b9761cb400d5a35857c3a2a
        Image:         busybox
        Image ID:      docker.io/library/busybox@sha256:b5d6fe0712636ceb7430189de28819e195e8966372edfc2d9409d79402a0dc16
        Port:          <none>
        Host Port:     <none>
        Command:
        sleep
        1000
        State:          Running
        Started:      Mon, 27 Mar 2023 21:29:46 +0000
        Ready:          True
        Restart Count:  0
        Environment:    <none>
        Mounts:
        /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-jsp5k (ro)
    Conditions:
    Type              Status
    Initialized       True 
    Ready             True 
    ContainersReady   True 
    PodScheduled      True 
    Volumes:
    kube-api-access-jsp5k:
        Type:                    Projected (a volume that contains injected data from multiple sources)
        TokenExpirationSeconds:  3607
        ConfigMapName:           kube-root-ca.crt
        ConfigMapOptional:       <nil>
        DownwardAPI:             true
    QoS Class:                   BestEffort
    Node-Selectors:              <none>
    Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                                node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
    Events:
    Type    Reason     Age   From               Message
    ----    ------     ----  ----               -------
    Normal  Scheduled  81s   default-scheduler  Successfully assigned default/newpods-5r698 to controlplane
    Normal  Pulling    81s   kubelet            Pulling image "busybox"
    Normal  Pulled     79s   kubelet            Successfully pulled image "busybox" in 1.720228932s (1.720254725s including waiting)
    Normal  Created    79s   kubelet            Created container busybox
    Normal  Started    79s   kubelet            Started container busybox

    $ kubectl get pods -o wide
    NAME            READY   STATUS    RESTARTS   AGE     IP           NODE           NOMINATED NODE   READINESS GATES
    nginx           1/1     Running   0          3m48s   10.42.0.9    controlplane   <none>           <none>
    newpods-5r698   1/1     Running   0          2m57s   10.42.0.12   controlplane   <none>           <none>
    newpods-bhpwt   1/1     Running   0          2m57s   10.42.0.10   controlplane   <none>           <none>
    newpods-vnqvl   1/1     Running   0          2m57s   10.42.0.11   controlplane   <none>           <none>

    $ kubectl get pods
    NAME            READY   STATUS             RESTARTS   AGE
    nginx           1/1     Running            0          6m36s
    newpods-5r698   1/1     Running            0          5m45s
    newpods-bhpwt   1/1     Running            0          5m45s
    newpods-vnqvl   1/1     Running            0          5m45s
    webapp          1/2     ImagePullBackOff   0          55s  ## There are two containers

    $ kubectl delete pod webapp
    pod "webapp" deleted

    $ kubectl run redis --image=redis123 --dry-run=client -o yaml
    apiVersion: v1
    kind: Pod
    metadata:
    creationTimestamp: null
    labels:
        run: redis
    name: redis
    spec:
    containers:
    - image: redis123
        name: redis
        resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
    status: {}

    $ kubectl run redis --image=redis123 --dry-run=client -o yaml > redis.yaml

    $ cat redis.yaml 
    apiVersion: v1
    kind: Pod
    metadata:
    creationTimestamp: null
    labels:
        run: redis
    name: redis
    spec:
    containers:
    - image: redis123
        name: redis
        resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
    status: {}

    $  kubectl create -f redis.yaml
    pod/redis created
    $ vi redis.yaml # Update redis image name from redis123 to redis
    $ cat redis.yaml 
    apiVersion: v1
    kind: Pod
    metadata:
    creationTimestamp: null
    labels:
        run: redis
    name: redis
    spec:
    containers:
    - image: redis
        name: redis
        resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
    status: {}

    $ kubectl apply -f redis.yaml 
    Warning: resource pods/redis is missing the kubectl.kubernetes.io/last-applied-configuration annotation which is required by kubectl apply. kubectl apply should only be used on resources created declaratively by either kubectl create --save-config or kubectl apply. The missing annotation will be patched automatically.
    pod/redis configured

    $ kubectl get pods
    NAME            READY   STATUS    RESTARTS        AGE
    nginx           1/1     Running   0               20m
    newpods-5r698   1/1     Running   1 (2m49s ago)   19m
    newpods-bhpwt   1/1     Running   1 (2m49s ago)   19m
    newpods-vnqvl   1/1     Running   1 (2m49s ago)   19m
    redis           1/1     Running   0               2m18s
    ```

- What does the `READY` column in the output of the kubectl get pods command indicate?
    - Running Containers in POD / Total Conatiners in POD

- `--dry-run` option 
    - You can use the `--dry-run=client` flag to preview the object that would be sent to your cluster, without really submitting it.

- kubectl run & create Difference
    - kubectl create : create resource from a file
    - `$ kubectl create -f FILENAME`

    - kubectl run : Create and run a particular image in a pod
    - `$ kubectl run NAME --image=image [--env="key=value"] [--port=port] [--dry-run=server|client] [--overrides=inline-json] [--command] -- [COMMAND] [args...]`

- `apiVersion` - Which version of the Kubernetes API you're using to create this object
- `kind` - What kind of object you want to create
- `metadata` - Data that helps uniquely identify the object, including a name string, UID, and optional namespace
    - `label` - Labels are key/value pairs that are attached to objects, such as pods. Labels are intended to be used to specify identifying attributes of objects that are meaningful and relevant to users
    - `annotation` - You can use Kubernetes annotations to attach arbitrary non-identifying metadata to objects. Clients such as tools and libraries can retrieve this metadata. The metadata in an annotation can be small or large, structured or unstructured, and can include characters not permitted by labels.
- `spec` - What state you desire for the object

<!--
Q. Pod yaml create 할때, meta 에 기재되는 name 과 Container 에 기재되는 name 이 동일 해야 하나요?

A. Pod YAML 파일에서 metadata 필드에 기재되는 name과 spec.containers 필드에 기재되는 name은 서로 다른 값이 될 수 있습니다.

metadata.name은 Pod 리소스의 고유한 이름을 지정하는 것이며, spec.containers.name은 Pod에 포함된 컨테이너의 이름을 지정하는 것입니다. 
하나의 Pod에 여러 개의 컨테이너가 포함될 수 있으며, 각 컨테이너는 고유한 이름을 가져야 합니다.

예를 들어, 하나의 Pod에 nginx와 mysql 두 개의 컨테이너가 포함된다고 가정해보겠습니다. 
이 경우, Pod의 metadata.name은 "web-server"일 수 있고, nginx 컨테이너의 name은 "nginx"이며, mysql 컨테이너의 name은 "mysql"이 될 수 있습니다.

따라서, metadata.name과 spec.containers.name은 서로 다른 값을 가질 수 있으며, 이는 Pod의 이름과 컨테이너의 이름이 각각 다르게 지정될 수 있다는 것을 의미합니다.

-->

## ReplicaSets
<!--우왕 재밌다-->
> **High Availability**  
> *The Replication Controller helps us run multiple instances of a single pod in the Kubernetes cluster, thus providing high availability.*  


> **Load Balancing & Slicing**  
> *Another reason we need Replication Controller is to create multiple pods to share the load across them.*  

- Replication Controller is the older technology that is replaced by Replica Set
- Replica Set is the new recommended way to set up replication

#### ReplicaSet Definition file

```yaml
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata: # ReplicaSet
      name: myapp-replicaset
      labels:
        app: myapp
        type: front-end
    spec: # ReplicaSet
     template: #Under spec>template, we add pod information to replicate
        metadata: # POD
          name: myapp-pod
          labels:
            app: myapp
            type: front-end
        spec: # POD
         containers:
         - name: nginx-container
           image: nginx
     replicas: 3 # How many replicas are expected
     selector: # helps ReplicaSets identify what pods fall under it
       matchLabels: # !!! It should be matched with label in template !!!
        type: front-end # labels of the POD 
 ```
    
- To Create the replicaset
```console
$ kubectl create -f replicaset-definition.yaml
```
- To list all the replicaset
```console
$ kubectl get replicaset
$ kubectl get rs
$ k get rs
```
- To list pods that are launch by the replicaset
```console
$ kubectl get pods
```
- To get the details of the replicaSet
```console
$ kubectl describe replicaset [replicaset-Name]
```
- Delete replicasets
```console
$ kubectl delete replicaset myapp-replicaset # Also deletes all underlying PODs
```
- Edit replicaset configuration
```console
$ kubectl edit replicaset [Replicaset-Name]
$ kubectl edit rs [Replicaset-Name]
```
- Command for replicaset help
```console
$ kubectl explain replicaset
```
- Command for help functionality of each command
```console
$ kubectl scale -h
$ kubectl create deployment --help
```
- ReplicaSet requires a selector definition
    - It's because Replica Set can also manage pods that were not created as part of the Replica Set creation.
    - Say for example, there were pods created before the creation of the Replica Set that match labels specified in the selector, the Replica Set will also take those pods into consideration when creating the replicas.

- Scale
    - Update the replicas value and apply with the following command
    ```console
    $ kubectl apply -f replicaset-definition.yaml   
    ```

## Deployments

![Deployment](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/deployment.PNG)

```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: myapp-deployment
      labels:
        app: myapp
        type: front-end
    spec:
     template: # POD definition
        metadata:
          name: myapp-pod
          labels:
            app: myapp
            type: front-end
        spec:
         containers:
         - name: nginx-container
           image: nginx
     replicas: 3
     selector:
       matchLabels:
        type: front-end
 ```
- Once the file is ready, create the deployment using deployment definition file
  ```
  $ kubectl create -f deployment-definition.yaml
  ```
- To see the created deployment
  ```
  $ kubectl get deployment
  ```
- The deployment automatically creates a **`ReplicaSet`**. 
- The replicasets ultimately creates **`PODs`**. 
- To see the all objects at once
  ```
  $ kubectl get all
  ```


## Services
<!--curl 은 사용자 상호작용없이 작동하도록 설계된 서버에서 다른 서버로 데이터를 전송하기 위한 명령줄 유틸리티-->

- **curl**
    - Client URL (cURL, pronounced “curl”) is a command line tool that enables data exchange between a device and a server through a terminal.

- Service Type

### NodePort 

![ServiceDefinitionFile](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/srvnp.PNG)
```yaml
apiVersion: v1
kind: Service
metadata:
 name: myapp-service
spec:
 type: NodePort
 ports:
 - targetPort: 80
   port: 80
   nodePort: 30008
 selector: # POD labels information from the pod yaml
   app: myapp
   type: front-end
```
    - The service listens to a port on the node and forward request to the Pods.
    - In the above picture, a NordPort means 30008
    - Exposes the Service **on each Node**'s IP at a static port
    - One of its use case is to listen to a port on the node and forward request on that port to a port on the Pod running the web application.

 ![NodePort](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/srvnp4.PNG)
- When Pods are distributed across multiple nodes
- To summarize, in any case, whether it be a single Pod on a single node, multiple Pods on a single node,or multiple Pods on multiple nodes, the service is created exactly the same without you having to do any additional steps during the service creation.    
    
### ClusterIP 
![ClusterIP](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/srvc1.PNG)

- The pod's IP like 10.244.0.3 in the picture above is not static, it changes when pods are down and new pods are created

- The service creates a virtual IP **inside the cluster** to enable communication between different services, such as a set of frontend servers to a set of backend servers.

- A Kubernetes service can help us group the pods together and provide a single interface to access the pods in a group.
    - For example, a service created for the backend pods will help group all the backend pods together and provide a single interface for other pods to access this service.The requests are forwarded to **one of the pods under the service randomly.** Similarly, create additional services for Redis and allow the backend podsto access the Redis systems through the service.

### LoadBalancer
- It **provisions a load balancer** for our application in supported cloud providers.

## Namespaces

## Imperative vs Declarative

