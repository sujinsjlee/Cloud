# MOCK EXAM2

## Quiz 1
Create a deployment called `my-webapp` with image: `nginx`, label `tier:frontend` and `2` replicas. Expose the deployment as a NodePort service with name `front-end-service` , port: `80` and NodePort: `30083`

- [type: NodePort](https://kubernetes.io/docs/concepts/services-networking/service/)

```
k create deploy my-webapp --image=nginx --replicas=2 
k expose deploy my-webapp --name front-end-service --port=80 --type=NodePort --dry-run=client -o yaml > service.yaml
vi service.yaml
```

```yaml
ports:
- nodePort:30083 ## ADD nodePort
```

```
k apply -f service.yaml
```

## Quiz 2
Add a taint to the node `node01` of the cluster. Use the specification below:  

key: `app_type`, value: `alpha` and effect: `NoSchedule`  

Create a pod called `alpha`, image: `redis` with toleration to `node01`.  

```
k taint node -h
kubectl taint node node01 app_type=alpha:NoSchedule
kubectl describe node node01 | grep -i app_type
```

- Now, create a pod manifest file called alpha
```
kubectl run alpha --image=redis --dry-run=client -o yaml > alpha.yaml
```


- and add rules under the spec.tolerations field as follows:
    - https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/#concepts

```yaml
tolerations:
  - effect: NoSchedule
    key: app_type
    value: alpha
```

- So final manifest file to create a pod called alpha with tolerations rules as follows:

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: alpha
  name: alpha
spec:
  tolerations:
  - effect: NoSchedule
    key: app_type
    value: alpha
  containers:
  - image: redis
    name: alpha
  dnsPolicy: ClusterFirst
  restartPolicy: Always
```
- To check the pod has been successfully deployed on the node node01, using -o wide flag:
```
kubectl get pods -o wide
```


## Quiz 3
Apply a label `app_type=beta` to node `controlplane`. Create a new deployment called `beta-apps` with image: `nginx` and replicas: `3`. Set Node Affinity to the deployment to place the PODs on `controlplane` only.

- NodeAffinity: requiredDuringSchedulingIgnoredDuringExecution

```
k label -h
kubectl label node controlplane app_type=beta
k get node controlplane --show-labels

kubectl create deploy beta-apps --image=nginx --replicas=3 --dry-run=client -oyaml > beta.yaml
```

- [NodeAffinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: beta-apps
  name: beta-apps
spec:
  replicas: 3
  selector:
    matchLabels:
      app: beta-apps
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: beta-apps
    spec:
      affinity:
        nodeAffinity:
         requiredDuringSchedulingIgnoredDuringExecution:
           nodeSelectorTerms:
           - matchExpressions:
             - key: app_type
               values: ["beta"]
               operator: In
      containers:
      - image: nginx
        name: nginx
```

```
k apply -f beta.yaml
```

## Quiz 4
Create a new Ingress Resource for the service: `my-video-service` to be made available at the URL: `http://ckad-mock-exam-solution.com:30093/video`.

- Create an ingress resource with `host: ckad-mock-exam-solution.com`
`path: /video`. Once set up, curl test of the URL from the nodes should be successful / `HTTP 200`

```
kubectl create ingress ingress --rule="ckad-mock-exam-solution.com/video*=my-video-service:8080"

curl http://ckad-mock-exam-solution.com:30093/video
```

## Quiz 5
We have deployed a new pod called `pod-with-rprobe`. This Pod has an initial delay before it is Ready. Update the newly created pod `pod-with-rprobe` with a `readinessProbe` using the given spec

- httpGet path: /ready
- httpGet port: 8080

- https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/

```
kubectl get po pod-with-rprobe -o yaml > pod.yaml
```

```yaml
readinessProbe:
  httpGet:
    path: /ready
    port: 8080
```

```
k replace --force -f pod.yaml
```


## Quiz 6
Create a new pod called `nginx1401` in the `default` namespace with the image `nginx`. Add a livenessProbe to the container to restart it if the command `ls /var/www/html/probe` fails. This check should start after a delay of `10 seconds` and run `every 60 seconds`.  

You may delete and recreate the object. Ignore the warnings from the probe.  

- https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-a-liveness-command

```
k run nginx1401 --image=nginx --dry-run=client -o yaml > pod.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx1401
  namespace: default
spec:
  containers:
    - name: nginx1401
      image: nginx
      livenessProbe:
        exec:
          command: ["ls /var/www/html/probe"]
        initialDelaySeconds: 10
        periodSeconds: 60
```



## Quiz 7
Create a job called `whalesay` with image `docker/whalesay` and command `"cowsay I am going to ace CKAD!"`.

- completions: 10
- backoffLimit: 6
- restartPolicy: Never

- https://kubernetes.io/docs/concepts/workloads/controllers/job/


```
k create job -h
k create job --image=docker/whalesay --dry-run=client -o yaml > job.yaml
```

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: whalesay
spec:
  completions: 10
  backoffLimit: 6
  template:
    metadata:
      creationTimestamp: null
    spec:
      containers:
      - command:
        - sh 
        - -c
        - "cowsay I am going to ace CKAD!"
        image: docker/whalesay
        name: whalesay
      restartPolicy: Never
```

```
k apply -f job.yaml
watch k get jobs
```

## Quiz 8
Create a pod called `multi-pod` with two containers.  

Container 1:  
name: `jupiter`, image: `nginx`  

Container 2:  
name: `europa`, image: `busybox`  
command: `sleep 4800`  

Environment Variables:  

Container 1:  

`type: planet`  

Container 2:  

`type: moon`  

```
k run multi-pod --image=nginx --dry-run=client -o yaml --command -- sleep 4800 > multi.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: multi-pod
  name: multi-pod
spec:
  containers:
  - image: nginx
    name: jupiter
    env:
    - name: type
      value: planet
  - image: busybox
    name: europa
    command: ["/bin/sh","-c","sleep 4800"]
    env:
     - name: type
       value: moon
```

## Quiz 9
Create a PersistentVolume called `custom-volume` with size: `50MiB` reclaim policy:`retain`, Access Modes: `ReadWriteMany` and hostPath: `/opt/data`

- **Reclaim Policy** : the word SHOULD BE started as UPPER CASE
    - **R**etain
    - **R**ecycle
    - **D**elete

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: custom-volume
spec:
  accessModes: ["ReadWriteMany"]
  capacity:
    storage: 50Mi
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /opt/data
```