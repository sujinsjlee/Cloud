# Section9 - Networking

- [Prerequisite](#Prerequisite)  

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

<!--

  <details>
  <summary>Answer</summary>

  ```console
  ~# 
  ```

  </details>

-->