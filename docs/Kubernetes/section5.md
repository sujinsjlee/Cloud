# Section 5. Application Lifecycle Management

- [Rolling Updates and Rollbacks](#Rolling-Updates-and-Rollbacks)  
- [Commands and Arguments](#Commands-and-Arguments)    
- [Configure Environment Variables in Applications](#Configure-Environment-Variables-in-Applications)  
- [Configure Secrets in Applications](#Configure-Secrets-in-Applications)  
- [Multi Container Pods](#Multi-Container-Pods)  
- [Init Containers](#Init-Containers)    


## Rolling Updates and Rollbacks

- See the status of the rollout by the below command
  ```
  ~# kubectl rollout status deployment/myapp-deployment
  ```
- To see the history and revisions
  ```
  ~# kubectl rollout history deployment/myapp-deployment
  ```

- By Rolling Update, the application never goes down and the upgrade is seamless.
![ru](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/dst.PNG)

![ru2](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/rcrl.PNG)

![command](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/sum.PNG) 

- Command

```
~# kubectl create -f deployment-definition.yaml
~# kubectl get deployments
~# kubectl apply -f deployment-definition.yaml
~# kubectl set image deployment/myapp-deployment nginx=nginx:1.9.1
~# kubectl rollout status deployment/myapp-deployment
~# kubectl rollout history deployment/myapp-deployment
~# kubectl rollout undo deployment/myapp-deployment
```

## Commands and Arguments

![commands](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/args.PNG)
- There are two fields that correspond to two instructions in the docker file.
- The `command` field overrides the entry point instruction
- The `args` field overrides the command instruction in the docker file.

### Practice
- Replace pod
```console
~# k replace --force -f [yaml file]
```
- Start the nginx pod using the default command, but use custom arguments (arg1 .. argN) for that command

```console
~# kubectl run nginx --image=nginx -- <arg1> <arg2> ... <argN>
```

## Configure Environment Variables in Applications
![c](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/cms.PNG)

### ConfigMaps
- When you have a lot of pod definition files, it will become difficult to manage the environment data stored within the query's files.
- We can take this information out of the pod definition file and manage it centrally using configuration maps.

- There are 2 phases involved in configuring ConfigMaps. 
  - First, create the configMaps
  - Second, Inject then into the pod.
- There are 2 ways of creating a configmap.
  - The Imperative way
    ```console
    ~# kubectl create configmap app-config --from-literal=APP_COLOR=blue --from-literal=APP_MODE=prod
    ~# kubectl create configmap app-config --from-file=app_config.properties (Another way)
    ```
    ![1](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/cmi.PNG)

  - The Declarative way
    - You can create as many config maps as you need in the same way for various different purposes.
    
    ```yaml
    apiVersion: v1
    kind: ConfigMap ##kind is set as ConfigMap
    metadata:
     name: app-config ## the config map file name
    data:
     APP_COLOR: blue
     APP_MODE: prod
    ```
    ```
    Create a config map definition file and run the 'kubectl create` command to deploy it.
    ~# kubectl create -f config-map.yaml
    ```  
    ![22](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/cmd1.PNG)

 - To **view configMaps**
   ```console
   ~# kubectl get configmaps (or)
   ~# kubectl get cm
   ```
 - To describe configmaps
   ```console
   ~# kubectl describe configmaps [configmaps-name]
   ~# kubectl describe cm [configmaps-name]
   ```

- **Inject configmap in pod**
    - The **`envFrom`** property is a list.
    - So, we can pass as many environment variables as required.
    - Each item in the list corresponds to config map item.
   
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
        name: simple-webapp-color
    spec:
        containers:
        - name: simple-webapp-color
        image: simple-webapp-color
        ports:
        - containerPort: 8080
        envFrom: ## envFrom property !!!
        - configMapRef:
            name: app-config
    ```

    ```console
    ~# kubectl create -f pod-definition.yaml
    ```
### Practice

```console
# Create a new configMap named my-config with key1=config1 and key2=config2
~# kubectl create configmap my-config --from-literal=key1=config1 --from-literal=key2=config2
```

## Configure Secrets in Applications
- **Secrets** are used to store sensitive information. They are similar to configmaps but they are stored in an encrypted format or a hashed format.

- There are 2 steps involved with secrets
![secret1](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/sec.PNG)
  - First, Create a secret
  - Second, Inject the secret into a pod.

- There are 2 ways of creating a secret
  - The Imperative way
  ![secret2](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/csi.PNG)

  - The Declaritive way

  ```console
  ~# kubectl create secret generic [Secret NAME] --from-literal=[ID]=[PASSWORD]
  ~# kubectl create secret generic app-secret --from-literal=DB_Host=mysql --from-literal=DB_User=root --from-literal=DB_Password=paswrd
  ~# kubectl create secret generic app-secret --from-file=app_secret.properties
  ```
  - echo to generate a hash value of the password and pass it to secret-data.yaml definition value as a value to DB_Password variable.

  ```console
  ~# echo -n "mysql" | base64
  ~# echo -n "root" | base64
  ~# echo -n "paswrd"| base64
  ```
  - `echo` is a Unix/Linux command tool used for displaying lines of text or string which are passed as arguments on the command line.

  - The `base64` command encodes binary strings into text representations using the base64 encoding format. Base64 encoding is often used in LDIF files to represent non-ASCII character strings.

  - To decode secrets
  ```console
  ~# echo -n "bX1zcWw=" | base64 --decode
  ~# echo -n "cm9vdA==" | base64 --decode
  ~# echo -n "cGFzd3Jk" | base64 --decode
  ```
  - Create a secret definition file and run kubectl create to deploy it

  ```yaml
  apiVersion: v1
  kind: Secret
  metadata:
  name: app-secret
  data:
    DB_Host: bX1zcWw=
    DB_User: cm9vdA==
    DB_Password: cGFzd3Jk
  ```

  ```console
  ~# kubectl create -f secret-data.yaml
  ```

### View Secrets
- To view secrets
  ```console
  ~# kubectl get secrets
  ```
- To describe secret
  ```console
  ~# kubectl describe secret
  ```
- To view the values of the secret
  ```console
  ~# kubectl get secret app-secret -o yaml
  ```

### Configuring secret with a pod

- To inject a secret to a pod add a new property **`envFrom`** followed by **`secretRef`** name and then create the pod-definition
  
  ```yaml
  apiVersion: v1
  kind: Secret
  metadata:
   name: app-secret
  data:
    DB_Host: bX1zcWw=
    DB_User: cm9vdA==
    DB_Password: cGFzd3Jk
  ```
  ```yaml
   apiVersion: v1
   kind: Pod ### Configure secret with a pod
   metadata:
     name: simple-webapp-color
   spec:
    containers:
    - name: simple-webapp-color
      image: simple-webapp-color
      ports:
      - containerPort: 8080
      envFrom:
      - secretRef: ### secretRef !!!
          name: app-secret
   ```

  ```console
  ~# kubectl create -f pod-definition.yaml
  ```

- There are other ways to inject secrets into pods.
  - You can inject as `Single ENV` variable
  - You can inject as whole secret as files in a `Volume`

![Secrets](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/seco.PNG)

<!--
https://tech.kakao.com/2021/12/20/kubernetes-etcd/
** ETCD
Container Cloud 관련 업무를 하고 계신 분이라면 모두 etcd를 한 번쯤은 들어보셨을 거라 생각합니다. 왜냐하면, 컨테이너 생태계에서 사실상 표준이 된 Kubernetes가 컨트롤 플레인 컴포넌트로 채택을 했기 때문이죠.

Etcd는 key:value 형태의 데이터를 저장하는 스토리지입니다. etcd가 다운된다면 Kubernetes 클러스터는 제대로 동작하지 못하게 되므로 높은 신뢰성을 제공해야 합니다.

Kubernetes는 기반 스토리지(backing storage)로 etcd를 사용하고 있고, 모든 데이터가 etcd에 보관됩니다. 예를 들어, 클러스터에 어떤 노드가 몇 개나 있고 어떤 파드가 어떤 노드에서 동작하고 있는지가 etcd에 기록됩니다. 만약 동작 중인 클러스터의 etcd 데이터베이스가 유실된다면 컨테이너뿐만 아니라 클러스터가 사용하는 모든 리소스가 미아가 되어 버립니다.
-->

## Multi Container Pods
![mp](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/mcpc.PNG)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-webapp
  labels:
    name: simple-webapp
spec:
  containers:
  - name: simple-webapp ## Container 1

    image: simple-webapp
    ports:
    - ContainerPort: 8080
  - name: log-agent ## Container 2
    image: log-agent
```

### Practice

- Check how many containers in the POD
  - **READY** shows the number of running POD/existing POD
```console
~#  k get pods
NAME        READY   STATUS              RESTARTS   AGE
app         0/1     ContainerCreating   0          22s
fluent-ui   0/1     ContainerCreating   0          22s
red         0/3     ContainerCreating   0          13s
```

- **Volume**
<!--
In computers, to mount is to make a group of files in a file system structure accessible to a user or user group. 

디스크와 같은 물리적인 장치를 특정위치인 디렉토리에 연결시켜주는 것을 마운트라고 합니다.

쿠버네티스 파드 내에서 돌아가는 컨테이너는 고유한 파일 시스템을 갖습니다. 파일 시스템은 컨테이너 이미지에서 제공되기 때문입니다. 그렇기 때문에 컨테이너가 재시작하게 되면 이전에 컨테이너에서 쓰여진 파일시스템은 새롭게 시작된 컨테이너가 볼 수 없습니다. 만약 컨테이너가 종료되더라도 파일시스템이 유지되도록 하고 싶다면 어떻게 하면 좋을까요?

이 경우 쿠버네티스 볼륨을 정의할 수 있습니다. 볼륨은 파드의 일부분으로 정의되며 파드와 동일한 라이프사이클을 갖는 디스크 스토리지입니다. 파드가 여러 개의 컨테이너를 가지는 경우 모든 컨테이너가 볼륨을 공유할 수도 있습니다. 

쿠버네티스 볼륨은 파드의 구성 요소로 컨테이너와 동일하게 파드 스펙에서 정의됩니다. 볼륨은 독립적인 쿠버네티스 리소스가 아니므로 자체적으로 생성, 삭제될 수 없습니다. 볼륨은 파드의 모든 컨테이너에서 사용 가능하며 이 경우 접근하려는 각 컨테이너에서 마운트해야 합니다. 각 컨테이너 파일 시스템의 어느 경로에나 볼륨을 마운트 할 수 있습니다.
-->
    - On-disk files in a container are ephemeral, which presents some problems for non-trivial applications when running in containers. `One problem is the loss of files when a container crashes. The kubelet restarts the container but with a clean state.` A second problem occurs when sharing files between containers running together in a Pod.
    - Kubernetes supports many types of volumes. A Pod can use any number of volume types simultaneously. Ephemeral volume types have a lifetime of a pod, but persistent volumes exist beyond the lifetime of a pod. When a pod ceases to exist, Kubernetes destroys ephemeral volumes; however, Kubernetes does not destroy persistent volumes. `For any kind of volume in a given pod, data is preserved across container restarts.`

## Init Containers 
[Init Containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)

- In a multi-container pod, *each container is expected to run a process that stays alive as long as the POD's lifecycle.* For example in the multi-container pod that we talked about earlier that has a web application and logging agent, both the containers are expected to stay alive at all times. The process running in the log agent container is expected to stay alive as long as the web application is running. If any of them fails, the POD restarts.

- *But at times you may want to run a process that runs to completion in a container.* For example a process that pulls a code or binary from a repository that will be used by the main web application. That is a task that will be run only  one time when the pod is first created. Or a process that waits  for an external service or database to be up before the actual application starts. That's where **initContainers** comes in.

- An **initContainer** is configured in a pod like all other containers, except that it is specified inside a `initContainers` section, like this:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox:1.28
    command: ['sh', '-c', 'echo The app is running! && sleep 3600']
  initContainers:
  - name: init-myservice
    image: busybox
    command: ['sh', '-c', 'git clone <some-repository-that-will-be-used-by-application> ; done;']
```
- When a POD is first created the initContainer is run, and the process in the initContainer must run to a completion before the real container hosting the application starts. 

- You can configure multiple such initContainers as well, like how we did for multi-pod containers. In that case each init container is run one at a time in sequential order.

- If any of the initContainers fail to complete, Kubernetes restarts the Pod repeatedly until the Init Container succeeds.


### Practice
- If the Exit Code is `0` : the container exited normally, no troubleshooting is required.

- Get logs of the specific container of specific POD

```console
kubectl logs [-f] [-p] (POD | TYPE/NAME) [-c CONTAINER] [options]

~# k logs [POD NAME] -c [CONTAINER NAME]
~# k logs orange -c init-myservice
```
