# Section 7 - Security

- [Authentification](#Authentification)  
- [TLS](#TLS)  
- [View Certificates](#View-Certificates)  
- [Certificates API](#Certificates-API)  


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

<!--

  <details>
  <summary>Answer</summary>

  - 
  
  </details>

-->