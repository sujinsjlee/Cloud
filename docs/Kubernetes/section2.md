# Section 2 - Core Concepts

- [Cluster Architecture](#Cluster-Architecture)  
- [ETCD](#ETCD)  
- [Kubelet](#Kubelet)  
- [Kube Proxy](#Kube-Proxy)  
- [POD](#POD)  

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

## Kubelet

- Register Node
    - The kubelet in the Kubernetes worker node **registers the node** with a Kubernetes cluster.

- Create PODs
    - **When it receives instructions to load a container or a pod** on the node  
    - **it requests** the container runtime engine, which may be **Docker, to pull the required image and run an instance.**

- Monitor Node & PODs
    -  monitor the state of the pod and containers in it and reports to the kube API server on a timely basis.

- View kubelet options

    ```console
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
    
    ```console
    $ kubectl run nginx --image nginx
    ```

    - See the list of PODs
    
    ```console
    $ kubectl get pods
    ```

    - Detail information of PODs
    
    ```console
    $ kubectl describe pod [pod-name]
    ```

- [PODs with YAML](https://github.com/mg729/Cloud/blob/master/docs/Kubernetes/podsWithYaml.md)  

### Practice

- Create a new pod with the `nginx` image.
    ```console
    $ kubectl run --help
    $ kubectl run nginx --image=nginx
    pod/nginx created
    ```

- Check pods details

    ```console
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

- `--dry-run=client` option 
    - You can use the `--dry-run=client` flag to preview the object that would be sent to your cluster, without really submitting it.


- kubectl create
    - Create resource from a file
    - `$ kubectl create -f FILENAME`

- kubectl run
    - Create and run a particular image in a pod
    - `$ kubectl run NAME --image=image [--env="key=value"] [--port=port] [--dry-run=server|client] [--overrides=inline-json] [--command] -- [COMMAND] [args...]`
