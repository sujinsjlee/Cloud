# Mock Exam 1

#### Solution to the Mock Exam 1

1. Deploy a pod named `nginx-pod` using the `nginx:alpine` image.

    <details>
    <summary>Answer</summary>

    ```
    k run nginx-pod --image=nginx:alpine
    ```

    </details>

2. Deploy a `messaging` pod using the `redis:alpine` image with the labels set to `tier=msg`.

    <details>
    <summary>Answer</summary>

    ```
    kubectl run messaging --image=redis:alpine --labels="tier=msg"
    kubectl run messaging --image=redis:alpine --labels=tier=msg
    ```

    </details>


3. Create a namespace named `apx-x9984574`.
    
    <details>
    <summary>Answer</summary>

    ```
    k create namespace apx-x9984574
    kubectl create ns apx-x9984574
    ```

    </details>

4. Get the list of nodes in JSON format and store it in a file at `/opt/outputs/nodes-z3444kd9.json`.

    <details>
    <summary>Answer</summary>

    ```
    kubectl get nodes -o json > /opt/outputs/nodes-z3444kd9.json

    kubectl get no -o json > /opt/outputs/nodes-z3444kd9.json
    ```

    </details>

5. Create a service messaging-service to expose the messaging application within the cluster on port 6379.

- Use imperative commands.

    - Service: messaging-service
    - Port: 6379
    - Type: ClusterIp
    - Use the right labels

    <details>
    <summary>Answer</summary>

    ```
    k expose pod -h

    kubectl expose pod messaging --port=6379 --name messaging-service
    k expose pod messaging --port=6379 --name=messaging-service

    k get svc
    ```

    </details>

6. Create a deployment named `hr-web-app` using the image `kodekloud/webapp-color` with `2` replicas.

    <details>
    <summary>Answer</summary>


    ```
    kubectl create deployment hr-web-app --image=kodekloud/webapp-color --replicas=2

    k create deploy hr-web-app --image=kodekloud/webapp-color --replicas=2
    ```

    </details>

7. To Create a static pod, copy it to the static pods directory. In this case, it is `/etc/kubernetes/manifests`. Apply below manifests:

- [K8s Documentation - static pod](https://kubernetes.io/docs/tasks/configure-pod-container/static-pod/)

    <details>
    <summary>Answer</summary>

    - The running kubelet periodically scans the configured directory (`/etc/kubernetes/manifests` in our example) for changes and adds/removes Pods as files appear/disappear in this directory.

    ```
    k run static-busybox --image=busybox --dry-run=client -o yaml --command -- sleep 1000 > static-busybox.yaml


    # This assumes you are using filesystem-hosted static Pod configuration
    # Run these commands on the node where the container is running

    mv static-busybox.yaml /etc/kubernetes/manifests/

    k get pods
    ```
    </details>

8. Run below command to create a pod in namespace `finance`:

    <details>
    <summary>Answer</summary>

    ```
    k create namespace temp-bus
    kubectl run finance --image=redis:alpine -n temp-bus
    ```
    </details>

9. A new application `orange` is deployed. There is something wrong with it. Identify and fix the issue.

    <details>
    <summary>Answer</summary>

    ```
    kubectl describe pod orange
    k logs orange [Container NAME]
    k logs orange init-myservice
    ```

    Export the running pod using below command and correct the spelling of the command **`sleeeep`** to **`sleep`** 

    ```
    k edit pod orange
    k replace --force -f /tmp/...yaml
    ```

    </details>

10. Expose the `hr-web-app` as service `hr-web-app-service` application on port `30082` on the nodes on the cluster. The web application listens on port `8080`.

    <details>
    <summary>Answer</summary>

    ```
    k get deploy
   
    k expose deploy hr-web-app --type=NodePort --port=8080 --name=hr-web-app-service --dry-run=client -o yaml > hr-web-app-service.yaml

    k get svc

    k describe svc hr-web-app-service

    k edit svc hr-web-app-service

    # Change the NodePort to 30082
    ```
    </details>

11. Use JSON PATH query to retrieve the `osImages` of all the nodes and store it in a file `/opt/outputs/nodes_os_x43kj56.txt`.

- [jsonpath](https://kubernetes.io/docs/reference/kubectl/jsonpath/)

    <details>
    <summary>Answer</summary>

    ``` 
    kubectl get nodes -o=jsonpath='{.items[*].status.nodeInfo.osImage}' > /opt/outputs/nodes_os_x43kj56.txt
    ```

    <!-- %키를 누르면 지금 커서가 위치하는 곳에 있는 괄호와 짝이 맞는 괄호를 찾아줍니다.-->
    - In vi **%** key
        - To jump to a matching opening or closing parenthesis, square bracket or a curly brace: `([{}])`

    </details>

12. Create a Persistent Volume with the given specification.
- Volume Name: pv-analytics
- Storage: 100Mi
- Access modes: ReadWriteMany
- Host Path: /pv/data-analytics

- [pv](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistent-volumes)


    <details>
    <summary>Answer</summary>

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
        storage: 100Mi
        volumeMode: Filesystem
        accessModes:
        - ReadWriteMany
        hostPath:
            path: /pv/data-analytics
    ```

    ```
    k create -f pv.yaml
    k get pv
    ```

    </details>
        
