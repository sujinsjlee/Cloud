# Section 5. Application Lifecycle Management

- [Rolling Updates and Rollbacks](#Rolling-Updates-and-Rollbacks)  
- [Commands and Arguments](#Commands-and-Arguments)    
- [Configure Environment Variables in Applications](#Configure-Environment-Variables-in-Applications)  

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
$ kubectl create configmap my-config --from-literal=key1=config1 --from-literal=key2=config2
```
