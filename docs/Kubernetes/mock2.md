# Mock Exam 2

#### Solution to the Mock Exam 2

1. Take a backup of the etcd cluster and save it to `/opt/etcd-backup.db`

- [etcd - backup](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster)  

    <details>
    <summary>Answer</summary>

    ```
    cat /etc/kubernetes/manifests/etcd.yaml | grep file

    ETCDCTL_API=3 etcdctl snapshot save --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key --endpoints=127.0.0.1:2379 /opt/etcd-backup.db

    ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
    --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key \
    snapshot save /opt/etcd-backup.db

    ls /opt/etcd-backup.db
    ```
    </details>

2. Create a Pod called `redis-storage` with image: `redis:alpine` with a Volume of type `emptyDir` that lasts for the life of the Pod.
    - Pod named 'redis-storage' created
    - Pod 'redis-storage' uses Volume type of emptyDir
    - Pod 'redis-storage' uses volumeMount with mountPath = /data/redis

    > [volumes - emptyDir](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir)  

    
    <details>
    <summary>Answer</summary>
    
    ```
    k run redis-storage --image=redis:alpine --dry-run=client -o yaml > redis-storage.yaml
    ```


    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
    creationTimestamp: null
    labels:
        run: redis-storage
    name: redis-storage
    spec:
    containers:
    - image: redis:alpine
        name: redis-storage    
        resources: {}
        volumeMounts:
        - mountPath: /data/redis
        name: cache-volume  ### Should be matched with volumes' name
    dnsPolicy: ClusterFirst
    restartPolicy: Always
    volumes:
    - name: cache-volume
        emptyDir: {} ### !!!
    status: {}
    ```

    ```
    k create -f redis-storage.yaml
    ```
    </details>



3. Create a new pod called `super-user-pod` with image `busybox:1.28`. Allow the pod to be able to set `system_time`. The container should sleep for 4800 seconds.

    - Pod: super-user-pod
    - Container Image: busybox:1.28
    - SYS_TIME capabilities for the conatiner?
    - [Cheat Sheet - sleep](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)  
    - [security capability - SYS_TIME](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)

    <details>
    <summary>Answer</summary>

    ```
    k run super-user-pod --image=busybox:1.28 --dry-run=client -o yaml > super.yaml

    vi syper.yaml
    ```


    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
    creationTimestamp: null
    name: super-user-pod
    spec:
    containers:
    - image: busybox:1.28
        name: super-user-pod
        args:
        - sleep
        - "4800"
        securityContext:
        capabilities:
            add: ["SYS_TIME"]
    ```

    ```
    k create -f super.yaml
    ```
    </details>

