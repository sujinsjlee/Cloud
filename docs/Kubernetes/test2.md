# CKAD

## CKAD Exercises
- https://github.com/dgkanatsios/CKAD-exercises
<!--
- LivenessProve 실패한 파드찾으라는 문제가 실제로 출제
- canary deployment문제가 실제로 출제
    - 기존 deploy는 되어있었고 서비스로 세팅을 다르게 하는 것
- network policy로 pod끼리 연결시키는 것
- 기본적인 deploy pod 만드는거
- cronjob
- docker image만들고 tar 파일로 변경해서 지정된 파일 위치에 넣는 문제
- ingress resources
-->

### Pod Design
> Jobs

> **Cronjobs**

### Observability
> **Liveness**  
> Lots of pods are running in qa,alan,test,production namespaces. All of these pods are configured with liveness probe. Please list all pods whose liveness probe are failed in the format of `<namespace>/<pod name>` per line.


[field selector](https://kubernetes.io/docs/concepts/overview/working-with-objects/field-selectors/)
[json path](https://kubernetes.io/docs/reference/kubectl/jsonpath/)
```
kubectl get pods --field-selector=status.phase=Failed -o
jsonpath='{.items[*].metadata.namespace}/{.items[*].metadata.name}' >
/opt/KDOB00401/broken.txt
```

<!--
kubectl get pods --field-selector status.phase=Running

kubectl get pods --field-selector status.phase=Failed

kubectl get pods -o=jsonpath='{.items[0].metadata.name}'

kubectl get pods --field-selector status.phase=Failed -o=jsonpath='{.items[0].metadata.namespace}/{.items[0].metadata.name}' > ///

-->

### Configuration
> **ConfigMaps**

## Test

### NO 1 <!--완똑-->
> Modifying the Deployment specifying a readiness probe using path `/healthz`

- https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/

```
vi backend-deploy.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: goproxy
  labels:
    app: goproxy
spec:
  containers:
  - name: goproxy
    image: registry.k8s.io/goproxy:0.1
    ports:
    - containerPort: 8081
    readinessProbe:
      httpGet: ### Add httpGet
        path: /healthz
        port: 8081
      initialDelaySeconds: 8 
      periodSeconds: 5
    livenessProbe:
      tcpSocket:
        port: 8081
      initialDelaySeconds: 15
      periodSeconds: 20
```

```
k apply -f backend-deploy.yaml

k get pods -n staging
k get deploy -n staging
```

### NO2

> A container within the poller pod is hard-coded to connect the nginxsvc service on port 90 . As this
port changes to 5050 an additional container needs to be added to the poller pod which adapts the
container to connect to this new port.

- Update the nginxsvc service to serve on port 5050
- Add an HAproxy container named haproxy bound to port 90 to the poller pod and deploy the
enhanced pod. 
- Use the image haproxy and inject the configuration located at
/opt/KDMC00101/haproxy.cfg, with a **ConfigMap** named haproxy-config, mounted into the container
so that haproxy.cfg is available at /usr/local/etc/haproxy/haproxy.cfg. 
- Ensure that you update the
args of the poller container to connect to localhost instead of nginxsvc so that the connection is
correctly proxied to the new service endpoint. 
- The spec file used to create the initial poller pod is available in
/opt/KDMC00101/poller.yaml

```
k edit svc nginxsvc
## Change the targetPort of the
service to 5050 and save the file 

## Edit pod definition
vi /opt/KDMC00101/poller.yaml

## Add a new container to pod's definition yaml file
```

- [subpath](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath)
```yaml
containers:
- name: haproxy
  image: haproxy
  ports:
  - containerPort: 90
  volumeMounts:
  - name: haproxy-config
    mountPath: /usr/local/etc/haproxy/haproxy.cfg
    subPath: haproxy.cfg
    args: ["haproxy", "-f", "/usr/local/etc/haproxy/haproxy.cfg"]
```

```
k create configmap haproxy-config --from-file=/opt/KDMC00101/haproxy.cfg
```

### NO3 <!--완똑-->
- canary deploy 생성부터하라고 조건제시
- 20% 를 카나리에
- curl 하라는데..난 잘모르겠더라그건

> Canary deployment

<!--
카나리 배포중에 current와 canary가 있는데 canary가 트래픽의 40퍼센트 받도록 service를 수정

-> 아직도 문제가 잘 이해 안되는 부분인데, Max파드가 10개고 canary가 40%정도 비율이여야 한다고 해서 current pod를 6개, canary pod를 4개로 맞추고 모두 service에 붙여줬습니다.
-->

- A Service named krill-Service in the goshark namespace points to 5 pod created by the Deployment
named current-krill-deployment

- Create an identical Deployment named canary-krill-deployment, in the same namespace.
- Modify the Deployment so that:
- A maximum number of 10 pods run in the goshawk namespace.
- 40% of the krill-service 's traffic goes to the canary-krill-deployment pod(s)

```
k get deploy -n goshawk
k scale deploy canary-krill-deployment --replicas=4 -n goshawk
k scale deploy current-krill-deployment --replicas=6 -n goshawk
k get deploy -n goshawk
```

### NO4
> Update deployment and expose service

```
k edit deploy [DEPLOY NAME] -n [NAMESPCE]
```

```yaml
  template:
    metadata:
      labels:
        app: nginx
        func: webFrontEnd ### Add key/value label
```

- Next, create ana deploy in namespace kdsn00l01 a service that accomplishes the following:
* Exposes the service on TCP port 8080
* is mapped to me pods defined by the specification of kdsn00l01-deployment
* Is of type NodePort
* Has a name of cherry

```
k expose deploy kdsn00l01-deployment -n kdsn00l01 --type Nodeport --port 8080 --name cherry
```

### NO5 <!--완똑-->

- A Dockerfile has been prepared at ~/human-stork/build/Dockerfile
- 1) Using the prepared Dockerfile, build a container image with the name macque and lag 3.0. You
may install and use the tool of your choice.
- 2) Using the tool of your choice export the built container image in OC-format and store it at -/human
stork/macque 3.0 tar

