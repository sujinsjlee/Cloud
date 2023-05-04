# Mock Exam 2

#### Solution to the Mock Exam 2

1. Take a backup of the etcd cluster and save it to `/opt/etcd-backup.db`

- [etcd - Backing up the etcd cluster](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster)  

    <details>
    <summary>Answer</summary>

    - etcd cluster is located at **/etc/kubernetes/manifests/etcd.yaml**
    - kubernetes keep manifest of static pods in the `/etc/kubernetes/manifests` folder. etcd.yaml

    ```
    cat /etc/kubernetes/manifests/etcd.yaml | grep file

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
        name: cache-volume ### Should be matched with volumes' name
    volumes:
    - name: cache-volume
        emptyDir:
        sizeLimit: 100Mi
    dnsPolicy: ClusterFirst
    restartPolicy: Always
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
    labels:
        run: super-user-pod
    name: super-user-pod
    spec:
    containers:
    - command:
        - sleep
        - "4800"
        image: busybox:1.28
        name: super-user-pod
        securityContext:
        capabilities:
            add: ["SYS_TIME"]
        resources: {}
    dnsPolicy: ClusterFirst
    restartPolicy: Always
    status: {}
    ```

    ```
    k create -f super.yaml
    ```
    </details>

4. A pod definition file is created at `/root/CKA/use-pv`.yaml. Make use of this manifest file and mount the persistent volume called `pv-1`. Ensure the pod is running and the PV is bound.
    - mountPath: `/data`
    - persistentVolumeClaim Name: `my-pvc`
    - [PV - PersistentVolumeClaims](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

    <details>
    <summary>Answer</summary>
    
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

    - [Claims As Volumes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#claims-as-volumes)  


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
        resources: {}
        volumeMounts:
        - mountPath: "/data"
            name: mypd
    volumes:
        - name: mypd
        persistentVolumeClaim:
            claimName: my-pvc
    dnsPolicy: ClusterFirst
    restartPolicy: Always
    status: {}
    ```

    - **After the file update!! MUST CREATE/APPLY changes of the yaml file**
    
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
    <summary>Answer</summary>

    ```
     k create deploy nginx-deploy --image=nginx:1.16 --replicas=1
    ```

    ```
    k edit deploy nginx-deploy

    # Update 1.16 to 1.17
    ```

    - Another way
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
   <summary>Answer</summary>

   - [Role Binding > CSR > Create a CertificateSigningRequest](https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/#create-certificatessigningrequest)

   - request is the base64 encoded value of the CSR file content. You can get the content using this command:

    ```
    cd /root/CKA
    vi csr.yaml
    cat john.csr | base64 | tr -d "\n"
    ```

    ```yaml
    apiVersion: certificates.k8s.io/v1
    kind: CertificateSigningRequest
    metadata:
      name: john-developer
    spec:
      request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1ZEQ0NBVHdDQVFBd0R6RU5NQXNHQTFVRUF3d0VhbTlvYmpDQ0FTSXdEUVlKS29aSWh2Y05BUUVCQlFBRApnZ0VQQURDQ0FRb0NnZ0VCQUt2Um1tQ0h2ZjBrTHNldlF3aWVKSzcrVVdRck04ZGtkdzkyYUJTdG1uUVNhMGFPCjV3c3cwbVZyNkNjcEJFRmVreHk5NUVydkgyTHhqQTNiSHVsTVVub2ZkUU9rbjYra1NNY2o3TzdWYlBld2k2OEIKa3JoM2prRFNuZGFvV1NPWXBKOFg1WUZ5c2ZvNUpxby82YU92czFGcEc3bm5SMG1JYWpySTlNVVFEdTVncGw4bgpjakY0TG4vQ3NEb3o3QXNadEgwcVpwc0dXYVpURTBKOWNrQmswZWhiV2tMeDJUK3pEYzlmaDVIMjZsSE4zbHM4CktiSlRuSnY3WDFsNndCeTN5WUFUSXRNclpUR28wZ2c1QS9uREZ4SXdHcXNlMTdLZDRaa1k3RDJIZ3R4UytkMEMKMTNBeHNVdzQyWVZ6ZzhkYXJzVGRMZzcxQ2NaanRxdS9YSmlyQmxVQ0F3RUFBYUFBTUEwR0NTcUdTSWIzRFFFQgpDd1VBQTRJQkFRQ1VKTnNMelBKczB2czlGTTVpUzJ0akMyaVYvdXptcmwxTGNUTStsbXpSODNsS09uL0NoMTZlClNLNHplRlFtbGF0c0hCOGZBU2ZhQnRaOUJ2UnVlMUZnbHk1b2VuTk5LaW9FMnc3TUx1a0oyODBWRWFxUjN2SSsKNzRiNnduNkhYclJsYVhaM25VMTFQVTlsT3RBSGxQeDNYVWpCVk5QaGhlUlBmR3p3TTRselZuQW5mNm96bEtxSgpvT3RORStlZ2FYWDdvc3BvZmdWZWVqc25Yd0RjZ05pSFFTbDgzSkljUCtjOVBHMDJtNyt0NmpJU3VoRllTVjZtCmlqblNucHBKZWhFUGxPMkFNcmJzU0VpaFB1N294Wm9iZDFtdWF4bWtVa0NoSzZLeGV0RjVEdWhRMi80NEMvSDIKOWk1bnpMMlRST3RndGRJZjAveUF5N05COHlOY3FPR0QKLS0tLS1FTkQgQ0VSVElGSUNBVEUgUkVRVUVTVC0tLS0tCg==
      usages:
      - client auth
    ```
    
    - CREATE AND **APPROVE CSR**

    ```
    k create -f csr.yaml
    k get csr
    k certificate approve john-developer
    k get csr
    ```
    
    - Create role
    ```
    k create role -h
    k create role developer --verb=create,get,list,update,delete --resource=pods -n developement

    k get role -n development
    ```

    - Role Binding
    
    ```
    k auth can-i get pods --namespace=development --as john 

    k create rolebinding -h
    k create rolebinding john-developer --role=developer --user=john -n development

    k get rolebinding -n development
    k describe rolebinding -n developement

    k auth can-i get pods --namespace=development --as john
    ```

   </details>

7. Create a nginx pod called `nginx-resolver` using image `nginx`, expose it internally with a service called `nginx-resolver-service`. Test that you are able to look up the service and pod names from within the cluster. Use the image: `busybox:1.28` for dns lookup. Record results in `/root/CKA/nginx.svc` and `/root/CKA/nginx.pod`

- Pod: nginx-resolver created
- Service DNS Resolution recorded correctly
- Pod DNS resolution recorded correctly

    <details>
    <summary>Answer</summary>

    ```
    k run nginx-resolver --image=nginx
    ```

    - NGINX port is 80, so let's specify that.

    ```
    k expose pod nginx-resolver --name=nginx-resolver-service --port=80
    ```

    - `-it` gives an interactive terminal to allow you to run commands within the pod
    - `--rm` will automatically perform cleanup of the pod once it's job is completed
    
    - **Before recore the results in .svc, must run the command first and then move the resulrts in .svc**

    ```
    kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service
    
    kubectl run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup nginx-resolver-service > /root/CKA/nginx.svc
    ```

    - [DNS > Pods](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#a-aaaa-records-1)
        - Pod has a DNS name : 
        - `172-17-0-3.default.pod.cluster.local`

    ```
    k get pods -o wide

    k run test-nslookup --image=busybox:1.28 --rm -it --restart=Never -- nslookup 10-244-192-1.default.pod.cluster.local > /root/CKA/nginx.pod 
    ```
    </details>

8. Create a static pod on `node01` called `nginx-critical` with image `nginx` and make sure that it is recreated/restarted automatically in case of a failure.

- Use `/etc/kubernetes/manifests` as the Static Pod path for example.

- static pod configured under /etc/kubernetes/manifests ?

- Pod nginx-critical-node01 is up and running

    <details>
    <summary>Answer</summary>

    ```
    kubectl run nginx-critical --image=nginx --dry-run=client -o yaml > static.yaml
    ```
    
    - Copy the contents of this file or use `scp` command to transfer this file from controlplane to node01 node.

    ```
    scp static.yaml node01:/root/

    k get nodes -o wide
    ssh node01

    cp /root/static.yaml /etc/kubernetes/manifests/

    ssh controlplane
    k get pods
    ```

    </details>
