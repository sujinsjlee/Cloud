# Section 7 - Security

- [Authentification](#Authentification)  
- [TLS](#TLS)  
- [View Certificates](#View-Certificates)  
- [Certificates API](#Certificates-API)  
- [KubeConfig](#KubeConfig)  
- [API Groups](#API-Groups)  
- [Authorization](#Authorization)  
- [Role Based Access Controls](#Role-Based-Access-Controls)  
- [Cluster Roles and Role Bindings](#Cluster-Roles-and-Role-Bindings)  
- [Service Accounts](#Service-Accounts)  
- [Image Security](#Image-Security)  
- [Security Contexts](#Security-Contexts)  
- [Network Policy](#Network-Policy)  

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

[Network Protocol](https://www.cloudflare.com/ko-kr/learning/network-layer/what-is-a-protocol/)  

> **Goals**  
> What are TLS Certificates?  
> How does K8s use Certificates?  
> How to generate them?   
> How to configure them?  
> How to view them?  
> How to troubleshoot issues related to Certificates  

- **CA**(Certificate Authorities) <!--인증기관-->
- **CSR**(Certificate Signing Request) <!--인증서발급요청-->
- **PKI**(Public Key Infrastructure)
- **PEM**(Privacy Enhanced Mail) files are a type of PKI file used for keys and certificates
- **CN**(Common Name)

- Symmetric Encryption
    - It is a secure way of encryption, but it uses the same key to encrypt and decrypt the data and the key has to be exchanged between the sender and the receiver, there is a risk of a hacker gaining access to the key and decrypting the data.

- Asymmetric Encryption
    - Instead of using single key to encrypt and decrypt data, asymmetric encryption uses a pair of keys, **a private key and a public key**.

![pub](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/cert3.PNG)

- In the lecture, public lock refers to public key

- Generate public and private key pairs by running the SSH keys and command

```
$ ssh-keygen
id_rsa id_rsa.pub
```

- `id_rsa` is a private key
- `id_rsa.pub` is the public key (This public key works as a Locker of the server)

- You can create copies of your public lock and place them on as many servers as you want. 
- You can use the same private key to SSH into all of your servers securely.
- What if other users need access to your servers?
    - They can generate their own public and private key pairs, and can access the servers using their private keys

### CA
> Validate legitimacy of the certificate


- They're well known organizations that can sign and validate your certificates for you.

![csr](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/cert8.PNG)

- Step
    - You  generate a certificate signing a request or CSR using the key you generated earlier with open SSL command
    - This generates a My-Bank.CSR file, which is the certificate signing request that should be sent to the CA for signing.
    - The CAs use different techniques to make sure that you are the actual owner of that domain.

### SUMMARY  

- We use asymmetric encryption with a pair of public and private keys
- An admin uses a pair of keys to secure SSH connectivity to the servers
- The server sends a certificate signing request to a CA
- The signed certificate is then sent back to the server.
- The server configures the web application with the signed certificate.
- Whenever a user accesses the web application, the server first sends the certificate with its public key.
- The User uses the **CA's public key** to validate and retrieve the server's public key
- It then generates a symmetric key that wishes to use going forward for all communication.
- The symmetric key is encrypted using **the server's public key** and sent back to the server.
- The server uses its private key to decrypt the recieved message and retrieve the symmetric key.
- The end user though only generates a single symmetric key.
- so what can the server do to validate that the client is who they say they are?
    - client also must generate a pair of keys and a signed certificate from a valid CA

### Naming convention for public key and private key
![k](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/cert11.PNG)

- Just remember private keys have the word **`key`** in them usually, either as an extension or in the name of the certificate.

### Securing K8s cluster with TLS certificates
- There are three types of Certificates
    - Server certificates : configured on the server
    - Root Certificates : configured on the CA servers
    - Client Certificates : configured on the clients

- The different components within the k8s cluster and identify the various servers and clients and who talks to whom.
![kkk](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/certs.PNG)

### How to generate the certificates for the cluster

#### Certificate Authority (CA)
- [openssl can manually generate certificates for your cluster](https://kubernetes.io/docs/tasks/administer-cluster/certificates/#openssl)

- Generate **Keys**
  ```
  $ openssl genrsa -out ca.key 2048
  ```
- Generate **CSR**
  ```
  $ openssl req -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr
  ```
- Sign certificates
  ```
  $ openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt
  ```

#### Generating Client Certificates

#### Admin User Certificates

- Generate Keys
  ```
  $ openssl genrsa -out admin.key 2048
  ```
- Generate CSR
  ```
  $ openssl req -new -key admin.key -subj "/CN=kube-admin" -out admin.csr
  ```
- Sign certificates (Generate Signed Certificates)
  ```
  $ openssl x509 -req -in admin.csr -CA ca.crt -CAkey ca.key -out admin.crt
  ```
  
- Certificate with admin privilages
  ```
  $ openssl req -new -key admin.key -subj "/CN=kube-admin/O=system:masters" -out admin.csr
  ```

## View Certificates


- To view the details of the certificate

![view](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/hrd2.PNG)

```
$ openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text -noout
```

### Practice

- Identify the certificate file used for the kube-api server.

  <details>
  <summary>Answer</summary>

  - Run the command `cat /etc/kubernetes/manifests/kube-apiserver.yaml` and look for the line `--tls-cert-file.`

    ```console
    /etc/kubernetes/manifests $  ls -al
    total 28
    drwxr-xr-x 1 root root 4096 Apr 10 23:09 .
    drwxr-xr-x 1 root root 4096 Apr 10 23:09 ..
    -rw------- 1 root root 2376 Apr 10 23:09 etcd.yaml
    -rw------- 1 root root 3854 Apr 10 23:09 kube-apiserver.yaml
    -rw------- 1 root root 3370 Apr 10 23:09 kube-controller-manager.yaml
    -rw------- 1 root root 1440 Apr 10 23:09 kube-scheduler.yaml
    ```
  
  </details>


- What is the Common Name (CN) configured on the Kube API Server Certificate?

  <details>
  <summary>Answer</summary>

  - View OpenSSL Syntax: 
    - [8. View the certificate](https://kubernetes.io/docs/tasks/administer-cluster/certificates/#openssl)

  ```
  openssl x509 -in [FILE_PATH.crt] -text -noout
  openssl x509 -in /etc/kubernetes/pki/apiserver.crt -text
  ```

  </details>

## Certificates API

- Kubernetes has a built-in certificates API
    - With the certificate API, we now send a CSR directly to kubernetes through an API call.

- A user first creates a key
  ```
  $ openssl genrsa -out jane.key 2048
  ```
- Generates a CSR (Certificate Signing Request)
  ```
  $ openssl req -new -key jane.key -subj "/CN=jane" -out jane.csr 
  ```
- Sends the request to the administrator and the adminsitrator takes the key and creates a CSR object, with kind as "CertificateSigningRequest" and a encoded "jane.csr" with base64

  ```yaml
  apiVersion: certificates.k8s.io/v1beta1
  kind: CertificateSigningRequest
  metadata:
    name: jane
  spec:
    groups:
    - system:authenticated
    usages:
    - digital signature
    - key encipherment
    - server auth
    request:
      <certificate-goes-here>
  ```

  ```
  $ cat jane.csr | base64 
  $ kubectl create -f jane.yaml
  ```

- To list the csr's
  ```
  $ kubectl get csr
  ```
- Approve the request
  ```
  $ kubectl certificate approve jane
  ```
- To view the certificate
  ```
  $ kubectl get csr jane -o yaml
  ```
- To decode it
  ```
  $ echo "<certificate>" | base64 --decode
  ```
  
#### K8s Reference Docs
- https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/
- https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/

### Practice

- Create a CertificateSigningRequest object with the name akshay with the contents of the akshay.csr file
    - [Kubernetes Documentation - Certificate Signing Request > Create CSR](https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/#create-certificatesigningrequest)

    - After editing akshay.yaml

    ```
    kubectl create -f akshay.yaml
    ```

- Approve the CSR

```
kubectl certificate approve myuser
```

- To check CSR

```
kubectl get csr [CSR NAME] -o yaml 
```

- To Deny a CSR

```
kubectl certificate deny [certificate-signing-request-name]
```

- Delete the new CSR object

```
kubectl delete csr [CSR NAME]
```

## KubeConfig

- `curl` enables data exchange between a device and a server through a terminal.

![curl](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/kc1.PNG)

- Client uses the certificate file and key to query the kubernetes Rest API for a list of pods using curl.

- Also, you can specify the same using `kubectl`

- Typing the above texts everytime is a tedious work, so we can move these information to a configuration file called KubeConfig

```
$ kubectl get pods --kubeconfig config
```

### KubeConfig

- [KubeConfig](https://cloud.google.com/anthos/clusters/docs/multi-cloud/aws/concepts/about-kubeconfig)
  - Kubernetes uses a **YAML file** called `kubeconfig` to store cluster authentication information for `kubectl`. `kubeconfig` contains a list of contexts to which kubectl refers when running commands. **By default, the file is saved at `$HOME/.kube/config`**.

### KubeConfig File

- The kubeconfig file has 3 sections
  - **Clusters**
    - Clusters are the various Kubernetes clusters that you need access to.
    - Development / Prooduction / prod
  
  - **Contexts**
    - Contexts define which user account will be used to access which cluster.

  - **Users**
    - Admin, Dev User, Prod User
    - Users may have different priviledges on different clusters

```yaml
apiVersion: v1
kind: Config
current-context: "" ## if there are many clusters, specify the context to use

clusters:
- name: my-kube-playground
  cluster:
    certificate-authority: ca.crt
    server: https://my-kube-playground:6443

users:
- name: my-kube-admin
  user: 
    client-certificate: admin.crt
    client-key: admin.key

contexts:
- name: my-kube-admin
  user: 
    client-certificate: admin.crt
    client-key: admin.key
```

- To view the current file being used
  
  ```
  $ kubectl config view
  ```
- You can specify the kubeconfig file with kubectl config view with "--kubeconfig" flag
  
  ```
  $ kubectl config veiw --kubeconfig=my-custom-config
  ```

- How do you update your current context? Or change the current context
  
  ```
  $ kubectl config view --kubeconfig=my-custom-config
  ```
  
- kubectl config help
  ```
  $ kubectl config -h
  ```

![Certificates](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/kc11.PNG)

- you may optionally use the certificate authority data field and provide the contents of the certificate itself but not the file as is.

#### K8s Reference Docs
- https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/
- https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#config

### Practice

- I would like to use the `dev-user` to access `test-cluster-1.` Set the current context to the right one so I can do that.

  - you can quickly switch between clusters by using the `kubectl config use-context` command.

  <details>
  <summary>Answer</summary>

  ```yaml
  ~# cat my-kube-config 
  apiVersion: v1
  kind: Config

  clusters:
  - name: production
    cluster:
      certificate-authority: /etc/kubernetes/pki/ca.crt
      server: https://controlplane:6443
  - name: development
    cluster:
      certificate-authority: /etc/kubernetes/pki/ca.crt
      server: https://controlplane:6443
  - name: test-cluster-1
    cluster:
      certificate-authority: /etc/kubernetes/pki/ca.crt
      server: https://controlplane:6443

  contexts:
  - name: test-user@development
    context:
      cluster: development
      user: test-user
  - name: research
    context:
      cluster: test-cluster-1
      user: dev-user

  users:
  - name: test-user
    user:
      client-certificate: /etc/kubernetes/pki/users/test-user/test-user.crt
      client-key: /etc/kubernetes/pki/users/test-user/test-user.key
  - name: dev-user
    user:
      client-certificate: /etc/kubernetes/pki/users/dev-user/developer-user.crt
      client-key: /etc/kubernetes/pki/users/dev-user/dev-user.key

  current-context: test-user@development
  preferences: {}
  ```

  ```
  ~# kubectl config use-context research --kubeconfig /root/my-kube-config
  ~# kubectl config use-context [CONTEXT NAME] --kubeconfig [KubeConfig PATH]
  ```

  </details>

- Make the `my-kube-config` file the default kubeconfig.

  <details>
  <summary>Answer</summary>

  ```
  mv /root/my-kube-config /root/.kube/config 
  ```
  
  </details>


## API Groups
- **API**(Application Programming Interface)
- - The kubernetes API is grouped into multiple such groups based on thier purpose. Such as one for **`APIs`**, one for **`healthz`**, **`metrics`** and **`logs`** etc.

### API and APIs
![keyy](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/api10.PNG)

- These APIs are categorized into two;
  - `/api` : the core group 
    - Where all the functionality exists
  - `/apis` : the named group
    - More organized and going forward all the newer features are going to be made available to these named groups.


## Authorization

### Authorization Mechanisms
- There are different authorization mechanisms supported by kubernetes
  - Node Authorization
  - Attribute-based Authorization (ABAC)
  - **Role-Based Authorization (RBAC)**
  - Webhook
  

- **Authorization Modes**
  ![am](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/mode.PNG)
  - The mode options can be defined on the `kube-apiserver`
  - When you have multiple modes configured your request is authorized using each one in the order it is specified.
  - So, every time a module denies the request it goes to the next one in the chain and as soon as a module approves the request no more checks are done and the user is granted permission.

## Role Based Access Controls
> RBAC  
> Role-based access control (RBAC) refers to the idea of assigning permissions to users based on their role within an organization. 


### How do we create a role?
- Each role has 3 sections
  - apiGroups
  - resources
  - verbs
- create the role with kubectl command
  ```
  $ kubectl create -f developer-role.yaml
  ```

### The next step is to link the user to that role.
- For this we create another object called **`RoleBinding`**. This role binding object links a user object to a role.
- create the role binding using kubectl command
  ```
  $ kubectl create -f devuser-developer-binding.yaml
  ```
- Also note that the roles and role bindings fall under the scope of namespace.
  ```yaml
  apiVersion: rbac.authorization.k8s.io/v1
  kind: Role
  metadata:
    name: developer
  rules:
  - apiGroups: [""] # "" indicates the core API group
    resources: ["pods"]
    verbs: ["get", "list", "update", "delete", "create"]
  - apiGroups: [""]
    resources: ["ConfigMap"]
    verbs: ["create"]
  ```

  ```yaml
  apiVersion: rbac.authorization.k8s.io/v1
  kind: RoleBinding
  metadata:
    name: devuser-developer-binding
  subjects:
  - kind: User
    name: dev-user # "name" is case sensitive
    apiGroup: rbac.authorization.k8s.io
  roleRef:
    kind: Role
    name: developer
    apiGroup: rbac.authorization.k8s.io
  ```

### View RBAC
  
- To list roles
  ```
  $ kubectl get roles
  ```
- To list rolebindings
  ```
  $ kubectl get rolebindings
  ```
- To describe role 
  ```
  $ kubectl describe role developer
  ```
    
- To describe rolebinding
  ```
  $ kubectl describe rolebinding devuser-developer-binding
  ```
  
#### What if you being a user would like to see if you have access to a particular resource in the cluster.
### Check Access

- You can use the kubectl auth command by `can-i` command
  ```
  $ kubectl auth can-i create deployments
  $ kubectl auth can-i delete nodes
  ```

  ```
  $ kubectl auth can-i create deployments --as dev-user
  $ kubectl auth can-i create pods --as dev-user
  ```

  ```
  $ kubectl auth can-i create pods --as dev-user --namespace test
  ```
### Resource Names
- Note on resource names we just saw how you can provide access to users for resources like pods within the namespace.
  ```yaml
  apiVersion: rbac.authorization.k8s.io/v1
  kind: Role
  metadata:
    name: developer
  rules:
  - apiGroups: [""] # "" indicates the core API group
    resources: ["pods"]
    verbs: ["get", "update", "create"]
    resourceNames: ["blue", "orange"] ## Allow blue and orange pod only
  ```  

### Practice

- Inspect the environment and identify the authorization modes configured on the cluster.

  <details>
  <summary>Answer</summary>

  - Check the kube-apiserver settings.

  ```console
  ~# k get pods -n kube-system 
  NAME                                   READY   STATUS    RESTARTS   AGE
  coredns-787d4945fb-7j9q9               1/1     Running   0          11m
  coredns-787d4945fb-sqchs               1/1     Running   0          11m
  etcd-controlplane                      1/1     Running   0          11m
  kube-apiserver-controlplane            1/1     Running   0          11m
  kube-controller-manager-controlplane   1/1     Running   0          11m
  kube-proxy-94t8s                       1/1     Running   0          11m
  kube-scheduler-controlplane            1/1     Running   0          11m
  ```
  
  ```console
  ~# k describe pod kube-apiserver-controlplane -n kube-system | grep authorization
      --authorization-mode=Node,RBAC
  ```

  - Another way to figure out is 
  ```
  ps -aux | grep authorization
  ```

  </details>

- How many roles exist in the default namespace?
  <details>
  <summary>Answer</summary>
  
  ```
  ~# k get roles
  ```
  </details>

- How many roles exist in all namespaces together?

  <details>
  <summary>Answer</summary>

  ```
  ~# k get roles -A --no-headers | wc -l
  ```
  
  </details>

- What are the resources the kube-proxy role in the kube-system namespace is given access to?

  <details>
  <summary>Answer</summary>

  ```console
  ~# k describe roles kube-proxy -n kube-system 
  Name:         kube-proxy
  Labels:       <none>
  Annotations:  <none>
  PolicyRule:
    Resources   Non-Resource URLs  Resource Names  Verbs
    ---------   -----------------  --------------  -----
    configmaps  []                 [kube-proxy]    [get]

  ```
  
  </details>

- Which account is the kube-proxy role assigned to?

  <details>
  <summary>Answer</summary>

  - **Role Binding**
  
  ```console
  ~# k get rolebindings -n kube-system
  ~# k describe rolebindings kube-proxy -n kube-system
  ```
  
  </details>

- Edit role
  
  ```
  k -as dev-user get pod dark-blue-app -n blue
  k get roles -n blue
  k get rolebindings -n blue
  k desribe role developer -n blue
  k edit role developer -n blue
  ```

- https://kubernetes.io/docs/reference/access-authn-authz/rbac/#kubectl-create-role

- https://kubernetes.io/docs/reference/access-authn-authz/rbac/#kubectl-create-clusterrolebinding

## Cluster Roles and Role Bindings

- Can you group or isolate **nodes** within a namespace?
  - No, those are cluster wide or cluster scoped resources. They cannot be associated to any particular namespace.

- But how do we authorize users to cluster-wide resources like nodes or persistent volumes.
  - That is where you use **cluster roles and cluster role bindings.**

![c](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/cr1.PNG)

#### K8s Reference Docs
- https://kubernetes.io/docs/reference/access-authn-authz/rbac/#role-and-clusterrole
- https://kubernetes.io/docs/reference/access-authn-authz/rbac/#command-line-utilities
  
### Practice
- How many ClusterRoles do you see defined in the cluster?

  <details>
  <summary>Answer</summary>

  ```
  $ kubectl get clusterroles --no-headers | wc -l
  ```
  
  </details>

- How many ClusterRoleBindings exist on the cluster?

  <details>
  <summary>Answer</summary>

  ```
  $ kubectl get clusterrolebindings --no-headers | wc -l
  ```
  
  </details>

- Cluster Roles are cluster wide and not part of any namespace

- What user/groups are the `cluster-admin` role bound to?

  <details>
  <summary>Answer</summary>

  ```
  $ kubectl describe clusterrolebinding cluster-admin
  ```
  
  </details>


- What level of permission does the cluster-admin role grant?

  <details>
  <summary>Answer</summary>

  ```
  $ kubectl describe clusterrole cluster-admin
  ```
  
  </details>

- A new user michelle joined the team. She will be focusing on the nodes in the cluster. Create the required ClusterRoles and ClusterRoleBindings so she gets access to the nodes.

  <details>
  <summary>Answer</summary>

  ```
  $ k create clusterrole --help
  $ k create clusterrole michelle-role --verb=get,list,watch --resource=nodes 


  $ k create clusterrolebinding --help
  $ kubectl create clusterrolebinding michelle-role-binding --clusterrole=cluster-admin --user=michelle

  ## check role in YAML format after creation
  $ k get clusterrole michelle-role -o yaml
  ``` 
  
  </details>


## Service Accounts
> *NOT PART OF THE CKA*


- A service account could be an account used by an application to interact with a Kubernetes cluster.

  - For example, a monitoring application like Prometheus uses a service account to pull the Kubernetes API for performance metrics.

- Kubernetes automatically mounts the default service account if you haven't explicitly specified any.


## Image Security
- you may choose to make a repository private so that it can be accessed using a set of credentials.

- Within Kubernetes, we know that the images are pulled and run by the Docker run time on the worker nodes.

- image:docker.io/nginx/nginx
  - `docker.io` : Registry
  - `nginx` : User/Account
  - `nginx` : Image/Repository

## Private Registry
![pr](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/prvr1.PNG)

- To login to the registry
  ```
  $ docker login private-registry.io
  ```
- Run the application using the image available at the private registry
  ```
  $ docker run private-registry.io/apps/internal-app
  ```
  
- To pass the credentials to the docker untaged on the worker node for that we first create a secret object with credentials in it.
  ```
  $ kubectl create secret docker-registry regcred \
    --docker-server=private-registry.io \ 
    --docker-username=registry-user \
    --docker-password=registry-password \
    --docker-email=registry-user@org.com
  ```
- We then specify the secret inside our pod definition file under the imagePullSecret section 
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: nginx-pod
  spec:
    containers:
    - name: nginx
      image: private-registry.io/apps/internal-app
    imagePullSecrets:
    - name: regcred
  ```

### Practice
- We decided to use a modified version of the application from an internal private registry. Update the image of the deployment to use a new image from `myprivateregistry.com:5000`  

  <details>
  <summary>Answer</summary>

  ```
  $ k edit deploy web
  ``` 

  - Update image filed from `nginx:alpine` to `myprivateregistry.com:5000/nginx:alpine`

  ```yaml
  ...
  spec:
    containers:
    - image : myprivateregistry.com:5000/nginx:alpine
  ```

  </details>

- Create a secret object with the credentials required to access the registry.
  <details>
  <summary>Answer</summary>
  
  ```console
  $ k create secret docker-registry -h
  
  Examples:
  # If you don't already have a .dockercfg file, you can create a dockercfg secret directly by using:
  kubectl create secret docker-registry my-secret --docker-server=DOCKER_REGISTRY_SERVER --docker-username=DOCKER_USER --docker-password=DOCKER_PASSWORD --docker-email=DOCKER_EMAIL
  ```
  
  </details>

- [Pull an Image from a Private Registry](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/)

  - Create a Pod that uses your Secret

  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: private-reg
  spec:
    containers:
    - name: private-reg-container
      image: <your-private-image>
    imagePullSecrets: ## This field should be udpated to pull images from the private registry
    - name: regcred
  ```

## Security Contexts

#### A security context defines privilege and access control settings for a Pod or Container.

- You may choose to configure the security settings at a container level or at a pod level.

- To add security context on the container and a field called **`securityContext`** under the spec section.
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: web-pod
  spec:
    securityContext: ###
      runAsUser: 1000
    containers:
    - name: ubuntu
      image: ubuntu
      command: ["sleep", "3600"]
  ```

  
- To set the same context at the **container level**, then move the whole section under container section.
  
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: web-pod
  spec:
    containers:
    - name: ubuntu
      image: ubuntu
      command: ["sleep", "3600"]
      securityContext: ###
        runAsUser: 1000
  ```

  
- To add capabilities use the **`capabilities`** option
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    name: web-pod
  spec:
    containers:
    - name: ubuntu
      image: ubuntu
      command: ["sleep", "3600"]
      securityContext:
        runAsUser: 1000
        capabilities: ###
          add: ["MAC_ADMIN"]
  ```

### Practice

- What is the user used to execute the sleep process within the ubuntu-sleeper pod?

  <details>
  <summary>Answer</summary>

  ```
  $ k exec ubuntu-sleeper -- whoami
  root
  ```
  
  </details>

- Edit the pod ubuntu-sleeper to run the sleep process with user ID 1010.
  - [Search Security Context in K8s Documentation](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)

  <details>
  <summary>Answer</summary>

  ```
  $  k edit pod ubuntu-sleeper
  ```
  
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: "2023-04-15T16:09:56Z"
    name: ubuntu-sleeper
    namespace: default
    resourceVersion: "871"
    uid: 123713a1-771f-4e19-8f91-717268200df2
  spec:
    containers:
    - command:
      - sleep
      - "4800"
      image: ubuntu
      securityConext: ## Add securityContext
        runAsUser: 1010
      imagePullPolicy: Always
      name: ubuntu
  ```


  ```console
  A copy of your changes has been stored to "/tmp/kubectl-edit-1494743597.yaml"
  $ k replace --force -f /tmp/kubectl-edit-1494743597.yaml
  ```
  
  </details>


- **The security settings that you specify for a Pod apply to all Containers in the Pod.**

- Wit what user are the processes in the `sidecar` container started?

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-pod
spec:
  securityContext:
    runAsUser: 1001 ### it started with 1001 User
  containers:
  -  image: ubuntu
     name: web
     command: ["sleep", "5000"]
     securityContext:
      runAsUser: 1002

  -  image: ubuntu
     name: sidecar
     command: ["sleep", "5000"]
```

- Update pod ubuntu-sleeper to run as Root user and with the SYS_TIME capability.

- Search security context in K8s Documentation and search **capabilities** (NOT capability)

  <details>
  <summary>Answer</summary>

  ```
  $  k edit pod ubuntu-sleeper
  ```
  
  ```yaml
  apiVersion: v1
  kind: Pod
  metadata:
    creationTimestamp: "2023-04-15T16:09:56Z"
    name: ubuntu-sleeper
    namespace: default
    resourceVersion: "871"
    uid: 123713a1-771f-4e19-8f91-717268200df2
  spec:
    containers:
    - command:
      - sleep
      - "4800"
      image: ubuntu
      securityConext: 
        runAsUser: 1010
        capabilities: ## Add capabilities
        add: ["SYS_TIME"]
      imagePullPolicy: Always
      name: ubuntu
  ```


  ```console
  A copy of your changes has been stored to "/tmp/kubectl-edit-1494743597.yaml"
  $ k replace --force -f /tmp/kubectl-edit-1494743597.yaml
  ```
  
  </details>


## Network Policy

#### NetworkPolicies are an application-centric construct which allow you to specify how a pod is allowed to communicate with various network "entities

- Traffic
  - **ingress traffic** : the incoming traffic from the users
  - **egress traffic** : the outgoing request to the server

- And network policy is another object in the Kubernetes namespace just like pods, replica sets or services, you link a network policy to one or more pods.

- how do you apply or link a network policy to a pod?
  - **labels and selectors**
  - We label the pod and use the same labels on the port selector field in the network policy and then we build our rule.


### Create a Network Policy

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
 name: db-policy
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          role: api-pod
      namespaceSelector: ## since there is no '-' infront of the namespaceSelector, AND rule applys for the podSelector and namespaceSelector
        matchLables:
          name: prod
    - ipBlock:
        cidr: 192.168.5.10/32
    ports:
    - protocol: TCP
      port: 3306
```

- From the DB pod's perspective,
  - we want to allow incoming traffic from the API pod.
  - So that is incoming. So that is ingress.

- However, this rule does not mean that the database pod will be able to connect to the API pod or make calls to the API.

-  the database pod tries to make an API call to the API pod, then that would not be allowed because that is now an egress traffic

- The `from` field defines the source of traffic that is allowed to pass through to the database pod.

- The `ports` field defines what port on the database pod is the traffic allowed to go to.
  - In this case, it's 3306 with the TCP protocol

- The current policy would allow any pod in any namespace with matching labels to reach the database pod.
  - If you want to allow the pod in the specific namespace, need to add `namespaceSelector` field

- `IP block` allows you to specify a range of IP addresses from which you could allow traffic to hit the database pod.

### Practice

- How many network policies do you see in the environment?

  <details>
  <summary>Answer</summary>

  ```console
  $ k get networkpolicies
  $ k get netpol
  NAME             POD-SELECTOR   AGE
  payroll-policy   name=payroll   2m34s

  $ k describe networkpolicies payroll-policy 
  Name:         payroll-policy
  Namespace:    default
  Created on:   2023-04-15 13:24:35 -0400 EDT
  Labels:       <none>
  Annotations:  <none>
  Spec:
    PodSelector:     name=payroll
    Allowing ingress traffic:
      To Port: 8080/TCP
      From:
        PodSelector: name=internal
    Not affecting egress traffic
    Policy Types: Ingress
  ```
  
  </details>  

- Create a network policy

  <details>
  <summary>Answer</summary>

  ```console
  $ vi internal-policy.yaml
  ``` 

  ```yaml
  apiVersion: networking.k8s.io/v1
  kind: NetworkPolicy
  metadata:
    name: test-network-policy
    namespace: default
  spec:
    podSelector:
      matchLabels:
        role: db
    policyTypes:
      - Ingress
      - Egress
    ingress:
      - from:
          - ipBlock:
              cidr: 172.17.0.0/16
              except:
                - 172.17.1.0/24
          - namespaceSelector:
              matchLabels:
                project: myproject
          - podSelector:
              matchLabels:
                role: frontend
        ports:
          - protocol: TCP
            port: 6379
    egress:
      - to:
          - ipBlock:
              cidr: 10.0.0.0/24
        ports:
          - protocol: TCP
            port: 5978
  ```

  ```console
  $ k create -f internal-policy.yaml
  ```
  
  </details>
