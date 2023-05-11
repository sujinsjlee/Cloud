# Lightning Lab 2

## Quiz 1

We have deployed a few pods in this cluster in various namespaces. Inspect them and identify the pod which is not in a `Ready` state. Troubleshoot and fix the issue.  

Next, add a check to restart the container on the same pod if the command `ls /var/www/html/file_check`  fails. This check should start after a delay of `10 seconds` and run every `60 seconds`.   

```
k get pod --all-namespaces
k describe pod nginx1401 -n dev1401
```

```
k get pod nginx1401 -n dev1401 -o yaml > pod.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx
  name: nginx1401
  namespace: dev1401
spec:
  containers:
  - image: kodekloud/nginx
    imagePullPolicy: IfNotPresent
    name: nginx
    ports:
    - containerPort: 9080
      protocol: TCP
    readinessProbe:
      httpGet:
        path: /
        port: 9080  ## Update port number
    livenessProbe: ## Add livenessProbe
      exec:
        command:
        - ls
        - /var/www/html/file_check
      initialDelaySeconds: 10
      periodSeconds: 60
```

```
k replace -f pod.yaml
```

## Quiz 2
Create a cronjob called `dice` that runs every `one minute`. Use the Pod template located at `/root/throw-a-dice`. The image `throw-dice` randomly returns a value between 1 and 6. The result of 6 is considered `success` and all others are `failure`.    

The job should be `non-parallel` and complete the task `once`. Use a `backoffLimit` of `25`.    

- **A CronJob creates Jobs on a repeating schedule.**

- CronJob is meant for performing regular scheduled actions such as backups, report generation, and so on. One CronJob object is like one line of a crontab (cron table) file on a Unix system. It runs a job periodically on a given schedule, written in Cron format.
- https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/

```
vi cronjob.yaml
cat /root/throw-a-dice.yaml ## Check image
```

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: dice
spec:
  schedule: "*/1 * * * *" ## one minute
  jobTemplate:
    spec:
      completions: 1
      backoffLimit: 25 # This is so the job does not quit before it succeeds.
      activeDeadlineSeconds: 20
      template:
        spec:
          containers:
          - name: dice
            image: kodekloud/throw-dice ## Check under /root/throw-a-dice
          restartPolicy: Never
```

```
# ┌───────────── minute (0 - 59)
# │ ┌───────────── hour (0 - 23)
# │ │ ┌───────────── day of the month (1 - 31)
# │ │ │ ┌───────────── month (1 - 12)
# │ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday;
# │ │ │ │ │                                   7 is also Sunday on some systems)
# │ │ │ │ │                                   OR sun, mon, tue, wed, thu, fri, sat
# │ │ │ │ │
# * * * * *
```

- tep values can be used in conjunction with ranges. Following a range with `/<number>` specifies skips of the number's value through the range. 

- https://kubernetes.io/docs/concepts/workloads/controllers/job/
- **.sepc.parallelism**
    - 먼저 parallelism은 말 그대로 병렬로 실행하는 Pod의 수이다. 1 이상의 값으로 (Default는 1이다.) 값에 따라 Pod의 생성 갯수가 결정된다. 예를 들어 parallelism의 값이 3이면 Pod는 3개가 실행되어 동작하게 된다.
- **.spec.completions**
    - completion은 Job이 작업을 끝내야하는 Pod의 수이다. 즉 Job에 있는 모든 Pod의 실행이 끝났을 때, 실행 완료된 (생성된 Pod) Pod의 갯수이다.

```
k apply -f cronjob.yaml
```

## Quiz 3
Create a pod called `my-busybox` in the `dev2406` namespace using the `busybox` image. The container should be called `secret` and should sleep for `3600` seconds.  

The container should mount a `read-only` secret volume called `secret-volume` at the path `/etc/secret-volume`. The secret being mounted has already been created for you and is called `dotfile-secret`.  

Make sure that the pod is scheduled on `controlplane` and no other node in the cluster.  


```
k run my-busybox --image=busybox --dry-run=client -o yaml > pod.yaml
```


```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: my-busybox
  name: my-busybox
  namespace: dev2406 ## Add ns
spec:
  volumes:
  - name: secret-volume
    secret:
      secretName: dotfile-secret
  nodeSelector:
    kubernetes.io/hostname: controlplane
  tolerations: 
  - key: "node-role.kubernetes.io/control-plane"
    operator: "Exists"
    effect: "NoSchedule"
  containers:
  - command:
    - sleep
    args:
    - "3600"
    image: busybox
    name: secret
    volumeMounts:
    - name: secret-volume
      readOnly: true
      mountPath: "/etc/secret-volume"
```


- Another way is to add nodeName (not toleration)
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: my-busybox
  name: my-busybox
  namespace: dev2406 ## Add ns
spec:
  nodeName: controlplane
  volumes:
  - name: secret-volume
    secret:
      secretName: dotfile-secret
  containers:
  - command:
    - sleep
    args:
    - "3600"
    image: busybox
    name: secret
    volumeMounts:
    - name: secret-volume
      readOnly: true
      mountPath: "/etc/secret-volume"
```

```
k apply -f pod.yaml
```


## Quiz 4
Create a single ingress resource called `ingress-vh-routing`. The resource should route HTTP traffic to multiple hostnames as specified below:  

The service `video-service` should be accessible on `http://watch.ecom-store.com:30093/video`  

The service `apparels-service` should be accessible on `http://apparels.ecom-store.com:30093/wear`  

Here `30093` is the port used by the `Ingress Controller`    

- https://kubernetes.io/docs/concepts/services-networking/ingress/#the-ingress-resource

```
vi ingress.yaml
```

```yaml
kind: Ingress
apiVersion: networking.k8s.io/v1
metadata:
  name: ingress-vh-routing
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - host: watch.ecom-store.com
    http:
      paths:
      - pathType: Prefix
        path: "/video"
        backend:
          service:
            name: video-service
            port:
              number: 8080
  - host: apparels.ecom-store.com
    http:
      paths:
      - pathType: Prefix
        path: "/wear"
        backend:
          service:
            name: apparels-service
            port:
              number: 8080
```

## Quiz 5
A pod called `dev-pod-dind-878516` has been deployed in the default namespace. Inspect the logs for the container called `log-x` and redirect the warnings to `/opt/dind-878516_logs.txt` on the `controlplane` node

```
kubectl logs dev-pod-dind-878516 -c log-x | grep WARNING > /opt/dind-878516_logs.txt
```

- `-c [CONTAINER NAME]`
    - get logs for specific container name
