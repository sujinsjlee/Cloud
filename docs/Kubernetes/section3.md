# Section3 - Scheduling

- [Manual scheduling](#Manual-scheduling)  
- [Labels,Selectors and Annotations](#Labels,-Selectors-and-Annotations)  
- [Taints and Tolerations](#Taints-and-Tolerations)  
- [Node Selectors](#Node-Selectors)  


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
controlplane ~ ➜  kubectl run bee --image=nginx --dry-run=client o yaml > bee.yaml

controlplane ~ ➜  cat bee.yaml 
pod/bee created (dry run)

controlplane ~ ➜  ?
-bash: ?: command not found

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

    ```
    kubectl taint nodes controlplane node-role.kubernetes.io/control-plane:NoSchedule-
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

  