# Section10 - Design and Install a Kubernetes Cluster

### Installing kubeadm, kubelet and kubectl
You will install these packages on all of your machines:

- `kubeadm`: the command to bootstrap the cluster.

- `kubelet`: the component that runs on all of the machines in your cluster and does things like starting pods and containers.

- `kubectl`: the command line util to talk to your cluster.

kubeadm will not install or manage kubelet or kubectl for you, so you will need to ensure they match the version of the Kubernetes control plane you want kubeadm to install for you.

### Cloud or OnPrem?
- Use  Kubeadm for on-Prem
- GKE for GCP
- Kops for AWS
- Azure Kubernetes Service (AKS) for Azure

### Storage
- High Performance : rely on SSD Backend Storage
- Multiple Concurrent connections : Network based storage
- Persistent shared volumes for shared access across multiple PODs
- Label nodes with specific disk types
- Use Node Selectors to assign applications to nodes with specific disk types

### ETCD
> ETCD is a distributed reliable key-valie store that is Simple, Secure & Fast


- Etcd is a consistent and highly-available key value store used as Kubernetes' backing store for all cluster data.

- Etcd is part of the Kubernetes master nodes.

- the other is where Etcd is separated from the control plane nodes and run on its own set of servers.

- etcd implements distributed consensus using **RAFT** protocol.
<!-- raft protocol이란
raft 는 etcd 에서 채택하고 있는 분산합의 알고리즘이다. 분산합의 알고리즘은 노드들이 서로 상태를 교환하고, 동기화하는 일련의 과정을 의미한다.-->
    
    - Raft is a protocol with which a cluster of nodes can maintain a replicated state machine. The state machine is kept in sync through the use of a replicated log.

### HA
- Consider multiple master nodes in a high availability configuration in the production environment.

