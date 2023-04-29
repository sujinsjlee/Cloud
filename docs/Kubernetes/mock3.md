# Mock Exam 3 Solution

1. Create a new service account with the name `pvviewer`. Grant this Service account access to `list` all PersistentVolumes in the cluster by creating an appropriate cluster role called `pvviewer-role` and ClusterRoleBinding called `pvviewer-role-binding`.
Next, create a pod called `pvviewer` with the image: `redis` and serviceAccount: `pvviewer` in the default namespace.

- ServiceAccount: pvviewer
- ClusterRole: pvviewer-role
- ClusterRoleBinding: pvviewer-role-binding
- Pod: pvviewer
- Pod configured to use ServiceAccount pvviewer ?

     <details>
     <summary>Answer</summary>

     ```
     k create serviceaccount -h
     kubectl create serviceaccount pvviewer
     ```

     - https://kubernetes.io/docs/reference/access-authn-authz/rbac/#kubectl-create-clusterrole

     ```
     # Grant service account accesss
     k create clusterrole -h
     k api-resources | grep persistent
     kubectl create clusterrole pvviewer-role --resource=persistentvolumes --verb=list

     # Create ClusterRoleBinding
     k create clusterrolebinding -h
     kubectl create clusterrolebinding pvviewer-role-binding --clusterrole=pvviewer-role --serviceaccount=default:pvviewer

     k describe clusterrole pvviewer-role
     k describe clusterrolebinding
     ```

    
     ```
     k run pvviewer --image=redis --serviceaccount=pvviewer
     k get pods pvviewer -o yaml
     ```

     ```yaml
     apiVersion: v1
     kind: Pod
     metadata:
       creationTimestamp: null
       labels:
         run: pvviewer
       name: pvviewer
     spec:
       containers:
       - image: redis
         name: pvviewer
         resources: {}
       serviceAccountName: pvviewer
     ```
     </details>

2. List the `InternalIP` of all nodes of the cluster. Save the result to a file `/root/CKA/node_ips`. Answer should be in the format: `InternalIP of controlplane`(**space**)`InternalIP of node01` (in a single line)

   <details>
     <summary>Answer</summary>

     - https://kubernetes.io/docs/reference/kubectl/cheatsheet/
 
     ```
     k get nodes -o json | jq -c 'paths' | grep type | grep -v "metadata" | grep address

     kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="InternalIP")].address}' > /root/CKA/node_ips
     ```
   </details>
 
3. Create a pod called `multi-pod` with two containers.
 
     <details>
     <summary>Answer</summary>

     ```
     k run multi-pod --image=busybox --command --dry-run=client -o yaml -- sleep 4800 > multi-pod.yaml

     vi multi-pod.yaml
     ```

     - [environment variable](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/)
 
     ```yaml
     apiVersion: v1
     kind: Pod
     metadata:
       name: multi-pod
     spec:
       containers:
       - image: nginx
         name: alpha
         env:
         - name: name
           value: alpha
       - image: busybox
         name: beta
         command: ["sleep", "4800"]
         env:
         - name: name
           value: beta
     status: {}
     ```

     ```
     k create -f multi-pod.yaml
     ```
     </details>
 
4. Create a Pod called `non-root-pod` , image: `redis:alpine`
- runAsUser: 1000
- fsGroup: 2000

     <details>
     <summary>Answer</summary>
     
     ```
     k run non-root-pod --image=redis:alpine --dry-run=client -o yaml > pod.yaml
     ``` 

     - [Security Context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-pod)

     ```yaml
     apiVersion: v1
     kind: Pod
     metadata:
       name: non-root-pod
     spec:
       securityContext:
         runAsUser: 1000
         fsGroup: 2000
       containers:
       - name: non-root-pod
         image: redis:alpine
     ```

     ```
     k create -f pod.yaml
     ```
     </details>
 
5. We have deployed a new pod called `np-test-1` and a service called `np-test-service`. Incoming connections to this service are not working. Troubleshoot and fix it. Create NetworkPolicy, by the name `ingress-to-nptest` that allows incoming connections to the service over port `80`.


  <details>
  <summary>Answer</summary>
  - [network policy](https://kubernetes.io/docs/concepts/services-networking/network-policies/#networkpolicy-resource)

  ```
  vi np.yaml

  k get pods np-test-1 -o yaml | grep label -A5 -B5
  k get pods np-test-1 -o yaml | grep label -A5
  ```

  ```yaml
  apiVersion: networking.k8s.io/v1
  kind: NetworkPolicy
  metadata:
    name: ingress-to-nptest
    namespace: default
  spec:
    podSelector:
      matchLabels:
        run: np-test-1  ## NEED TO MATCH with POD label
    policyTypes:
    - Ingress
    ingress:
    - ports:
      - protocol: TCP
        port: 80
  ```

  ```
  k create -f np.yaml
  ```
  </details>
   
6. Taint the worker node `node01` to be Unschedulable. Once done, create a pod called `dev-redis`, image `redis:alpine`, to ensure workloads are not scheduled to this worker node. Finally, create a new pod called `prod-redis` and image: `redis:alpine` with toleration to be scheduled on `node01`.
 
     <details>
     <summary>Answer</summary>
 
     ```
     kubectl taint node node01 env_type=production:NoSchedule
     ```

     Deploy `dev-redis` pod and to ensure that workloads are not scheduled to this `node01` worker node.
     ```
     kubectl run dev-redis --image=redis:alpine

     kubectl get pods -owide
     ```

     Deploy new pod `prod-redis` with toleration to be scheduled on `node01` worker node.
     ```
     apiVersion: v1
     kind: Pod
     metadata:
       name: prod-redis
     spec:
       containers:
       - name: prod-redis
         image: redis:alpine
       tolerations:
       - effect: NoSchedule
         key: env_type
         operator: Equal
         value: production     
     ```

     View the pods with short details: 
     ```
     kubectl get pods -owide | grep prod-redis
     ```
     </details>
 
7. Create a pod called `hr-pod` in `hr` namespace belonging to the `production` environment and `frontend` tier .
 
     <details>
     <summary>Answer</summary>
 
     ```
     kubectl create namespace hr
     kubectl run hr-pod --image=redis:alpine --namespace=hr --labels=environment=production,tier=frontend
     ```
     </details>

8. A kubeconfig file called `super.kubeconfig` has been created under `/root/CKA`. There is something wrong with the configuration. Troubleshoot and fix it.

     <details>
     <summary>Answer</summary>

     ```
     vi /root/CKA/super.kubeconfig

     Change the 2379 port to 6443 and run the below command to verify
     
     kubectl cluster-info --kubeconfig=/root/CKA/super.kubeconfig     
     ```
     </details>

9. We have created a new deployment called `nginx-deploy`. scale the deployment to 3 replicas. Has the replica's increased? Troubleshoot the issue and fix it.
   
     <details>
     <summary>Answer</summary>
     
     ```
     sed -i 's/kube-contro1ler-manager/kube-controller-manager/g' kube-controller-manager.yaml
     ```
     </details>