4. A pod definition file is created at `/root/CKA/use-pv`.yaml. Make use of this manifest file and mount the persistent volume called `pv-1`. Ensure the pod is running and the PV is bound.
    - mountPath: `/data`
    - persistentVolumeClaim Name: `my-pvc`
    - [PV](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

    <details>
    
    ```
    vi pvc.yaml
    ```


    ```yaml
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
    name: my-pvc
    spec:
    accessModes:
        - ReadWriteOnce
    volumeMode: Filesystem
    resources:
        requests:
        storage: 10Mi ### 8Gi --> Not working 
    ```

    ```
    k create -f pvc.yaml
    vi /root/CKA/use-pv
    ```

    - [Claim as Volume](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#claims-as-volumes)  


    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
    creationTimestamp: null
    labels:
        run: use-pv
    name: use-pv
    spec:
    containers:
    - image: nginx
        name: use-pv
        volumeMounts:
        - mountPath: "/data"
            name: pv-1
        resources: {}
    volumes:
    - name: pv-1
        persistentVolumeClaim:
        claimName: my-pvc
    dnsPolicy: ClusterFirst
    restartPolicy: Always
    status: {}
    ```

    ```
    k create -f /root/CKA/use-pv.yaml
    ```

    </details>

5. Create a new deployment called `nginx-deploy`, with image `nginx:1.16` and `1` replica. Next upgrade the deployment to version `1.17` using rolling update.
    - Deployment : nginx-deploy. Image: nginx:1.16
    - Image: nginx:1.16
    - Task: Upgrade the version of the deployment to 1:17
    - Task: Record the changes for the image upgrade

    <details>

    ```
     k create deploy nginx-deploy --image=nginx:1.16 --replicas=1
    ```

    - https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#updating-a-deployment

    ```
    k set image -h
    k set image deployment/nginx-deploy nginx=nginx:1.17
    ```

    </details>

6. Create a new user called `john`. Grant him access to the cluster. John should have permission to `create, list, get, update and delete pods` in the `development` namespace . The private key exists in the location: `/root/CKA/john.key` and csr at `/root/CKA/john.csr`.
- Important Note: As of kubernetes 1.19, the CertificateSigningRequest object expects a `signerName`.
- CSR: john-developer Status:Approved
- Role Name: developer, namespace: development, Resource: Pods
- Access: User 'john' has appropriate permissions

> [Role Biding - CSR](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)


    <details>
    
    ```
    cd /root/CKA
    cat john.csr

    vi csr.yaml
    ```

    - https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/#create-certificatessigningrequest

    - request is the base64 encoded value of the CSR file content. You can get the content using this command:


    ```
    cat john.csr | base64 | tr -d "\n"
    ```

    ```yaml
    apiVersion: certificates.k8s.io/v1
    kind: CertificateSigningRequest
    metadata:
    name: john-developer
    spec:
    signerName: kubernetes.io/kube-apiserver-client
    request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1ZEQ0NBVHdDQVFBd0R6RU5NQXNHQTFVRUF3d0VhbTlvYmpDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRApnZ0VQQURDQ0FRb0NnZ0VCQUt2Um1tQ0h2ZjBrTHNldlF3aWVKSzcrVVdRck04ZGtkdzkyYUJTdG1uUVNhMGFPCjV3c3cwbVZyNkNjcEJFRmVreHk5NUVydkgyTHhqQTNiSHVsTVVub2ZkUU9rbjYra1NNY2o3TzdWYlBld2k2OEIKa3JoM2prRFNuZGFvV1NPWXBKOFg1WUZ5c2ZvNUpxby82YU92czFGcEc3bm5SMG1JYWpySTlNVVFEdTVncGw4bgpjakY0TG4vQ3NEb3o3QXNadEgwcVpwc0dXYVpURTBKOWNrQmswZWhiV2tMeDJUK3pEYzlmaDVIMjZsSE4zbHM4CktiSlRuSnY3WDFsNndCeTN5WUFUSXRNclpUR28wZ2c1QS9uREZ4SXdHcXNlMTdLZDRaa1k3RDJIZ3R4UytkMEMKMTNBeHNVdzQyWVZ6ZzhkYXJzVGRMZzcxQ2NaanRxdS9YSmlyQmxVQ0F3RUFBYUFBTUEwR0NTcUdTSWIzRFFFQgpDd1VBQTRJQkFRQ1VKTnNMelBKczB2czlGTTVpUzJ0akMyaVYvdXptcmwxTGNUTStsbXpSODNsS09uL0NoMTZlClNLNHplRlFtbGF0c0hCOGZBU2ZhQnRaOUJ2UnVlMUZnbHk1b2VuTk5LaW9FMnc3TUx1a0oyODBWRWFxUjN2SSsKNzRiNnduNkhYclJsYVhaM25VMTFQVTlsT3RBSGxQeDNYVWpCVk5QaGhlUlBmR3p3TTRselZuQW5mNm96bEtxSgpvT3RORStlZ2FYWDdvc3BvZmdWZWVqc25Yd0RjZ05pSFFTbDgzSkljUCtjOVBHMDJtNyt0NmpJU3VoRllTVjZtCmlqblNucHBKZWhFUGxPMkFNcmJzU0VpaFB1N294Wm9iZDFtdWF4bWtVa0NoSzZLeGV0RjVEdWhRMi80NEMvSDIKOWk1bnpMMlRST3RndGRJZjAveUF5N05COHlOY3FPR0QKLS0tLS1FTkQgQ0VSVElGSUNBVEUgUkVRVUVTVC0tLS0tCg==
    usages:
    - digital signature
    - key encipherment
    - client auth
    groups:
    - system:authenticated
    ```
    
    ```
    k create -f csr.yaml
    k get csr
    k certificate approve john-developer
    k get csr

    k create role -h
    k create role developer --verb=create,get,list,update,delete --resource=pods -n developement

    k get role -n development
    k  auth --help
    ```

    </details>

7. Run the below command for solution:

    <details>

    ```
    kubectl run nginx-resolver --image=nginx
    kubectl expose pod nginx-resolver --name=nginx-resolver-service --port=80 --target-port=80 --type=ClusterIP
    kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service
    kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service > /root/CKA/nginx.svc

    Get the IP of the nginx-resolver pod and replace the dots(.) with hyphon(-) which will be used below.

    kubectl get pod nginx-resolver -o wide
    kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup <P-O-D-I-P.default.pod> > /root/CKA/nginx.pod

    ```

    </details>

8. Run the below command for solution:

    <details>

    ```
    kubectl run nginx-critical --image=nginx --dry-run=client -o yaml > static.yaml
    
    cat static.yaml - Copy the contents of this file.

    kubectl get nodes -o wide
    ssh node01 
    OR
    ssh <IP of node01>

    Check if static-pod directory is present which is /etc/kubernetes/manifests if not then create it.
    mkdir -p /etc/kubernetes/manifests

    Paste the contents of the file(static.yaml) copied in the first step to file nginx-critical.yaml.

    Move/copy the nginx-critical.yaml to path /etc/kubernetes/manifests/

    cp nginx-critical.yaml /etc/kubernetes/manifests

    Go back to master node

    kubectl get pods 
    ```

    </details>
