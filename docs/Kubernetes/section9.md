# Section9 - Networking

- [Prerequisite](#Prerequisite)  
- [Pod Networking](#Pod-Networking)  
- [CNI](#CNI)  
- [IP Address Management](#IPAM)  
- [Service Networking](#Service-Networking)  
- [DNS in Kubernetes](#DNS-in-Kubernetes)  
- [Ingress Networking](#Ingress)  


## Prerequisite

### Switching 
![switching](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/net14.PNG)

- To connect system A to B, we connect them to a **switch**
- To connect them to a switch, we need an interface on each host
- To see the interfaces for the host -> we use `ip link` command
    - In the above example, `eth0` is the interface

- Let's assume it's a network with the address 192.168.1.0. We then assign the systems with IP addresses on the same network.

- Once the links are up and the IP addresses are assigned, the computers can now communicate with each other through the **switch**.

- The switch can only enable communication **within a network**


### Routing
![routing](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/net15.PNG)

- How does a system in one network reach a system in the other?
- How does system B with the IP `192.168.1.11` reach system C with the IP `2.10` on the other network?
    - That's where a **router** comes in

- **A router helps connect two networks together.**

### Gateway
- When system B tries to send a packet to system C, how does it know where the router is on the network to send the packet through?
    - That's where we configure the systems with a **gateway**

- **default gateway**
    - Instead of adding a routing table entry for the same router's IP address for each of those networks, you can simply say for any network that you don't know a route to, use this router as the default gateway.
    - Instead of the word default, you could also say `0.0.0.0`. It means any IP destination.

<!--

라우터 : 네트워크주소가 다른 경우 서로 통신을 하도록 도와주는 장치

게이트웨이 : 네트워크 주소가 다른 네트워크를 연결할 때 반드시 거쳐가는 것 (기계, 장비가 아니라 인터넷 방향으로 나갈 때 찾아가야할 아이피 주소)

즉, 라우터는 장비, 게이트웨이는 장비가 아닌 개념적 의미, 통로, 출입구

-->

- `ip link`
    - IP link is to list interfaces on the host.

- `ip addr`
    - IP addr command is to see the IP addresses assigned to those interfaces.

- `ip addr add 192.168.10/24 dev eth0`
    - IP addr add command is used to set IP addresses on the interfaces.

- `ip route`
    - view the routing table

- `ip route add 192.168.1.0/24 via 192.168.2.1`
    - Add entries into the routing table    

### DNS

- **host**
    - Host means the element that can be communicated via Internet by configuring IP address

- **DNS Server** 
    - DNS : Domain Name System
    - All the host entries saved into a single server who will manage it centrally. We call that our DNS server.

```
https://www.google.com
mail.google.com
drive.google.com
```    

- **protocol**
    - `https`

- **subdomain**
    - `www`, `mail`, `drive` : A subdomain is what comes before the domain

- **domain name**
    - `google.com` : identifier that registered in domain registory to differentiate the computer system in network

- **hostname** is a network identifier. It is an attribute of a system stored locally on that system. "Computer name" is what Windows uses to refer to the hostname.

<!--
인터넷 망에서 호스트(host)라고 하면, IP주소를 설정함으로써 인터넷 망에 연결된, 통신 가능한 요소를 의미

호스트 이름(hostname)은 이런 호스트(host)에 부여된 이름(name)입니다. IP주소를 갖고 있는 '어떤 것'에 '이름(name)'을 부여한 것입니다. 이미 IP주소를 갖고 있는데 왜 이름을 더 부여할까요? IP주소는 사람이 일일이 기억하기 힘들기 때문에 기억하기 쉬운 이름을 부여하여 통신에 사용합니다.

호스트 이름은 도메인 이름의 한 가지 특수한 유형입니다. 도메인 이름 중에서 IP주소를 설정할 수 있는 이름이 호스트 이름입니다. 호스트 이름은 도메인 이름의 유형 중 일부분입니다.

a Domain name is the registered domain. ie:

example.com
dell.com

Hosts are addressable entities in a domain, eg:

ftp.example.com
mail.example.com
www.example.com


넷상의 존재하는 네트워크 장비들은 기본적으로 ip를 통해 통신을 하게 됩니다.

다만 이용자, 즉 우리 사람들은 모든 서버의 ip를 외울 순 없습니다.

이를 간단하게 외울 수 있도록 도와주는 것이 도메인(domain)입니다.

ip를 외우는 것 대신 도메인을 외움으로써 좀 더 간단하게 원하는 웹페이지, 필요한 서버 등을 찾을 수가 있습니다.

이 과정에서 ip와 도메인을 매칭해주는 역할을 하는 서버가 필요한데 이를 도와주는 것이

DNS(Domain Name Service) 입니다.

내가 구매한 도메인의 네임서버를 지정해주면 해당 네임서버에서 갖고 있는 dns의 레코드를 통해

이용자가 찾고자 하는 도메인을 ip로 변경하여 통신을 하도록 도와주는 것입니다.

그렇다면 원하는 서버와의 정확한 통신을 위해서는 도메인에 대해서 정확한 레코드 값을 지정함으로써

내가 원하는 서버와 통신이 되도록 만들어야 합니다.

DNS 레코드에는 여러 종류가 있습니다. SOA, A, AAAA, NS, MX, CNAME, PTR, TXT, SRV 등의 여러 종류가 있습니다.

CNAME(Canonical NAME): 별칭 레코드;

- 실제호스트명(A레코드)과 연결되는 별칭, 별명을 정의 해주는 레코드입니다.

-->

- So for every host name, the host first looks into the `/etc/hosts` file, and if it cannot find it there,it then looks at the `DNS server`.


### CoreDNS
- **CoreDNS is a flexible, extensible DNS server that can serve as the Kubernetes cluster DNS.**

### Network Namespaces

- **ARP**
    - Address Resolution Protocol (ARP) is a protocol or procedure that connects an ever-changing Internet Protocol (IP) address to a fixed physical machine address, also known as a media access control (MAC) address, in a local-area network (LAN).

<!--ARP는 IP 주소와 MAC 주소를 일대일 매칭 시켜 LAN에서 목적지를 찾아갈 수 있도록 하는 프로토콜-->

- Create Network Namespace

```
$ ip netns add red
$ ip netns add blue
```

- List the network namespace

```
$ ip netns
```

### Docker Networking

#### Host Network

- Running docker container with `host` network
- With the host network, the container is attached to the host network. There is no network isolation between the host and the container. If you deploy a web application listening on port 80 in the container, then the web application is available on port 80 on the host without having to do any additional port mapping.

```
$ docker run --network host nginx
```

#### Bridge Network

- Running docker container with `bridge` network
- **An internal private network is created which the Docker host and containers attach to.**

```
$ docker run --network bridge nginx
```

#### List the Docker Network

```
$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
4974cba36c8e        bridge              bridge              local
0e7b30a6c996        host                host                local
a4b19b17d2c5        none                null                local
```

### CNI

> **Container Network Interface**  
> The CNI is a set of standards that define how programs should be developed to solve networking challenges in a container runtime environments.



- Container Network Interface (CNI) is a framework for dynamically configuring networking resources. The plugin specification defines an interface for configuring the network, provisioning IP addresses, and maintaining connectivity with multiple hosts.

- When used with Kubernetes, CNI can integrate smoothly with the kubelet to enable the use of an overlay or underlay network to automatically configure the network between pods. 


### Cluster Networking

- The Kubernetes cluster consists of master and worker nodes. Each node must have at least one interface connected to a network.

#### IP and Hostname

- To view the hostname

```
$ hostname 
```

- To view the IP addr of the system

```
$ ip a
```


#### Set the hostname

```
$ hostnamectl set-hostname <host-name>
$ exec bash
```

#### View the Listening Ports of the system

```
$ netstat -nltp
```

### Practice


1. <details>
   <summary>What is the Internal IP address of the controlplane node in this cluster?</summary>

      ```
      kubectl get nodes -o wide
      ```

      Note the value in `INTERNAL-IP` column for `controlplane`

   </details>

1. <details>
   <summary>What is the network interface configured for cluster connectivity on the controlplane node?</summary>

   This will be the network interface that has the same IP address you determined in the previous question.

   ```
   ip a
   ```

   There is quite a lot of output for the above command. We can filter it better:

   ```
   ip a | grep -B2 X.X.X.X
   ```

   where `X.X.X.X` is the IP address you got from the previous question. `grep -B2` will find the line containing the value we are looking for and print that and the previous 2 line of output. It will look like this, though the values will be different each time you run the lab.

   ```
   3058: eth0@if3059: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue state UP group default
      link/ether 02:42:c0:08:ea:03 brd ff:ff:ff:ff:ff:ff link-netnsid 0
     inet 192.8.234.3/24 brd 192.8.234.255 scope global eth0
   ```

   From this, we can determine the answer to be

   > `eth0`
   </details>

1. <details>
   <summary>What is the MAC address of the interface on the controlplane node?</summary>

   This value is also present in the output of the command you ran for the previous question. The MAC address is the value in the `link/ether` field of the output and is 6 hex numbers separated by `:`. Note that the value can be different each time you run the lab.

   If the output for `eth0` is

   ```
   3058: eth0@if3059: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1450 qdisc noqueue state UP group default
      link/ether 02:42:c0:08:ea:03 brd ff:ff:ff:ff:ff:ff link-netnsid 0
     inet 192.8.234.3/24 brd 192.8.234.255 scope global eth0
   ```

   then the MAC address is

   > `02:42:c0:08:ea:03`
   </details>

1. <details>
   <summary>What is the IP address assigned to node01?</summary>

   ```
   kubectl get nodes -o wide
   ```

   Note the value in `INTERNAL-IP` column for `node01`

   </details>

1. <details>
   <summary>What is the MAC address assigned to node01?</summary>

   For this we will need to SSH onto `node01` so we can view its interfaces. We know what IP to look for, as we determined this in the previous question

   ```
   ssh node01
   ip a | grep -B2 X.X.X.X
   ```

   where `X.X.X.X` is the IP address you got from the previous question. Again, look at the `link/ether` field.

   We could guess that the correct interface on `node01` is also `eth0` and simply run

   ```
   ip link show eth0
   ```

   but it's best to be sure.

   Now return to `controlplane`

   ```
   exit
   ```

   </details>

1. <details>
   <summary>We use Containerd as our container runtime. What is the interface/bridge created by Containerd on this host?</summary>

   This is not immediately straight forward.

   ```
   ip address show type bridge
   ip link show
   ```

   Know that

   * Any interface with name beginning `eth` is a "physical" interface, and represents a network card attached to the host.
   * Interface `lo` is the loopback, and covers all IP addresses starting with `127`. Every computer has this.
   * Any interface with name beginning `veth` is a virtual network interface used for tunnelling between the host and the pod network. These connect with bridges, and the bridge interface name is listed with their details.

   We can see that for the two `veth` devices, they are associated with another device in the list `cni0`, therefore that is the answer.

   </details>

1. <details>
   <summary>What is the state of the interface cni0?</summary>

   You can see in the output of the previous command that the state field for `cni0` is
   
   ```
   ~# ip address show type bridge
   .......state UP group default ......
   ```
   
   > `UP`
   </details>

1. <details>
   <summary>If you were to ping google from the controlplane node, which route does it take?</summary>

   What is the IP address of the Default Gateway?

   Run

   ```
   ip route
   ip route show default
   ```

   and note the output

   </details>

1. <details>
   <summary>What is the port the kube-scheduler is listening on in the controlplane node?</summary>

   Use the [netstat](https://linux.die.net/man/8/netstat) command to look at network sockets used by programs running on the host. There's a lot of output, so we will filter by process name, i.e. `kube-scheduler`

   - The netstat command displays the contents of various network-related data structures for active connections. This netstat function shows the state of all configured interfaces.

   <!--
   netstat(network statistics)는 전송 제어 프로토콜, 라우팅 테이블, 수많은 네트워크 인터페이스(네트워크 인터페이스 컨트롤러 또는 소프트웨어 정의 네트워크 인터페이스), 네트워크 프로토콜 통계를 위한 네트워크 연결을 보여주는 명령 줄 도구이다. 네트워크의 문제를 찾아내고 성능 측정으로서 네트워크 상의 트래픽의 양을 결정하기 위해 사용된다.
   -->

   ```
   netstat -nplt | grep kube-scheduler
   ```

   <details>
   <summary>What the netstat options used mean</summary>

   * `-n` - Show IP addresses (don't try to resolve to host names)
   * `-p` - Show the process names (e.g. `kube-scheduler`)
   * `-l` - Include only _listening_ sockets
   * `-t` - Include only TCP sockets

   </details>

   Output:

   ```
   tcp        0      0 127.0.0.1:10259         0.0.0.0:*               LISTEN      3291/kube-scheduler
   ```

   We can see it's listening on localhost, port `10259`
   </details>

1. <details>
   <summary>Notice that ETCD is listening on two ports. Which of these have more client connections established?</summary>

   We use `netstat` with slightly different options and filter for `etcd`

   ```
   netstat -anp | grep etcd
   ```

   <details>
   <summary>What the netstat options used mean</summary>

   * `-a` - Include sockets in all states
   * `-n` - Show IP addresses (don't try to resolve to host names)
   * `-p` - Show the process names (e.g. `etcd`)

   </details>

   You can see that by far and away, the most used port is `2379`.

   </details>

1. Information

   That's because `2379` is the port of ETCD to which API server connects to There are multiple concurrent connections so that API server can process multiple etcd operations simultaneously.

    `2380` is only for etcd peer-to-peer connectivity when you have multiple controlplane nodes. In this case we don't.

## Pod Networking
![pod](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/net12.PNG)

> Every POD shoud have an IP Address  
> Every POD should be able to communicate with every other POD in the same node  
> Every POD should be able to communicate with every other POD on other nodes without NAT  


- *NAT* : Network Address Translation
    - to map multiple private addresses inside a local network to a public IP address before tranferring the information onto the internet.

## CNI 
> **Container Networking Interface**


- We performed a number of manual steps to get the environment ready with the bridge networks and routing tables. We then wrote a script that can be run for each container that performs the necessary steps required to connect each container to the network, and we executed the script manually.

- So how do we run the script automatically when a Pod is created on Kubernetes?
    - **CNI** tells Kubernetes that this is how you should call a script as soon as you create a container.
    - Whenever a container is created, the kubelet looks at the CNI configuration, passed as a command line argument when it was run, and identifies our script's name.

![cni](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/net1.PNG)

- Container Runtime must create network namespace
- Identify network the container must attach to
- Conatiner Runtime to invoke Network Plugin (bridge) when container is ADDed
- Container Runtime to invoke Network Plugin (bridge) when contianer is DELeted
- JSON format of the Network Configuration


## IP Address Management
> **WeaveWorks**(CNI)  


- Instead of our own custom script, we integrated the **Weave plugin**.

- So how do we deploy Weave on a Kubernetes cluster?
    - Weave and weave peers can be deployed as services or Daemons on each node in the cluster manually or if Kubernetes is set up already then an easier way to do that is to deploy it as pods in the cluster.


### Explore CNI - Practice

1. <details>
    <summary>Inspect the kubelet service and identify the container runtime value is set for Kubernetes.</summary>

    Check kubelet unit file

    ```bash
    systemctl cat kubelet
    ```

    Note from the output this line

    ```
    EnvironmentFile=-/var/lib/kubelet/kubeadm-flags.env
    ```

    Inspect this file

    ```bash
    cat /var/lib/kubelet/kubeadm-flags.env
    ```

    Answer can be found as value of `--container-runtime`

    > REMOTE
    </details>

2. <details>
    <summary>What is the path configured with all binaries of CNI supported plugins?</summary>

    This is the standard location for the installation of CNI plugins

    | `/opt/cni/bin`

    </details>

3. <details>
    <summary>Identify which of the below plugins is not available in the list of available CNI plugins on this host?</summary>

    ```bash
    ls -l /opt/cni/bin
    ```

    Find the option from the given answers not in the output opf the above

    > cisco
    </details>

4. <details>
    <summary>What is the CNI plugin configured to be used on this kubernetes cluster?</summary>

    From the available options, we need to recognise which of the four is not the name of a container networking provider. Of the three that are, only one of them is present in `/opt/cni/bin`

    > flannel
    Note that `bridge` is a mechanism for connecting networks together, and not a network _provider_.
    </details>

5. <details>
    <summary>What binary executable file will be run by kubelet after a container and its associated namespace are created.</summary>

    Following on from Q4...

    > flannel
    All the files in `/opt/cni/bin` are binary executables with tasks related to configuring network namespaces. After the network namespace is configured using the other programs, `flannel` implements the network.

    [This is a great article](https://tonylixu.medium.com/k8s-network-cni-introduction-b035d42ad68f) on what the programs in `/opt/cni/bin` are for.
    </details>

## IPAM

1. <details>
    <summary>How many Nodes are part of this cluster?</summary>

    ```bash
    kunbectl get nodes
    ```

    > 2
    </details>

2. <details>
    <summary>What is the Networking Solution used by this cluster?</summary>

    Two ways to do this:

    1.
        ```bash
        kubectl get pods -n kube-system
        ```

    1.
        ```bash
        ls -l /opt/cni/bin
        ```

    In both you see evidence of

    > weave
    </details>

3. <details>
    <summary>How many weave agents/peers are deployed in this cluster?</summary>

    ```bash
    kubectl get pods -n kube-system
    ```

    > 2
    </details>

4. <details>
    <summary>On which nodes are the weave peers present?</summary>

    ```bash
    kubectl get pods -n kube-system -o wide
    ```

    > One on every node
    </details>

5. <details>
    <summary>Identify the name of the bridge network/interface created by weave on each node.</summary>

    At either host...

    ```bash
    ip addr list
    ```

    > weave
    In actual fact, the network interface is `weave` and the bridge is implemented by `vethwe-datapath@vethwe-bridge` and `vethwe-bridge@vethwe-datapath`

    </details>

6. <details>
    <summary>What is the POD IP address range configured by weave?</summary>

    Examine output of previous connad for `weave` interface. Note its IP begins with `10.`, so...

    > 10.X.X.X
    </details>

7. <details>
    <summary>What is the default gateway configured on the PODs scheduled on node01?</summary>

    Now we can deduce this from the naswer to the previous question. Since we know weave's IP range, its gateway must be on the same network. However we can verify that by starting a pod which is known to contain the `ip` tool.

    Remember this [container image](https://github.com/wbitt/Network-MultiTool). It is extremely useful for debugging cluster networking issues!

    ```bash
    kubectl run testpod --image=wbitt/network-multitool
    ```

    Wait for it to be running.

    ```bash
    kubectl exec -it testpod -- ip route
    ```

    Note the first line of the output. This is the answer.
    </details>

## Service Networking

### Practice

1. <details>
   <summary>What network range are the nodes in the cluster part of?</summary>

   ```
   kubectl get nodes -o wide
   ```

   Note the INTERNAL-IP column to derive:

   ```
   192.20.116.0/24
   ```
   </details>

2. <details>
   <summary>What is the range of IP addresses configured for PODs on this cluster?</summary>

   ```
   kubectl get pods -A -o wide
   ```

   From this list, exclude the static control plane pods like `kube-apiserver` as these run on the host network, not the pod network. From the remaining pods we can derive:

   ```
   10.244.0.0/16
   ```
   </details>

3. <details>
   <summary>What is the IP Range configured for the services within the cluster?</summary>

   ```
   kubectl get service -A
   ```

   Note the CLUSTER-IP column to derive:

   ```
   10.96.0.0/12
   ```
   </details>

4. <details>
   <summary>How many kube-proxy pods are deployed in this cluster?</summary>

   ```
   kubectl get pod -n kube-system | grep kube-proxy
   ```

   Count the results
   </details>

5. <details>
   <summary>What type of proxy is the kube-proxy configured to use?</summary>

   From the output of the above question, you have two kube-proxy pods, e.g.

   ```
   controlplane ~ ➜  kubectl get pod -n kube-system | grep kube-proxy
   kube-proxy-rtr8p                       1/1     Running   0             56m
   kube-proxy-t7w8f                       1/1     Running   0             56m
   ```

   Pick either and check its logs. The answer is there.

   ```
   k logs -n kube-system kube-proxy-rtr8p
   ```
   </details>

6. <details>
   <summary>How does this Kubernetes cluster ensure that a kube-proxy pod runs on all nodes in the cluster?</summary>

   ```
   kubectl get all -n kube-system
   ```

   From this, you can see that `kube-proxy` is a `daemonset`
   </details>



## DNS in Kubernetes

### Pod DNS Record

- The following DNS resolution:

```
<POD-IP-ADDRESS>.<namespace-name>.pod.cluster.local
```
> Example
```
# Pod is located in a default namespace
10-244-1-10.default.pod.cluster.local
```
```
# To create a namespace
$ kubectl create ns apps
# To create a Pod
$ kubectl run nginx --image=nginx --namespace apps
# To get the additional information of the Pod in the namespace "apps"
$ kubectl get po -n apps -owide
NAME    READY   STATUS    RESTARTS   AGE   IP           NODE     NOMINATED NODE   READINESS GATES
nginx   1/1     Running   0          99s   10.244.1.3   node01   <none>           <none>
# To get the dns record of the nginx Pod from the default namespace
$ kubectl run -it test --image=busybox:1.28 --rm --restart=Never -- nslookup 10-244-1-3.apps.pod.cluster.local
Server:    10.96.0.10
Address 1: 10.96.0.10 kube-dns.kube-system.svc.cluster.local
Name:      10-244-1-3.apps.pod.cluster.local
Address 1: 10.244.1.3
pod "test" deleted
# Accessing with curl command
$ kubectl run -it nginx-test --image=nginx --rm --restart=Never -- curl -Is http://10-244-1-3.apps.pod.cluster.local
HTTP/1.1 200 OK
Server: nginx/1.19.2
```

### Service DNS Record

- The following DNS resolution:

```
<service-name>.<namespace-name>.svc.cluster.local
```
> Example
```
# Service is located in a default namespace
web-service.default.svc.cluster.local
```
- Pod, Service is located in the `apps` namespace
```
# Expose the nginx Pod
$ kubectl expose pod nginx --name=nginx-service --port 80 --namespace apps
service/nginx-service exposed
# Get the nginx-service in the namespace "apps"
$ kubectl get svc -n apps
NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
nginx-service   ClusterIP   10.96.120.174   <none>        80/TCP    6s
# To get the dns record of the nginx-service from the default namespace
$ kubectl run -it test --image=busybox:1.28 --rm --restart=Never -- nslookup nginx-service.apps.svc.cluster.local
Server:    10.96.0.10
Address 1: 10.96.0.10 kube-dns.kube-system.svc.cluster.local
Name:      nginx-service.apps.svc.cluster.local
Address 1: 10.96.120.174 nginx-service.apps.svc.cluster.local
pod "test" deleted
# Accessing with curl command
$ kubectl run -it nginx-test --image=nginx --rm --restart=Never -- curl -Is http://nginx-service.apps.svc.cluster.local
HTTP/1.1 200 OK
Server: nginx/1.19.2
```

- Kubernetes deploys a DNS server within the cluster.
    - The recommended DNS server is CoreDNS
    - CoreDNS server is deployed in the K8s Cluster in the Kube system namespace in the K8s cluster

### To view the configmap of CoreDNS

```
$ kubectl get configmap -n kube-system
NAME                                 DATA   AGE
coredns                              1      52m
```

### CoreDNS Configuration File

```
$ kubectl describe cm coredns -n kube-system
Corefile:
---
.:53 {
    errors
    health {       lameduck 5s
    }
    ready
    kubernetes cluster.local in-addr.arpa ip6.arpa {
       pods insecure
       fallthrough in-addr.arpa ip6.arpa
       ttl 30
    }
    prometheus :9153
    forward . /etc/resolv.conf
    cache 30
    loop
    reload
}
```

### Practice

- Identify the DNS solution implemented in this cluster.

  <details>
  <summary>Answer</summary>

    ```console
    ~# k get pods -n kube-system 
    NAME                                   READY   STATUS    RESTARTS   AGE
    coredns-787d4945fb-7qvg8               1/1     Running   0          6m19s
    coredns-787d4945fb-pj7rh               1/1     Running   0          6m19s
    etcd-controlplane                      1/1     Running   0          6m35s
    kube-apiserver-controlplane            1/1     Running   0          6m33s
    kube-controller-manager-controlplane   1/1     Running   0          6m30s
    kube-proxy-gv5j5                       1/1     Running   0          6m20s
    kube-scheduler-controlplane            1/1     Running   0          6m34s
    ```

  </details>

- What is the name of the service created for accessing CoreDNS?

  <details>
  <summary>Answer</summary>

  ```console
  ~# k get svc -n kube-system 
  ```

  </details>

- Where is the configuration file located for configuring the CoreDNS service?

  <details>
  <summary>Answer</summary>

    - It's in containers' `Args > -conf`
        - Under Arguments > Configfile field
        - /etc/coredns/Corefile 

    ```console
    ~# k describe pod coredns-787d4945fb-7qvg8 -n kube-system 
    Name:                 coredns-787d4945fb-7qvg8
    Namespace:            kube-system
    Priority:             2000000000
    Priority Class Name:  system-cluster-critical
    Service Account:      coredns
    Node:                 controlplane/192.27.133.3
    Start Time:           Wed, 19 Apr 2023 10:29:16 -0400
    Labels:               k8s-app=kube-dns
                        pod-template-hash=787d4945fb
    Annotations:          <none>
    Status:               Running
    IP:                   10.244.0.2
    IPs:
    IP:           10.244.0.2
    Controlled By:  ReplicaSet/coredns-787d4945fb
    Containers:
    coredns:
        Container ID:  containerd://ec55f5f1815e630f86fa0bdb4dbde3ee21515ccb838b13da49c5f5a7c971ec35
        Image:         registry.k8s.io/coredns/coredns:v1.9.3
        Image ID:      registry.k8s.io/coredns/coredns@sha256:8e352a029d304ca7431c6507b56800636c321cb52289686a581ab70aaa8a2e2a
        Ports:         53/UDP, 53/TCP, 9153/TCP
        Host Ports:    0/UDP, 0/TCP, 0/TCP
        Args:
          -conf
          /etc/coredns/Corefile ## config File!
    ```

  </details>

- What is the name of the ConfigMap object created for Corefile?

  <details>
  <summary>Answer</summary>
  
  - To see the detail info of CoreDNS POD, check the yaml file 
  
  ```console
  ~# k get pod coredns -n kube-system -o yaml
  ```

  </details>

- What is the root domain/zone configured for this kubernetes cluster?

  <details>
  <summary>Answer</summary>

  ```console
  ~# k get configmap -n kube-system
  ~# k descrive configmap [coredns NAME] -n kube-system
  ```

  </details>


## Ingress

> An API object that manages external access to the services in a cluster, typically HTTP.  
> Ingress may provide load balancing, SSL termination and name-based virtual hosting.  


- https://kubernetes.io/docs/concepts/services-networking/ingress/

- if you were on a public cloud environment like Google Cloud platform. In that case, instead of creating a service of type node port for your wear application, you could set it to type **load balancer**.

- Your company's business grows and you now have new services for your customers.
    - You'd like to make your old application accessible my-online-store.com/wear. Your developers developed the new video streaming application as a completely different application as it has nothing to do with the existing one.
    - However, to share the cluster's resources, you deploy the new application as a separate deployment within the same cluster.
    - So how do you direct traffic between each of these load balancers based on the URL that the user types in?
    - You need yet another proxy or load balancer that can redirect traffic based on URLs to the different services.
    - You need to configure your firewall rules for each new service, and it's expensive as well as for each service, a new cloud-native load balancer needs to be provisioned.

- Wouldn't it be nice if you could manage all of that within the Kubernetes cluster and have all that configuration as just another Kubernetes definition file that lives along with the rest of your application deployment files?
    - **Ingress** helps your users access your application using a single externally accessible URL that you can configure to route traffic to different services within your cluster based on the URL path.


### Ingress controller

### ConfigMap

```yaml
kind: ConfigMap
apiVersion: v1
metadata:
  name: nginx-configuration
```

### Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ingress-controller
spec:
  replicas: 1
  selector:
    matchLabels:
      name: nginx-ingress
  template:
    metadata:
      labels:
        name: nginx-ingress
    spec:
      serviceAccountName: ingress-serviceaccount
      containers:
        - name: nginx-ingress-controller
          image: quay.io/kubernetes-ingress-controller/nginx-ingress-controller:0.21.0
          args:
            - /nginx-ingress-controller
            - --configmap=$(POD_NAMESPACE)/nginx-configuration ## ConfigMap
          env:
            - name: POD_NAME ## POD Name
              valueFrom:
                fieldRef:
                  fieldPath: metadata.name
            - name: POD_NAMESPACE
              valueFrom:
                fieldRef:
                  fieldPath: metadata.namespace
          ports:
            - name: http
              containerPort: 80
            - name: https
              containerPort: 443
```

### Service Type - NodePort
> We then need a service to expose the Ingress controller to the external world, so we create a service of type **NodePort** with the NGINX Ingress label selector to link the service to the deployment.

```yaml
# service-Nodeport.yaml
apiVersion: v1
kind: Service
metadata:
  name: ingress
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 80
    protocol: TCP
    name: http
  - port: 443
    targetPort: 443
    protocol: TCP
    name: https
  selector:
    name: nginx-ingress
```

- Create a service

```
$ kubectl create -f service-Nodeport.yaml
```

- To get the service

```
$ kubectl get service
```

### Ingress Resources

> An **Ingress resource** is a set of rules and configurations applied on the Ingress controller.

```yaml
Ingress-wear.yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: ingress-wear
spec:
     backend:
        serviceName: wear-service
        servicePort: 80
```

- To create the ingress resource

```
$ kubectl create -f Ingress-wear.yaml
ingress.extensions/ingress-wear created
```

- To get the ingress
    - The new Ingress is now created and routes all incoming traffic directly to the wear service.

```
$ kubectl get ingress
NAME           CLASS    HOSTS   ADDRESS   PORTS   AGE
ingress-wear   <none>   *                 80      18s
```

### Ingress Resources - Rules
 
- 1 Rule and 2 Paths.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: ingress-wear-watch
spec:
  rules:
  - http:
      paths:
      - path: /wear
        backend:
          serviceName: wear-service
          servicePort: 80
      - path: /watch
        backend:
          serviceName: watch-service
          servicePort: 80
```

- Describe the earlier created ingress resource

```console
$ kubectl describe ingress ingress-wear-watch
Name:             ingress-wear-watch
Namespace:        default
Address:
Default backend:  default-http-backend:80 (<none>)
Rules:
  Host        Path  Backends
  ----        ----  --------
  *
              /wear    wear-service:80 (<none>)
              /watch   watch-service:80 (<none>)
Annotations:  <none>
Events:
  Type    Reason  Age   From                      Message
  ----    ------  ----  ----                      -------
  Normal  CREATE  23s   nginx-ingress-controller  Ingress default/ingress-wear-watch
```

- 2 Rules and 1 Path each.
    - The host field in each rule matches the specified value with the domain name used in the request URL and routes traffic to the appropriate backend.

```yaml
# Ingress-wear-watch.yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: ingress-wear-watch
spec:
  rules:
  - host: wear.my-online-store.com
    http:
      paths:
      - backend:
          serviceName: wear-service
          servicePort: 80
  - host: watch.my-online-store.com
    http:
      paths:
      - backend:
          serviceName: watch-service
          servicePort: 80
```

### Practice

- Which namespace is the Ingress Controller deployed in?

  <details>
  <summary>Answer</summary>

  ```console
  ~# k get pods -A
  ```

  </details>

- What is the name of the ingress Controller Deployment?
    
  <details>
  <summary>Answer</summary>

  ```console
  ~# k get deploy -n ingress-nginx
  ```

  </details>

- Which namespace is the Ingress Resource deployed in?

  <details>
  <summary>Answer</summary>

  ```console
  ~# k get ingress -A
  NAMESPACE   NAME                 CLASS    HOSTS   ADDRESS         PORTS   AGE
  app-space   ingress-wear-watch   <none>   *       10.105.216.56   80      12m
  ```

  </details>

- What is the Host configured on the Ingress Resource?

  <details>
  <summary>Answer</summary>

  - All Hosts (*)

  ```console
  ~# k get ingress -A
  NAMESPACE   NAME                 CLASS    HOSTS   ADDRESS         PORTS   AGE
  app-space   ingress-wear-watch   <none>   *       10.105.216.56   80      12m
  ```

  </details>

- You are requested to add a new path to your ingress to make the food delivery application available to your customers. Make the new application available at `/eat`.

    <details>
    <summary>Answer</summary>

    Run the command `kubectl edit ingress --namespace app-space` and add a new Path entry for the new service.

    OR

    ```yaml
    apiVersion: v1
    items:
    - apiVersion: extensions/v1beta1
        kind: Ingress
        metadata:
        annotations: ## annotations 
            nginx.ingress.kubernetes.io/rewrite-target: /
            nginx.ingress.kubernetes.io/ssl-redirect: "false"
        name: ingress-wear-watch
        namespace: app-space
        spec:
        rules:
        - http:
            paths:
            - backend:
                serviceName: wear-service
                servicePort: 8080
                path: /wear
                pathType: ImplementationSpecific
            - backend:
                serviceName: video-service
                servicePort: 8080
                path: /stream
                pathType: ImplementationSpecific
            - backend:
                serviceName: food-service
                servicePort: 8080
                path: /eat
                pathType: ImplementationSpecific
        status:
        loadBalancer:
            ingress:
            - {}
    kind: List
    metadata:
        resourceVersion: ""
        selfLink: ""
    ```
    </details>

- You are requested to make the new application available at `/pay`.

  <details>
  <summary>Answer</summary>

    ```console
    ~# k create ingress -h
    Create an ingress with the specified name.

    Aliases:
    ingress, ing

    Examples:
    # Create a single ingress called 'simple' that directs requests to foo.com/bar to svc
    # svc1:8080 with a tls secret "my-cert"
    kubectl create ingress simple --rule="foo.com/bar=svc1:8080,tls=my-cert"
    ```

    ```yaml
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
    name: test-ingress
    namespace: critical-space
    annotations:
        nginx.ingress.kubernetes.io/rewrite-target: /
        nginx.ingress.kubernetes.io/ssl-redirect: "false"
    spec:
    rules:
    - http:
        paths:
        - path: /pay
            pathType: Prefix
            backend:
            service:
                name: pay-service
                port:
                number: 8282 
    ```

  </details>

### Practice 2

1. Let us now deploy an Ingress Controller. First, create a namespace called `ingress-nginx`.

    <details>
    <summary>Answer</summary>

    ```
    kubectl create namespace ingress-space
    ```
    </details>

2. The NGINX Ingress Controller requires a ConfigMap object. Create a ConfigMap object with name `ingress-nginx-controller` in the `ingress-nginx` namespace..

    <details>
    <summary>Answer</summary>

    ```
    kubectl create configmap nginx-configuration --namespace ingress-space
    ```
    </details>

3. The NGINX Ingress Controller requires two ServiceAccounts. Create both ServiceAccount with name `ingress-nginx` and `ingress-nginx-admission` in the `ingress-nginx` namespace.

    <details>
    <summary>Answer</summary>

    ```
    kubectl create serviceaccount ingress-serviceaccount --namespace ingress-space
    ```
    </details>

4. We have created the Roles and RoleBindings for the ServiceAccount. Check it out!!

    <details>
    <summary>Answer</summary>

    ```
    kubectl get roles,rolebindings --namespace ingress-space
    ```
    </details>

5. Let us now create a service to make Ingress available to external users

    <details>
    <summary>Answer</summary>

    ```console
    ~# k expose deploy ingress-controller -n ingress-space --name ingress --port=80 --target-port=80 --type NodePort
    ```

    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
    name: ingress
    namespace: ingress-space
    spec:
    type: NodePort
    ports:
    - port: 80
        targetPort: 80
        protocol: TCP
        nodePort: 30080
        name: http
    - port: 443
        targetPort: 443
        protocol: TCP
        name: https
    selector:
        name: nginx-ingress
    ```
    </details>

7. Create the ingress resource to make application available at `/wear` and `/watch` on the Ingress service

    <details>
    <summary>Answer</summary>
    
    ```console
    ~# k create ingress -h
    ~# k create ingress ingress-wear-watch -n app-space --rule="..." --rule="..."
    ```

    ```yaml
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
    name: ingress-wear-watch
    namespace: app-space
    annotations:
        nginx.ingress.kubernetes.io/rewrite-target: /
        nginx.ingress.kubernetes.io/ssl-redirect: "false"
    spec:
    rules:
    - http:
        paths:
        - path: /wear
            pathType: Prefix
            backend:
            service:
                name: wear-service
                port: 
                number: 8080
        - path: /watch
            pathType: Prefix
            backend:
            service:
                name: video-service
                port:
                number: 8080
    ```

    </details>