```
cd ~/human-stork/build/Dockerfile
ls -al
sudo docker build -t macque:3.0 . ### 으아 이거 너무 아깝다. 현재 경로 `.` 꼭 표시해야함..
sudo docker save macque:3.0 > ~/human-stork/build/Dockerfile
```

<!--
The -t option goes to how Unix/Linux handles terminal access
-->

### NO6
[field selector](https://kubernetes.io/docs/concepts/overview/working-with-objects/field-selectors/)
[json path](https://kubernetes.io/docs/reference/kubectl/jsonpath/)
```
kubectl get pods --field-selector=status.phase=Failed -o
jsonpath='{.items[*].metadata.namespace}/{.items[*].metadata.name}' >
/opt/KDOB00401/broken.txt
```

<!--
kubectl get pods --field-selector status.phase=Running

kubectl get pods --field-selector status.phase=Failed

kubectl get pods -o=jsonpath='{.items[0].metadata.name}'

kubectl get pods --field-selector status.phase=Failed -o=jsonpath='{.items[0].metadata.namespace}/{.items[0].metadata.name}' > ///

-->
### NO7 <!--완똑-->

### NO8 <!--완똑-->

### NO9
- pv
- pvc
- pvc claim

### NO10 <!--완똑-->
> secret

```
k create secret generic app-secret -n default --from-literal=key3=value1
k get secrets

k run nginx-secrets -n default --image=nginx:stable --dry-run=client -o yaml > sec.yaml

vi sec.yaml
```
- [secretKeyRef](https://kubernetes.io/docs/tasks/inject-data-application/distribute-credentials-secure/)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: env-single-secret
spec:
  containers:
  - name: envars-test-container
    image: nginx
    env:
    - name: BEST_VARIABLE
      valueFrom:
        secretKeyRef:
          name: app-secret
          key: key3
```

### NO11
> ConfigMap

* Create a ConfigMap named another-config containing the key/value pair: key4/value3
* start a pod named nginx-configmap containing a single container using the nginx image, and mount the key you just created into the pod under directory /also/a/path

```
k create configmap another-config --from-literal=key4=value3
k get configmap
k run nginx-configmap --image=nginx --dry-run=client -o yaml > pod.yaml

vi pod.yaml
```

- https://kubernetes.io/docs/concepts/configuration/configmap/

```yaml
volumeMounts:
- name: myvol
  mountPath: /also/a/path
volumes:
- name: myvol
  configMap:
    name: another-config
```

```
k apply -f pod.yaml
```
### NO12 <!--완똑-->

### NO13
```
--image=redis:3.2
```

### NO14
* Create a YAML formatted Kubernetes manifest /opt/KDPD00301/periodic.yaml that runs the following shell command: date in a single busybox container. The command should run every minute and must complete within 22 seconds or be terminated oy Kubernetes. The Cronjob namp and container name should both be hello
* Create the resource in the above manifest and verify that the job executes successfully at least once

- https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/

```
k create cronjob hello --image=busybox --schedule "* * * * *" --dry-run=client -o yaml > /opt/KDPD00301/periodic.yaml
```

```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  creationTimestamp: null
  name: hello
spec:
  jobTemplate:
    metadata:
      creationTimestamp: null
      name: hello
    spec:
      template:
        metadata:
          creationTimestamp: null
        spec:
          containers:
          - command:
            - date
            image: busybox
            name: hello
            resources: {}
          restartPolicy: OnFailure
  schedule: '*/1 * * * *'
status: {}
```

```
k get cronjob
```

### NO16
- https://kubernetes.io/docs/concepts/services-networking/network-policies/

```
vi np.yaml
```

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
name: newpod-network-policy
namespace: default
spec:
  podSelector:
    matchLabels:
      app: kdsn00201-newpod
  ingress:
    - from:
    - podSelector:
        matchLabels:
          app: web ###
    - podSelector:
        matchLabels:
          app: storage ###
```



### NO17 <!--완똑-->
- Update the app-a deployment in the production namespace to run as the restrictedservice service account.

```
k set serviceaccount deploy app-a restricservice -n production
```

### NO19 <!--완똑-->
- Limits the memory to half the maximum memory constraint set for the crayfah name space. <!--(이게 좀 당황스러웠던 조건)-->
- https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/

### NO20
```
k top pods -n cpu-stress
echo '...' > ///
```
### NO21 <!--완똑-->


```
k rollout status deploy app -n kdpd00202
k rollout undo deploy app -n kdpd00202
k rollout status deploy app -n kdpd00202
```

### NO22 <!--완똑-->

<!--- 아 이거 틀렸을 것같은데
- web-access=true 이렇게 안하고 web=true 이렇게씀 ㅠㅠ
-->

```
k label pod newpod -n ckad web-access=true
k label pod newpod -n ckad db-access=true
```

### NO23 <!--완똑-->

### NO27 <!--완똑-->
### NO28 <!--완똑-->
- https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/

### NO29 <!--완똑-->
```
k rollout status deploy /// -n ///
k rollout undo deploy ///
k rollout history deploy ///
k get rs -n ///
```
### NO30 <!--완똑-->
- privilege
- https://kubernetes.io/docs/tasks/configure-pod-container/security-context/

```yaml
securityContext:
  runAsUser: 2000
  allowPrivilegeEscalation: false
```
### NO31
- sidecar

