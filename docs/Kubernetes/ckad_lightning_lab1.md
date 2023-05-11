# Lightning Lab

## Quiz 1
Create a Persistent Volume called `log-volume`. It should make use of a `storage class` name `manual`. It should use `RWX` as the access mode and have a size of `1Gi`. The volume should use the hostPath `/opt/volume/nginx`  

Next, create a PVC called `log-claim` requesting a minimum of `200Mi` of storage. This PVC should bind to `log-volume`.  

Mount this in a pod called `logger` at the location `/var/www/nginx.` This pod should use the image `nginx:alpine`.  

- [PV](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistent-volumes)  

```
vi log-volume.yaml
```

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: log-volume
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany
  storageClassName: manual
  hostPath:
      path: /opt/volume/nginx
```

```
k apply -f log-volume.yaml
```

```
vi pvc.yaml
```

- [PVC](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims)

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: log-claim
spec:
  accessModes:
    - ReadWriteMany ## Should match with the PV
  resources:
    requests:
      storage: 200Mi
  storageClassName: manual
```

```
k apply -f pvc.yaml
k run logger --image=nginx:alpine --dry-run=client -o yaml > pod.yaml
vi pod.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: logger
spec:
  containers:
    - name: logger
      image: nginx:alpine
      volumeMounts:
      - mountPath: /var/www/nginx
        name: mypd
  volumes:
    - name: mypd
      persistentVolumeClaim:
        claimName: log-claim
```

## Quiz 2

We have deployed a new pod called `secure-pod` and a service called `secure-service`. Incoming or Outgoing connections to this pod are not working. Troubleshoot why this is happening.  

Make sure that incoming connection from the pod `webapp-color` are successful.  

Important: Don't delete any current objects deployed.  

```
k describe netpol # Error msg related ingress will show up
k get pod --show-labels # secure-pod's label is run=secure-pod

k get netpol

k get netpol [Network Policy Name] -o yaml > netpol.yaml
```

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  creationTimestamp: "2023-05-11T02:24:10Z"
  generation: 1
  name: default-deny
  namespace: default
  resourceVersion: "5263"
  uid: e3d65e42-3b46-4ff1-9486-3f60c59e78f9
spec:
  podSelector: 
    matchLabels:
      run=secure-pod
  policyTypes:
  - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              name: webapp-color
      ports:
        - protocol: TCP
          port: 80
```


## Quiz 3

Create a pod called `time-check` in the `dvl1987` namespace. This pod should run a container called `time-check` that uses the `busybox` image.  
Create a config map called `time-config` with the data `TIME_FREQ=10` in the same namespace.  
The `time-check` container should run the command: `while true; do date; sleep $TIME_FREQ;done` and write the result to the location `/opt/time/time-check.log`.  
The path `/opt/time` on the pod should mount a volume that lasts the lifetime of this pod.  

```
k create ns dvl1987
k create cm time-config -n dvl1987 --from-literal=TIME_FREQ=10 # cm is configmap

k get cm -n dvl1987

k run time-check --image=busybox --dry-run=client -o yaml > pod.yaml
```

- [Define container environment variables using ConfigMap data](hhttps://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/#define-container-environment-variables-using-configmap-data)  

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: time-check
  name: time-check
  namespace: dvl1987
spec:
  volumes:
  - name: log-volume
    emptyDir: {}
  containers:
  - image: busybox
    name: time-check
    env:
    - name: TIME_FREQ
      valueFrom:
            configMapKeyRef:
              name: time-config
              key: TIME_FREQ
    volumeMounts:
    - mountPath: /opt/time
      name: log-volume
    command:  [ "/bin/sh", "-c", "while true; do date; sleep $TIME_FREQ;done > /opt/time/time-check.log" ]
```

```
k delete pod time-check
k apply -f pod.yaml
```

## Quiz 4
Create a new deployment called `nginx-deploy`, with one single container called `nginx`, image `nginx:1.16` and `4` replicas.
The deployment should use `RollingUpdate` strategy with `maxSurge=1`, and `maxUnavailable=2`.  

Next upgrade the deployment to version `1.17`.  


```
k create deploy nginx-deploy --image=nginx:1.16 --replicas=4 --dry-run=client > deploy.yaml
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx-deploy
  name: nginx-deploy
  namespace: default
spec:
  replicas: 4
  selector:
    matchLabels:
      app: nginx-deploy
  strategy:
    rollingUpdate: ## Add rollingUpdate strategy
      maxSurge: 1
      maxUnavailable: 2
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: nginx-deploy
    spec:
      containers:
      - image: nginx:1.16 ## Yon can update version to 17 here
        imagePullPolicy: IfNotPresent
        name: nginx
```

```
k apply -f deploy.yaml
kubectl set image deployment nginx-deploy nginx=nginx:1.17

kubectl rollout undo deployment nginx-deploy
```
- Run the kubectl rollout command to undo the update and go back to the previous version 


## Quiz 5

Create a `redis` deployment with the following parameters:  

Name of the deployment should be `redis` using the `redis:alpine` image. It should have exactly `1` replica.  

The container should request for `.2` CPU. It should use the label `app=redis`.  

It should mount exactly 2 volumes.  

a. An Empty directory volume called `data` at path `/redis-master-data`.  

b. A configmap volume called `redis-config` at path `/redis-master`.  

c. The container should expose the port `6379`.  

```
k create deploy redis --image=redis:alpine --replicas=1 --dry-run=client -o yaml > deploy.yaml
vi deploy.yaml
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: redis
  name: redis
spec:
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      volumes:
      - name: data
        emptyDir: {}
      - name: redis-config
        configMap:
          name: redis-config
      containers:
      - image: redis:alpine
        name: redis
        volumeMounts:
        - mountPath: /redis-master-data
          name: data
        - mountPath: /redis-master
          name: redis-config
        ports:
        - containerPort: 6379
        resources:
          requests:
            cpu: "0.2"
```
