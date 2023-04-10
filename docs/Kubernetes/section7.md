# Section 7 - Security

- [Authentification](#Authentification)  
- [TLS](#TLS)  


## Authentification
- What are the risks and what measures do you need to take to secure the cluster?

![secure](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/seck.PNG)
- the `kube-apiserver` is at the center of all operations within Kubernetes. We interact with it through the kubectl utility or by accessing the API directly, and through that you can perform almost any operation on the cluster.
- So that's the first line of defense. Controlling access to the API server itself.

### TLS Certificates
![tls](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/tls.PNG)

- All communication with the cluster, between the various components such as the ETCD Cluster, kube-controller-manager, scheduler, api server, as well as those running on the working nodes such as the kubelet and kubeproxy is secured using TLS encryption.

### Network Policies
![np](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/np.PNG)
- communication between applications within the cluster

### Auth Mechanism
- Our focus is on users' access to the Kubernetes cluster for administrative purposes so we are left with two types of users: 
    - humans such as the administrators
    - and developers and robots such as other processes, or services, or applications that require access to the cluster

![acc](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/acc3.PNG)
- All user access is managed by the **API server**


## TLS
> **Goals**  
> What are TLS Certificates?  
> How does K8s use Certificates?  
> How to generate them?   
> How to configure them?  
> How to view them?  
> How to troubleshoot issues related to Certificates  



