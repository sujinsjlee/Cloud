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

## Node Selectors