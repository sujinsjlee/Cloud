# MOCK EXAM1

### Quiz 1
> Deploy a pod named `nginx-448839` using the `nginx:alpine` image.

```
k run nginx-448839 --image=nginx:alpine
```

### Quiz 2
> Create a namespace named `apx-z993845`

```
k create ns apx-z993845
```

### Quiz 3
> Create a new Deployment named `httpd-frontend` with 3 replicas using image `httpd:2.4-alpine`

- Name: httpd-frontend
- Replicas: 3
- Image: httpd:2.4-alpine


```
k create deploy httpd-frontend --replicas=3 --image=httpd:2.4-alpine
```

### Quiz 4
> Deploy a `messaging` pod using the `redis:alpine` image with the labels set to `tier=msg`.

```
k run messaging --image=redis:alpine --labels=tier=msg
```

## Quiz 5
> A replicaset `rs-d33393` is created. However the pods are not coming up. Identify and fix the issue.

- Once fixed, ensure the ReplicaSet has 4 `Ready` replicas.

```
k get rs
k describe rs rs-d33393
k edit rs rs-d33393

# Change image from busyboxXXX to busybox

# Delete POD with wrong image
# Check label of the pod
k delete pod -l name=busybox-pod
k get rs
```

## Quiz 6
> Create a service `messaging-service` to expose the `redis` deployment in the `marketing` namespace within the cluster on port `6379`.

```
k expose deploy redis --port=6379 --name=messaging-service --namespace=marketing
```

### Quiz 7
> Update the environment variable on the pod `webapp-color` to use a `green` background.

```
k get pod webapp-color -o yaml > pod.yaml
# Change env : [APP_COLOR, pink] to green

vi pod.yaml

k replace --force -f pod.yaml
```

## Quiz 8
> Create a new ConfigMap named `cm-3392845`. Use the spec given on the below.
- ConfigName Name: cm-3392845
- Data: DB_NAME=SQL3322
- Data: DB_HOST=sql322.mycompany.com
- Data: DB_PORT=3306

```
k create configmap cm-3392845 --from-literal=DB_NAME=SQL3322 --from-literal=DB_HOST=sql322.mycompany.com --from-literal=DB_PORT=3306
```

## Quiz 9
> Create a new Secret named `db-secret-xxdf` with the data given (on the below).

- Secret Name: db-secret-xxdf
- Secret 1: DB_Host=sql01
- Secret 2: DB_User=root
- Secret 3: DB_Password=password123

```
k create secret generic -h
k create secret generic db-secret-xxdf --from-literal=DB_Host=sql01 --from-literal=DB_User=root --from-literal=DB_Password=password123
```

## Quiz 10
> Update pod `app-sec-kff3345` to run as Root user and with the `SYS_TIME` capability.

- Pod Name: app-sec-kff3345
- Image Name: ubuntu
- SecurityContext: Capability SYS_TIME

- [SYS_TIME](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)

```
k get pod app-sec-kff3345 -o yaml > pod.yaml
vi pod.yaml

# Add securityContext
```

```yaml
    securityContext:
      capabilities:
        add: ["SYS_TIME"]
```

```
k replace --force -f pod.yaml 
```

## Quiz 11
> Export the logs of the `e-com-1123 pod` to the file `/opt/outputs/e-com-1123.logs`

- **It is in a different namespace. Identify the namespace first.**

```
k get ns

k logs e-com-1123 --namespce=e-commerce > /opt/outputs/e-com-1123.logs
```

## Quiz 12
> Create a `Persistent Volume` with the given specification.

- Volume Name: pv-analytics
- Storage: 100Mi
- Access modes: ReadWriteMany
- Host Path: /pv/data-analytics

- https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistent-volumes

```
vi pv.yaml
```

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-analytics
spec:
  capacity:
    storage: 5Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteMany
  hostPath:
    path: /pv/data-analytics
```

```
k apply -f pv.yaml
```

## Quiz 13
> Create a `redis` deployment using the image `redis:alpine` with `1 replica` and label `app=redis`.   
> Expose it via a ClusterIP service called `redis` on `port 6379`. Create a new `Ingress Type` NetworkPolicy called `redis-access` which allows only the pods with label `access=redis` to access the deployment.

- To create deployment:
```
kubectl create deployment redis --image=redis:alpine --replicas=1
```
- To expose the deployment using ClusterIP:
  - **NEED TO CONFIGURE --target-port OPTION** 
```
kubectl expose deployment redis --name=redis --port=6379 --target-port=6379
```
- To create ingress rule:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: redis-access
  namespace: default
spec:
  podSelector:
    matchLabels:
       app: redis
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          access: redis
    ports:
     - protocol: TCP
       port: 6379
```


### Quiz 14
> Create a Pod called `sega` with two containers:

- Container 1: Name `tails` with image `busybox` and command: `sleep 3600`.
- Container 2: Name `sonic` with image `nginx` and Environment variable: `NGINX_PORT` with the value `8080`.

- [env](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: sega
spec:
  containers:
  - image: busybox
    name: tails
    command: ["sleep", "3600"]
  - image: nginx
    name: sonic
    env:
    - name: NGINX_PORT
      value: "8080"
```