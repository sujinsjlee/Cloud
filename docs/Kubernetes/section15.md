# Section15 - Lightning Lab

- [Quiz1](#Quiz1)  
- [Quiz2](#Quiz2)  
- [Quiz3](#Quiz3)  
- [Quiz4](#Quiz4)  
- [Quiz5](#Quiz5)  
- [Quiz6](#Quiz6)  
- [Quiz7](#Quiz7)  

## Quiz1

- Upgrade the current version of kubernetes from `1.25.0` to `1.26.0` exactly using the `kubeadm` utility. Make sure that the upgrade is carried out one node at a time starting with the controlplane node. To minimize downtime, the deployment `gold-nginx` should be rescheduled on an alternate node before upgrading each node.
    - Upgrade `controlplane` node first and drain node `node01` before upgrading it. Pods for `gold-nginx` should run on the `controlplane` node subsequently.

> Answer

```
# replace <node-to-drain> with the name of your node you are draining
kubectl drain <node-to-drain> --ignore-daemonsets

# Determine which version to upgrade to
apt update
apt-cache madison kubeadm


# replace x in 1.26.x-00 with the latest patch version
apt-mark unhold kubeadm && \
apt-get update && apt-get install -y kubeadm=1.26.0-00 && \
apt-mark hold kubeadm

kubeadm version
kubeadm upgrade plan v1.26.0
kubeadm upgrade apply v1.26.0

# replace x in 1.26.x-00 with the latest patch version
apt-mark unhold kubelet kubectl && \
apt-get update && apt-get install -y kubelet=1.26.0-00 kubectl=1.26.0-00 && \
apt-mark hold kubelet kubectl

# Restart the kubelet:

sudo systemctl daemon-reload
sudo systemctl restart kubelet

# replace <node-to-uncordon> with the name of your node
kubectl uncordon <node-to-uncordon>
```

- Before draining node01, we need to remove the taint from the controlplane node.

```
# Identify the taint first. 
root@controlplane:~# kubectl describe node controlplane | grep -i taint

# Remove the taint with help of "kubectl taint" command.
root@controlplane:~# kubectl taint node controlplane node-role.kubernetes.io/control-plane:NoSchedule-

# Verify it, the taint has been removed successfully.  
root@controlplane:~# kubectl describe node controlplane | grep -i taint
```

- Upgrade node01

```
# replace <node-to-drain> with the name of your node you are draining
kubectl drain <node-to-drain> --ignore-daemonsets


ssh node01

# replace x in 1.26.x-00 with the latest patch version
apt-mark unhold kubeadm && \
apt-get update && apt-get install -y kubeadm=1.26.0-00 && \
apt-mark hold kubeadm

kubeadm version

# replace x in 1.26.x-00 with the latest patch version
apt-mark unhold kubelet kubectl && \
apt-get update && apt-get install -y kubelet=1.26.0-00 kubectl=1.26.0-00 && \
apt-mark hold kubelet kubectl

# Restart the kubelet:
sudo systemctl daemon-reload
sudo systemctl restart kubelet

exit

# replace <node-to-uncordon> with the name of your node
kubectl uncordon <node-to-uncordon>
```

```
root@controlplane:~# kubectl get pods -o wide | grep gold (make sure this is scheduled on node)
```

- [K8s documentation - Upgrading kubeadm clusters](https://v1-26.docs.kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)

- [CKA study document](https://sulky-ranunculus-577.notion.site/3-0b87326714eb416f8b0fcfe5443512ab)


## Quiz2
- Print the names of all deployments in the `admin2406` namespace in the following format:  

`DEPLOYMENT CONTAINER_IMAGE READY_REPLICAS NAMESPACE`  

`<deployment name> <container image used> <ready replica count> <Namespace>`
. The data should be sorted by the increasing order of the `deployment name`.  

Example:  

`DEPLOYMENT CONTAINER_IMAGE READY_REPLICAS NAMESPACE`  
`deploy0 nginx:alpine 1 admin2406`  
Write the result to the file `/opt/admin2406_data.`  

> Answer


```
kubectl get deployment -n admin2406

kubectl get deployment deploy1 -n admin2406 -o json

kubectl -n admin2406 get deployment -o custom-columns=DEPLOYMENT:.metadata.name,CONTAINER_IMAGE:.spec.template.spec.containers[].image,READY_REPLICAS:.status.readyReplicas,NAMESPACE:.metadata.namespace --sort-by=.metadata.name > /opt/admin2406_data
```

- [Command line tool - sorting list object](https://kubernetes.io/docs/reference/kubectl/overview/#sorting-list-objects)

- [Command line tool - custom columns](https://kubernetes.io/docs/reference/kubectl/overview/#custom-columns)

## Quiz3
- A kubeconfig file called `admin.kubeconfig` has been created in `/root/CKA`. There is something wrong with the configuration. Troubleshoot and fix it.

> Answer

<!--
cat /root/CKA/admin.kubeconfig

파일을 확인해보면

server: https://172.17.0.28:2379 항목이 있는데

2379 port는 etcd가 사용하는 port 임

kube-apiserver에 붙기 위해서는 6443 port를 사용해야함.

 

해당 부분을 6443으로 변경해야함.
-->

```
vi /root/CKA/admin.kubeconfig

# change server port from 4380 to 6443
```

- [Ports and Protocols - Control Plane port 6443](https://kubernetes.io/docs/reference/networking/ports-and-protocols/)

## Quiz4

- Create a new deployment called `nginx-deploy`, with image `nginx:1.16` and `1` replica. Next upgrade the deployment to version `1.17` using `rolling update`.

    - Image: nginx:1.16
    - Task: Upgrade the version of the deployment to 1:17

> Answer

```
k create deployment nginx-deploy --image=nginx:1.16 --replicas=1
k edit deployment nginx-deploy

# search 1.16 and update 1.16 -> 1.17
```


## Quiz5

- A new deployment called `alpha-mysql` has been deployed in the `alpha` namespace. However, the pods are not running. Troubleshoot and fix the issue. The deployment should make use of the persistent volume `alpha-pv` to be mounted at `/var/lib/mysql` and should use the environment variable `MYSQL_ALLOW_EMPTY_PASSWORD=1` to make use of an empty root password.

Important: Do not alter the persistent volume.

- Troubleshoot and fix the issues

> Answer

```
k get deploy -n alpha

# Check pod
k get pod -A -n alpha
k describe pod  alpha-mysql-6d945ffc78-6f7mm -n alpha

Events:
  Type     Reason            Age   From               Message
  ----     ------            ----  ----               -------
  Warning  FailedScheduling  17m   default-scheduler  0/2 nodes are available: 2 persistentvolumeclaim "mysql-alpha-pvc" not found. preemption: 0/2 nodes are available: 2 Preemption is not helpful for scheduling.
  Warning  FailedScheduling  17m   default-scheduler  0/2 nodes are available: 2 persistentvolumeclaim "mysql-alpha-pvc" not found. preemption: 0/2 nodes are available: 2 Preemption is not helpful for scheduling.

controlplane ~ ➜  k get pv -n alpha 
NAME       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
alpha-pv   1Gi        RWO            Retain           Available           slow                    28m

controlplane ~ ➜  k get pvc -n alpha 
NAME          STATUS    VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS   AGE
alpha-claim   Pending                                      slow-storage   28m
```

- Edit pvc - **should write NAMESPACE**

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-alpha-pvc
  namespace: alpha ###!!!
spec:
  storageClassName: slow ### NOT slow-storage BUT slow (Error pod's storageClassName is slow)
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```


```
k apply -f pvc.yaml
```

- [Create a PVC](https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/#create-a-persistentvolumeclaim)

<!--
[pv 확인]

kubectl get pv -n alpha
확인후 어떤 storagecalss 를 사용하는지 확인
kubectl describe pv alpha-pv -n alpha

(결과)
slow storageclass를 사용.
-->


## Quiz6

- Take the backup of ETCD at the location `/opt/etcd-backup.db `on the `controlplane` node.

    - Troubleshoot and fix the issues

> Answer


```
k get all -n kube-system
k describe pod/etcd-controlplane -n kube-system

ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
  --cacert=<trusted-ca-file> --cert=<cert-file> --key=<key-file> \
  snapshot save <backup-file-location>


ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key \
  snapshot save /opt/etcd-backup.db
```

- [Backup ETCD - Shapshot using etcdctl options](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#snapshot-using-etcdctl-options)


## Quiz7

- Create a pod called `secret-1401` in the `admin1401` namespace using the `busybox` image. The container within the pod should be called `secret-admin` and should sleep for `4800` seconds.

The container should mount a `read-only` secret volume called `secret-volume` at the path `/etc/secret-volume`. The secret being mounted has already been created for you and is called `dotfile-secret`.

> Answer

```
k run secret-1401 -n admin1402 --image=busybox --dry-run=client -o yaml > pod1.yaml
vi pod1.yaml
```


```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: secret-1401
  name: secret-1401
  namespace: admin1401
spec:
  volumes:
  - name: secret-volume
    # secret volume
    secret:
      secretName: dotfile-secret
  containers:
  - command:
    - sleep
    - "4800"
    image: busybox
    name: secret-admin
    # volumes' mount path
    volumeMounts:
    - name: secret-volume
      readOnly: true
      mountPath: "/etc/secret-volume"
```

```
k apply -f pod1.yaml
```

- [Secrets](https://kubernetes.io/docs/concepts/configuration/secret/#using-secrets)

- https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/
