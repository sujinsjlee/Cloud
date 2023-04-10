# Section 6 - Cluster Maintenance
- [OS Upgrade](#OS-Upgrade)  
- [Cluster Upgrade](#Cluster-Upgrade)  
- [Backup and Restore Methods](#Backup-and-Restore-Methods)  

- **Cluster**
    - A Kubernetes cluster is a set of **node machines** for running containerized applications. If you’re running Kubernetes, you’re running a cluster.
    - At a minimum, **a cluster contains a control plane and one or more compute machines, or nodes.** The control plane is responsible for maintaining the desired state of the cluster, such as which applications are running and which container images they use. Nodes actually run the applications and workloads.
    - The cluster is the heart of Kubernetes’ key advantage: **the ability to schedule and run containers across a group of machines**, be they physical or virtual, on premises or in the cloud. **Kubernetes containers aren’t tied to individual machines. Rather, they’re abstracted across the cluster.**

## OS Upgrade
![cordon](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/drain.PNG)

- When you `drain` a node, the pods are gracefully **terminated** from the node that they're on and recreated on another.

- The node is also cordoned or marked as unschedulable,meaning no pods can be scheduled on this node until you specifically remove the restriction.

> **cordon**  
> prevent access to or from an area or building by surrounding it with police or other guards.


- `Cordon` simply marks a node unschedulable.
- Unlike drain, it **does not terminate** or move the pods on an existing node. It simply makes sure that new pods are not scheduled on that node.

### Practice
- How many applications do you see hosted on the cluster?

```console
~# kubectl get deploy
```

```
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   0/3     0            0           1s
```

- `NAME` lists the names of the Deployments in the namespace.
- `READY` displays how many replicas of the **application** are available to your users. It follows the pattern ready/desired.
- `UP-TO-DATE` displays the number of replicas that have been updated to achieve the desired state.
- `AVAILABLE` displays how many replicas of the **application** are available to your users.
- `AGE` displays the amount of time that the **application** has been running.

- Run the command **`kubectl get pods -o wide`** and get the list of nodes the pods are placed on

- Need to take `node01` out for maintenance

```console
~# kubectl drain node01 --ignore-daemonsets
```

- Configure the `node01` to be scheduled again

```console
~# k uncordon node01
```

- We do not want to schedule any more pods on `node01` and don't want current pods to be removed
```console
~# kubectl cordon node01 
```

## Cluster Upgrade

![version1](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/kgn.PNG)

![version2](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/mmp.PNG)

![k](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/up2.PNG)

- Since the `kube-apiserver` is the primary component in the control plane and that is the component that all other components talk to.

- None of the other components should ever be at a version higher than the `kube-apiserver`.

- Now this is not the case with `kubectl`, the kubectl utility could be at 1.11, a version higher than the `kube-apiserver`.

- **At any time, kubernetes supports only up to the recent 3 minor versions**

- Do we upgrade directly from 1.10 to 1.13?  NO. The recommended approach is to upgrade **one minor version at a time.**

<!--
kubeadm: 클러스터를 부트스트랩하는 명령이다.

부스트랩 : " 입력 장치에서 프로그램의 나머지 부분을 도입할 수 있도록 하는 몇 가지 초기 지침을 통해 프로그램을 컴퓨터에 로드하는 기술이다."

저희 환경에서 부트스트랩은 쿠버네티스 클러스터를 초기화하는데 진행하는 절차로 보시면 될 것 같습니다. 컨테이너로 서비스를 위한 환경을 구성하기 위해 클러스터가 구성될 수 있도록 init과 join을 수행해 컨테이너 실행환경을  준비합니다.

kubelet: 클러스터의 모든 머신에서 실행되는 파드와 컨테이너 시작과 같은 작업을 수행하는 컴포넌트이다.

kubectl: 클러스터와 통신하기 위한 커맨드 라인 유틸리티이다.
-->

- **kubeadm**: the command to bootstrap the cluster.

- **kubelet**: the component that runs on all of the machines in your cluster and does things like starting pods and containers.

- **kubectl**: the command line util to talk to your cluster.

- Upgrading a cluster involves two major steps.
    - First, you upgrade your master nodes
    - and then upgrade the worker nodes

### Practice

- Check version of the cluster
  <details>
  <summary>Answer</summary>

  ```console
  ~# k get nodes
    NAME           STATUS   ROLES           AGE   VERSION
    controlplane   Ready    control-plane   93m   v1.25.0
    node01         Ready    <none>          92m   v1.25.0
  ```
  </details>
  
- How many nodes are hosted on the cluster? Inspect the applications and taints set on the node

  <details>
  <summary>Answer</summary>

  ```console
  ~# k describe node | grep Taints
  Taints:      <none>
  Taints:      <none>
  ```
  </details>
  
- How many applications are hosted on the cluster?

  <details>
  <summary>Answer</summary>

  ```console
  ~#  k get deploy
    NAME   READY   UP-TO-DATE   AVAILABLE   AGE
    blue   5/5     5            5           9m
  ```

  </details>

- What nodes are the pods hosted on?

  <details>
  <summary>Answer</summary>

  ```console
  ~# kubectl get pods -o wide
    NAME                    READY   STATUS    RESTARTS   AGE   IP           NODE           NOMINATED NODE   READINESS GATES
    blue-5db6db69f7-5c5sf   1/1     Running   0          16m   10.244.0.5   controlplane   <none>           <none>
    blue-5db6db69f7-896np   1/1     Running   0          16m   10.244.1.3   node01         <none>           <none>
    blue-5db6db69f7-9vfwq   1/1     Running   0          16m   10.244.1.4   node01         <none>           <none>
    blue-5db6db69f7-pv4rc   1/1     Running   0          16m   10.244.1.2   node01         <none>           <none>
    blue-5db6db69f7-r59wh   1/1     Running   0          16m   10.244.0.4   controlplane   <none>           <none>

  ~#  k describe pod | grep Node:
    Node:             controlplane/192.24.71.12
    Node:             node01/192.24.71.3
    Node:             node01/192.24.71.3
    Node:             node01/192.24.71.3
    Node:             controlplane/192.24.71.12
  ```
  </details>

- What is the latest stable version of Kubernetes as of today?

  <details>
  <summary>Answer</summary>

  ```console
  ~# kubeadm upgrade plan 
  ```

  </details>
  

- Upgrade the controlplane components to exact version v1.26.0

    - Search K8s documentation with keyword `upgrade`
        - [Upgrade](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/)

    - Check system version (In this case - `Ubuntu`)

    ```console
    controlplane ~#  cat /etc/*release*
    DISTRIB_ID=Ubuntu
    DISTRIB_RELEASE=20.04
    DISTRIB_CODENAME=focal
    DISTRIB_DESCRIPTION="Ubuntu 20.04.5 LTS"
    NAME="Ubuntu"
    VERSION="20.04.5 LTS (Focal Fossa)"
    ID=ubuntu
    ID_LIKE=debian
    PRETTY_NAME="Ubuntu 20.04.5 LTS"
    VERSION_ID="20.04"
    HOME_URL="https://www.ubuntu.com/"
    SUPPORT_URL="https://help.ubuntu.com/"
    BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
    PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
    VERSION_CODENAME=focal
    UBUNTU_CODENAME=focal
    ```

    - Determine which version to upgrade to
    ```console
    apt update
    apt-cache madison kubeadm
    ```

    - Upgrading control plane
    ```
    # replace x in 1.26.x-00 with the latest patch version
    apt-mark unhold kubeadm && \
    apt-get update && apt-get install -y kubeadm=1.26.x-00 && \
    apt-mark hold kubeadm
    ```

    - Verify that the download works and has the expected version:
    ```console
    kubeadm version
    ```
    - Verify the upgrade plan:
    ```console
    kubeadm upgrade plan
    ```
    This command checks that your cluster can be upgraded, and fetches the versions you can upgrade to. It also shows a table with the component config version states.


    - Choose a version to upgrade to, and run the appropriate command. For example:
    ```console
    # replace x with the patch version you picked for this upgrade
    sudo kubeadm upgrade apply v1.26.x
    ```

    - Upgrade kubelet and kubectl
    ```
    # replace x in 1.26.x-00 with the latest patch version
    apt-mark unhold kubelet kubectl && \
    apt-get update && apt-get install -y kubelet=1.26.x-00 kubectl=1.26.x-00 && \
    apt-mark hold kubelet kubectl
    ```

    - Restart the kubelet:
    ```console
    sudo systemctl daemon-reload
    sudo systemctl restart kubelet
    ```

    - Uncordon the node
    ```console
    Bring the node back online by marking it schedulable:

    # replace <node-to-uncordon> with the name of your node
    kubectl uncordon <node-to-uncordon>
    ```    

- Upgrade worker node
    - **ssh worker node**
    
    ```console
    ~# k get nodes -o  wide
    NAME           STATUS                     ROLES           AGE    VERSION   INTERNAL-IP    EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION   CONTAINER-RUNTIME
    controlplane   Ready                      control-plane   123m   v1.25.0   192.24.222.3   <none>        Ubuntu 20.04.5 LTS   5.4.0-1102-gcp   containerd://1.6.6
    node01         Ready,SchedulingDisabled   <none>          122m   v1.25.0   192.24.222.6   <none>        Ubuntu 20.04.5 LTS   5.4.0-1102-gcp   containerd://1.6.6

    ~# ssh 192.24.222.6
    root@node01 ~#  
    ```

    - Type exit or logout or enter `CTRL + d` to go back to the controlplane node.

- [Upgrade worker node](https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/upgrading-linux-nodes)

<!--
This will upgrade Kubernetes controlplane node.

- kubeadm upgrade apply v1.26.0

This will update the kubelet with the version 1.26.0.

- apt-get install kubelet=1.26.0-00 

On the node01 node, run the following commands:
If you are on the controlplane node, run ssh node01 to log in to the node01.

This will update the package lists from the software repository.

- apt-get update

This will install the kubeadm version 1.26.0.

- apt-get install kubeadm=1.26.0-00

This will upgrade the node01 configuration.

- kubeadm upgrade node

This will update the kubelet with the version 1.26.0.

- apt-get install kubelet=1.26.0-00 

-->

## Backup and Restore Methods


- `ETCD cluster` : All cluster-related information is stored 

- `Persistent Volumes` : persistent storage

### Backup - ETCD

- **ETCD Cluster** : So information about the cluster itself, the nodes and every other resources created within the cluster, are stored here.

- the etcd cluster is hosted on **the master nodes**.

- **Data Directory**
  ![dd](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/be.PNG)
  - While configuring etcd, we specified a location where all the data would be stored, the data directory.
  - That is the directory that can be configured to be backed up by your backup tool.

- **Restore**
![res](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/er.PNG)

- [Restore ETCD cluster](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/)

- To restore etcd from the backup at later in time. First stop kube-apiserver service
  ```
  $ service kube-apiserver stop
  ```
- Run the etcdctl snapshot restore command
- Update the etcd service
- Reload system configs
  ```
  $ systemctl daemon-reload
  ```
- Restart etcd
  ```
  $ service etcd restart
  ```

- Start the kube-apiserver
  ```
  $ service kube-apiserver start
  ```

#### With all etcdctl commands specify the cert,key,cacert and endpoint for authentication.

```
$ ETCDCTL_API=3 etcdctl \
  snapshot save /tmp/snapshot.db \
  --endpoints=https://[127.0.0.1]:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/etcd-server.crt \
  --key=/etc/kubernetes/pki/etcd/etcd-server.key
```


- if you're using a managed Kubernetes environment, then, at times, you may not even access to the etcd cluster.
- In that case, backup by querying the Kube API server is probably the better way.

### Practice

- What is the version of ETCD running on the cluster?

  <details>
  <summary>Answer</summary>

  ```console
  ~# k get pods -n kube-system
  ~# k get pods -A -o wide 
  ~# k describe pod etcd-controlplane -n kube-system 
  ```
  - `kube-system`
    - The namespace for objects created by the Kubernetes system.
  </details>

- At what address can you reach the ETCD cluster from the controlplane node

  <details>
  <summary>Answer</summary>
  - Should check `--listen-client` part

  ```
  Command:
      etcd
      --listen-client-urls=https://127.0.0.1:2379,https://192.6.73.9:2379
  ```
  
  </details>


<!--
kube-system
쿠버네티스 시스템에 의해 생성되는 API 오브젝트들을 관리하기 위한 Namespace이다.
클러스터 레벨의 관리자 영역의 Namespace라고 이해하면 좋다. 
-->

- Take a snapshot of the ETCD database using the built-in snapshot functionality.

  <details>
  <summary>Answer</summary>
  
  - Check static pod
  
  ```
  $ ls /etc/kubernetes/manifests/
  etcd.yaml  kube-apiserver.yaml  kube-controller-manager.yaml  kube-scheduler.yaml

  $ ls /etc/kubernetes/pki/etcd/
  ca.crt  healthcheck-client.crt  peer.crt  server.crt
  ca.key  healthcheck-client.key  peer.key  server.key

  $ls /var/lib/etcd/
  member

  $ cat /etc/kubernetes/manifests/etcd.yaml 
  apiVersion: v1
  kind: Pod
  spec:
    containers:
    - command:
      - etcd
      - --cert-file=/etc/kubernetes/pki/etcd/server.crt
      - --listen-client-urls=https://127.0.0.1:2379,https://192.6.73.9:2379
      - --key-file=/etc/kubernetes/pki/etcd/server.key
      - --trusted-ca-file=/etc/kubernetes/pki/etcd/ca.crt
  ```

  - Snapshot 
    - Store the backup file at location `/opt/snapshot-pre-boot.db`

  ```
  $ export ETCDCTL_API=3
  $ etcdctl snapshot save --endpoints=127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key \
  /opt/snapshot-pre-boot.db
  ```
  
  </details>


- Luckily we took a backup. Restore the original state of the cluster using the backup file.

  <details>
  <summary>Answer</summary>
  
  ```
  $ etcdctl snapshot restore --data-dir /var/lib/etcd-from-backup /opt/snapshot-pre-boot.db
  2023-04-10 00:49:18.427304 I | mvcc: restore compact to 2840
  2023-04-10 00:49:18.434978 I | etcdserver/membership: added member 8e9e05c52164694d [http://localhost:2380] to cluster cdf818194e3a8c32
  ```

  - **Change the hostPath**

  ```
  $ vim /etc/kubernetes/manifests/etcd.yaml 
  ```

  ```yaml
  volumes:
  - hostPath:
      path: /etc/kubernetes/pki/etcd
      type: DirectoryOrCreate
    name: etcd-certs
  - hostPath:
      path: /var/lib/etcd-from-backup ## Change /var/lib/etcd to backup Path
      type: DirectoryOrCreate
    name: etcd-data
  ```


  </details>
