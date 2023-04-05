# Section3 - Scheduling

- [Manual scheduling](#Manual-scheduling)  
- [Labels,Selectors and Annotations](#Labels,-Selectors-and-Annotations)  
- [Taints and Tolerations](#Taints-and-Tolerations)  
- [Node Selectors](#Node-Selectors)  
- [Node Affinity](#Node-Affinity)  
- [Resource Requirements and Limits](#Resource-Requirements-and-Limits)  
- [DaemonSets](#DaemonSets)  
- [Static Pods](#Static-Pods)  
- [Multiple Schedulers](#Multiple-Schedulers)  
- [Configuring Scheduler Profiles](#Configuring-Scheduler-Profiles)  

## Manual scheduling

- What do you do when you do not have a scheduler in your cluster?
  - Kubernetes adds the `nodeName` automatically.
  - Once identified it schedules the POD on the node by setting the nodeName property to the name of the node by creating a binding object.

```yaml
apiVersion: v1
kind: Pod
metadata:
 name: nginx
 labels:
  name: nginx
spec:
 containers:
 - name: nginx
   image: nginx
   ports:
   - containerPort: 8080
 nodeName: node02
```

### Practice

```console
~# k get nodes
~# kubectl get pods -o wide 
```

- To check why is the POD in a pending state? check scheduler 
```console
~# kubectl describe pod [POD NAME]
~# kubectl get pods --namespace kube-system
```

- Manually schedule the pod on node01.
    - delete and recereate the pod, as the only property that may be edited on a running container is `image`

    ```
    vi nginx.yaml
    ```

    - Make the following edit

    ```yaml
    ---
    apiVersion: v1
    kind: Pod
    metadata:
      name: nginx
    spec:
      nodeName: node01    # add this line
      containers:
      -  image: nginx
         name: nginx
    ```

    ```
    kubectl delete -f nginx.yaml
    kubectl create -f nginx.yaml
    kubectl replace --force -f nginx.yaml
    ```

## Labels, Selectors and Annotations
- **Labels** are properties attached to each item
- **Selectors** help you to filter these items

![Labels and Selector](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/lpod.PNG)

- Once the pod is created with App1 labels, select the pod with labels
```
$ kubectl get pods --selector app=App1
```

- **Annotations**
    - While labels and selectors are used to group objects, annotations are used to record other details for informative purpose.

    ```yaml
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
    name: simple-webapp
    labels:
        app: App1
        function: Front-end
    annotations:
        buildversion: 1.34
    spec:
    replicas: 3
    selector:
    matchLabels:
        app: App1
    template:
    metadata:
        labels:
        app: App1
        function: Front-end
    spec:
        containers:
        - name: simple-webapp
        image: simple-webapp  
      ```
### Practice
- How many PODs exist in the `dev` environment?

```console
$ kubectl get pods --selector env=dev
$ kubectl get pods --selector env=dev  --no-headers | wc -l
```
- How many objects are in the prod environment including PODs, ReplicaSets and any other objects?

```console
$ kubectl get all --selector env=prod
```

- Identify the POD which is part of the prod environment, the finance BU and of frontend tier?
    - We can combine label expressions with `comma`. Only items with all the given label/value pairs will be returned, i.e. it is an and condition.

```console
$ kubectl get all --selector env=prod,bu=finance,tier=frontend
```

## Taints and Tolerations
![Taints and Toleration](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/tandt.PNG)

- **Taints are set on nodes**
- **Tolerations are set on pods**
- Only pods which are tolerant to the particular taint on a node will get scheduled on that node.

## Taints
- Use **`kubectl taint nodes`** command to taint a node.

  Syntax
  ```
  $ kubectl taint nodes <node-name> key=value:taint-effect
  ```
 
  Example
  ```
  $ kubectl taint nodes node1 app=blue:NoSchedule
  ```
  
- The taint effect defines what would happen to the pods if they do not tolerate the taint.
- There are 3 taint effects
  - **`NoSchedule`** : no schedule, which means the pods will not be scheduled on the node
  - **`PreferNoSchedule`** : the system will try to avoid placing a pod on the node, but that is not guaranteed.
  - **`NoExecute`** :  new pods will not be scheduled on the node and existing pods on the node, if any, will be evicted if they do not tolerate the taint.
  
- Tolerations are added to pods by adding a **`tolerations`** section in pod definition.
- all of these values need to be encoded in double codes.
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: myapp-pod
  spec:
    containers:
    - name: nginx-container
      image: nginx

    tolerations:
    - key: "app"
      operator: "Equal"
      value: "blue"
      effect: "NoSchedule"
  ```

- When the Kubernetes cluster is first set up, a taint is set on the **master node** automatically that prevents any pods from being scheduled on this node.

### Practice

- In yaml file, the default value for operator in toletation filed is `Equal`.

- When POD's status is temporarily `Running` status, check pods status with `--watch` option

```console
~# kubectl get pods --watch
```
<details>
<summary> Taints and Tolerations </summary>

```console
controlplane ~ ➜  k get nodes
NAME           STATUS   ROLES           AGE   VERSION
controlplane   Ready    control-plane   19m   v1.26.0
node01         Ready    <none>          18m   v1.26.0

controlplane ~ ➜  k describe node node01
Name:               node01
Roles:              <none>
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/arch=amd64
                    kubernetes.io/hostname=node01
                    kubernetes.io/os=linux
...
```

- Create a taint on node01 with key of spray, value of mortein and effect of NoSchedule 

```console
controlplane ~ ➜  kubectl taint nodes node01 spray=mortein:NoSchedule
node/node01 tainted
```

- Create a new pod
```console
controlplane ~ ➜  kubectl run --help
Create and run a particular image in a pod.

Examples:
  # Start a nginx pod
  kubectl run nginx --image=nginx
```

- When creating YAML file with image, `--image nginx` there should be a space between them, not equal letter.

```console
controlplane ~ # kubectl run bee --image nginx --dry-run=client -o yaml > bee.yaml

controlplane ~ # cat bee.yaml 
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: bee
  name: bee
spec:
  containers:
  - image: nginx
    name: bee
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}

```
</details>


<details>
<summary>Remove the taint on controlplane, which currently has the taint effect of NoSchedule.</summary>

- Add **-** at the end of the command


```console
~# kubectl taint nodes controlplane node-role.kubernetes.io/control-plane:NoSchedule-
```
</details>

## Node Selectors 
- We add new property called Node Selector to the spec section and specify the label.
- The scheduler uses these labels to match and identify the right node to place the pods on

![NS](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/nsel.PNG)


- To label nodes

  - Syntax
  ```console
  $ kubectl label nodes <node-name> <label-key>=<label-value>
  ```

  - Example
  ```console
  $ kubectl label nodes node-1 size=Large
  ```

## Node Affinity

- With `Node Selectors` we cannot provide the advance options like allowing multiple size or excluding specific size.

```yaml
apiVersion: v1
kind: Pod
metadata:
 name: myapp-pod
spec:
 containers:
 - name: data-processor
   image: data-processor
 affinity:
   nodeAffinity:
     requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: In
            values: 
            - Large
            - Medium
```

- The `In` operator ensures that the pod will be placed on a node whose label size has any value in the list of values specified here. In this case, it is just one called `Large` and `Medium`.
```yaml
apiVersion: v1
kind: Pod
metadata:
 name: myapp-pod
spec:
 containers:
 - name: data-processor
   image: data-processor
 affinity:
   nodeAffinity:
     requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: NotIn
            values: 
            - Small
```

- Use the node `In` operator to say something like, size `NotIn` small, where node affinity will match the nodes with a size not set to small.

```yaml
apiVersion: v1
kind: Pod
metadata:
 name: myapp-pod
spec:
 containers:
 - name: data-processor
   image: data-processor
 affinity:
   nodeAffinity:
     requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: Exists
```

- The `Exists` operator will simply check if the label size exists on the nodes, and there is no need to add the value section for that as it does not compare the values.

### Node Affinity Types
![11](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/nats.PNG)  
![22](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/nats1.PNG)

### Practice
<details>
    <summary>How many Labels exist on node node01?</summary>
    ```
    kubectl describe node node01
    ```
    Look under `Labels` section
    --- OR ---
    ```
    kubectl get node node01 --show-labels
    ```
</details>

<details>
    <summary>Apply a label color=blue to node node01</summary>
    ```
    kubectl label node node01 color=blue
    ```
</details>

<details>
    <summary>Create a new deployment named blue with the nginx image and 3 replicas.</summary>
    ```
    kubectl create deployment blue --image=nginx --replicas=3
    ```
</details>

<details>
    <summary>Which nodes can the pods for the blue deployment be placed on?</summary>

    Check if master and node01 have any taints on them that will prevent the pods to be scheduled on them. If there are no taints, the pods can be scheduled on either node.
    ```
    kubectl describe nodes controlplane | grep -i taints
    kubectl describe nodes node01 | grep -i taints
    ```
</details>

<details>
    <summary>Set Node Affinity to the deployment to place the pods on node01 only.</summary>
    Now we edit in place the deployment we created earlier. Remember that we labelled `node01` with `color=blue`? Now we are going to create an affinity to that label, which will "attract" the pods of the deployment to it.
    ```
    $ kubectl edit deployment blue
    ```
</details>

- **Indentation in VIM**
  - Type `v` (to enter visual block editing mode)
  - Move the cursor with the arrow keys (or with h/j/k/l) to highlight the lines you want to indent or unindent.
  - Then:
    - Type `>` to indent once (2 spaces).
    - Type `2>` to indent twice (4 spaces).
    - Type `3>` to indent thrice (6 spaces).

## Taints and Tolerations and Node Affinity

![TTNA](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/tn-na.PNG)

- Taints and Tolerations do not guarantee that the pods will only prefer these nodes
- The red pods may end up on one of the other nodes that do not have a taint or toleration set.
- At the same time Node Affinity do not guarantee that other pods are not placed on these nodes.

![combination](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/tn-nsa.png)

- There is a chance that one of the other pods may end up on our nodes.
- As such, **a combination of taints and tolerations and node affinity rules can be used together to completely dedicate nodes for specific parts**.

## Resource Requirements and Limits
> If you know that your application will need more CPU, Memory, Disk capacity, modify the values by specifying them in POD definition or deployment yaml file

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp-color
  labels:
    name: simple-webapp-color
spec:
 containers:
 - name: simple-webapp-color
   image: simple-webapp-color
   ports:
    - containerPort:  8080
   resources: # Specify new value for memory and CPU usage
     requests:
      memory: "1Gi" # 1 Gigabyte memory
      cpu: "1"
```

- There is a difference between G and Gi

  |||bytes|
  |---|---|---|   
  |1G|Gigabyte|1,000,000,000 |   
  |1M|Megabyte|1,000,000 |  
  |1K|Kilobyte|1,000 |  
  |1Gi|Gigabyte|1,073,741,824 |    
  |1Mi|Megabyte|1,048,576 |  
  |1Ki|Kilobyte|1,024 |  

### Request & Limit
<!--Request & Limit
컨테이너에 적용될 리소스의 양을 정의하는데 쿠버네티스에서는 request와 limit이라는 컨셉을 사용한다.

request는 컨테이너가 생성될때 요청하는 리소스 양이고, limit은 컨테이너가 생성된 후에 실행되다가 리소스가 더 필요한 경우 (CPU가 메모리가 더 필요한 경우) 추가로 더 사용할 수 있는 부분이다.-->

- **Request**
  - Requests are the minimum guaranteed amount of a resource that is reserved for a container.
- **Limit**
  - Kubernetes defines Limits as the maximum amount of a resource to be used by a container. This means that the container can never consume more than the memory amount or CPU amount indicated. 

 
- The limits and requests are **set for each container** within the pod.

> So what happens when a pod tries to exceed resources beyond its specified limit?

- In case of **CPU, Kubernetes throttles the CPU** so that it does not go beyond the specified limit.
- A container cannot use more CPU resources than its limit.


## DaemonSets
> DaemonSets are like replicasets, as it helps in to deploy multiple instances of pod. But it runs one copy of your pod on each node in your cluster.


![demonset](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/ds-uc-kp.PNG)

```yaml
apiVersion: apps/v1
kind: DaemonSet ## kind is set as DaemonSet
metadata:
  name: monitoring-daemon
  labels:
    app: nginx
spec:
  selector:
    matchLabels:
      app: monitoring-agent
  template:
    metadata:
     labels:
       app: monitoring-agent
    spec:
      containers:
      - name: monitoring-agent
        image: monitoring-agent
```

### Practice

```console
~# k get daemonsets -A 
~# k get ds -A
~# k describe ds kube-flannel-ds -n kube-system
```

## Static Pods
- **Created by the kubelet**
- **Deploy Control Plane components as Static Pods**

#### How do you provide a pod definition file to the kubelet without a kube-apiserver?
- You can configure the kubelet to read the pod definition files from a directory on the server designated to store information about pods.

### Configure Static Pod
- The designated directory can be any directory on the host and the location of that directory is passed in to the kubelet as an option while running the service.
  - The option is named as **`--pod-manifest-path`**.
  
  ![sp](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/sp.PNG)
  
### Another way to configure static pod 
- Instead of specifying the option directly in the **`kubelet.service`** file, you could provide a path to another config file using the config option, and define the directory path as staticPodPath in the file.

  ![sp1](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/sp1.PNG)

### View the static pods
- To view the static pods
  ```
  $ docker ps
  ```
- Since we don't have kube-api, we cannot use `kubectl` command


## Multiple Schedulers
> Your kubernetes cluster can schedule multiple schedulers at the same time.

![ms](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/dask.PNG)

- View scheduler pods
```yaml
$ kubectl get pods -n kube-system
```

## Use the Custom Scheduler
![sch](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/cs.png)

- Create a pod definition file and add new section called **`schedulerName`** and specify the name of the new scheduler
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: nginx
  spec:
    containers:
    - image: nginx
      name: nginx
    schedulerName: my-custom-scheduler ## Add customized schedulerName
  ```

### Practice

```yaml
~# kubectl descrive pod [POD NAME] -n kube-systme | grep Image
``` 

## Configuring Scheduler Profiles

- [Scheduling framework](https://kubernetes.io/docs/concepts/scheduling-eviction/scheduling-framework/)
