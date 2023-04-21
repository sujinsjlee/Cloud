# Section11 - Install Kubernetes the kubeadm

### Kubeadm

- The kubeadm tool helps us set up a multi-node cluster

- we'll have to install the kubeadm tool on all the nodes. So the kubeadm tool helps us bootstrap the Kubernetes solution by installing all of the necessary components on the right nodes in the right order.

### POD Network
- Kubernetes requires a special networking solution between the master and worker nodes called the POD Network.

- And so once the POD Network is set up, we are all set to go on having the worker nodes join the master node.

### Practice

1. Install the kubeadm, kubelet and kubectl packages at exact version 1.24.0 on the controlplane and node01.

    <details>
    <summary>Answer</summary>

    Run the following two steps on both `controlplane` and `node01` (use `ssh node01` to get to the worker node).

    1. Configure kernel parameters

        ```
        cat <<EOF | tee /etc/modules-load.d/k8s.conf
        br_netfilter
        EOF
        cat <<EOF | tee /etc/sysctl.d/k8s.conf
        net.bridge.bridge-nf-call-ip6tables = 1
        net.bridge.bridge-nf-call-iptables = 1
        net.ipv4.ip_forward = 1
        EOF
        sysctl --system
        ```

    2. Install kubernetes binaries

        ```
        apt-get update
        apt-get install -y apt-transport-https ca-certificates curl
        curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
        echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | tee /etc/apt/sources.list.d/kubernetes.list
        apt-get update
        apt-get install -y kubelet=1.24.0-00 kubeadm=1.24.0-00 kubectl=1.24.0-00
        apt-mark hold kubelet kubeadm kubectl
        ```
    </details>

1. What is the version of kubelet installed?

    <details>
    <summary>Answer</summary>

    ```
    kubelet --version
    ```
    </details>

1. How many nodes are part of kubernetes cluster currently?

    <details>
    <summary>Answer</summary>

    Are you able to run `kubectl get nodes`?

    Know that the kubeconfig file installed by kubeadm is located in `/etc/kubernetes/admin.conf`

    ```
    kubectl get nodes --kubeconfig /etc/kubernetes/admin.conf
    ```

    > 0
    </details>

1. Information only

1. Initialize controlplane node

    <details>
    <summary>Answer</summary>

    1. Get the IP address of the `eth0` adapter of the controlplane

        ```
        ifconfig eth0
        ```

        Take the value printed for `inet` in the output. This will be something like the following, but can be different each time you run the lab.

        > 10.13.26.9
    1. Run `kubeadm init` using the IP address determined above for `--apiserver-advertise-address`

        ```
        kubeadm init \
        --apiserver-cert-extra-sans=controlplane \
        --apiserver-advertise-address 10.13.26.9 \
        --pod-network-cidr=10.244.0.0/16
        ```

    1. Set up the default kubeconfig file

        ```
        mkdir ~/.kube
        cp /etc/kubernetes/admin.conf ~/.kube/config
        ```

    </details>

1. Generate a kubeadm join token

    <details>
    <summary>Answer</summary>

    You can copy the join command output by `kubeadm init` which looks like

    ```
    kubeadm join 10.13.26.9:6443 --token cpwmot.ldhadf3cokvyyx60 \
    --discovery-token-ca-cert-hash sha256:ea3a622922315b14b289c6efd7b1a77cbf81d29f6ddaf03472c304b6d3228c06
    ```

    Note it will be different each time you do the lab.

    </details>

1. Join node01 to the cluster using the join token

    <details>
    <summary>Answer</summary>

    1. `ssh` onto `node01` and paste the join command from above
    1. Return to the controlplane node
    1. Run `kubectl get nodes`. Note that both nodes are `NotReady`. This is OK because we have not yet installed networking.

    </details>

1. Install a Network Plugin

    <details>
    <summary>Answer</summary>

    1. Install flannel

        ```
        kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
        ```

    2. Wait 30 seconds or so, then run `kubectl get nodes`. Nodes should now be ready.

    </details>

