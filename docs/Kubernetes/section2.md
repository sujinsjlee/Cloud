# Section 2

## Cluster Architecture
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
