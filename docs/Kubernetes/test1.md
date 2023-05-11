# Practice Exam

## 01 : etcd backup restore
- Snapshot
    - ETCDCTL_API=3 etcdctl --endpoints=https://127.0.0.1:2379 \
  --cacert=<trusted-ca-file> --cert=<cert-file> --key=<key-file> \
  snapshot save <backup-file-location>

- Restore
    - ETCDCTL_API=3 etcdctl snapshot restore --data-dir <data-dir-location> snapshotdb


## 02 : Upgrade

- k get nodes
- ssh hk8s-m
- sudo -i
- kubeadm upgrade plan v1.23.3
- exit
- exit
    - exit before drain
- kubectl drain hk8s-m --ignore-daemonsets
- ssh hk8s-m
- sudo -i
- exit
- exit
    - exit before uncordon
- k uncordon hk8s-m

## 03 : ServiceAccount - role / rolebinding
- k create serviceaccount -h
- kubectl create serviceaccount pod-viewer -n api-access

- k create role -h
- kubectl create role podreader-role --verb=watch,list,get **--resource=pod -n api-access**

- k create rolebinding -h
- kubectl create rolebinding podreader-rolebinding --role=podreader-role --serviceaccount=api-access:pod-reviewer -n api-access

- kubectl describe rolebinding podreader-rolebinding -n api-access

## 04 : Serviceaccount > clusterrole > clusterrolebinding

- Create clusterrole
    - k create clusterrole -h
    - kubectl create clusterrole deployment-clusterrole --verb=create --resource=deployment,statefulSet,daemonSet
- Create serviceAccount
    - k create serviceaccount -h
    - kubectl create serviceaccount cicd-token -n api-access

- ClusterRoleBinding
    - k create clusterrolebinding -h
    - kubectl create clusterrolebinding deployment-clusterrolebinding --clusterrole=deployment-clusterrole --serviceaccount=**api-access:cicd-token**

## 06 : POD creation
- environment Variable: CERT = "CKA-cert"
- command: /bin/sh
- args: -c "while true; do echo $(CERT); sleep 10;done"

- **First, create namespace**
    - kubectl create namespace cka-exam

- kubectl run pod-01 --image=busybox -n cka-exam --dry-run=client -o yaml > pod-01.yaml

- vi pod-01.yaml
    - **env > Define Enviroment Variable**
   
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
    creationTimestamp: null
    labels:
        run: pod-01
    name: pod-01
    namespace: cka-exam
    spec:
    containers:
    - image: busybox
        name: pod-01
        resources: {}
        env:
        - name: CERT
          value: "CKA-cert"
        command: ["/bin/sh"]
        args: ["-c", "while true; do echo $(CERT); sleep 10;done"]
    dnsPolicy: ClusterFirst
    restartPolicy: Always
    status: {}
    ```

    - in args **"-c"** is separated 

## 07 : pod log extract

- kubectl logs custom-app | grep -i 'file-not-found' > /var/CKA2023/custom-app-log

- k logs custom-app | grep i 'file-not-found' > /~~
- k logs [POD NAME] | grep i [LOG LINES] > /[FILE PATH]

## 08 : Static pod

- ssh hk8s-w1
- sudo -i

- cd /etc/kubernetes/manifests
- kubectl run nginx-static-pod --image=nginx --port=80 --dry-run=client -o yaml > nginx-static-pod.yaml
- k apply -f nginx-static-pod.yaml

- exit
- exit

- k get pods -o wide

## 09 : multi-container POD
- kubectl run eshop-frontend --image=nginx --dry-run=client -o yaml > multipod.yaml
- vi multipod.yaml
- 여기서 spec > containers 아래에 image와 name 두 줄 복사해서 밑으로 세 번 붙여넣습니다. (두 줄 복사: 2yy, 붙여넣기: p)

## 10 : sidecar container pod 
- k8s documentation
    - sidecar > Logging Architecture
    - 3번 째 그림 지나가고 두번 째 yaml 참고

- kubectl get pods eshop-cart-app -o yaml > sidecar.yaml
- vi sidecar.yaml

```yaml
- name: price
  image: busybox 
  args : ["/bin/sh", "-c", "tail -n+1 -F /var/log/cart-app.log"]
  volumneMounts:
  - name: varlog
    mountPath: /var/log
```

- args에는 
    - args: [/bin/sh, -c, 'tail -n+1 -F /var/log/cart-app.log'] 혹은 
    - args: ["/bin/sh", "-c", "tail -n+1 -F /var/log/cart-app.log"] 로 넣습니다.
- 컨테이너에 volumeMounts: 를 추가하고아래에 name과 mountPath를 넣습니다. name은 varlog로, mountPath는 문제에서 제시한 /var/log 로 넣습니다.

- k apply -f sidecar.yaml
    - The pod is already exits
- k delete pods [POD NAME]
- k apply -f sidecar.yaml

## 11 : deployment & scaling

- kubectl create deployment webserver --image=nginx:1.14 --replicas=2 --dry-run=client -o yaml > webserver.yaml

- vi webserver.yaml
- label 추가, container > name 변경
- kubectl apply -f webserver.yaml
- **kubectl scale deployment webserver --replicas=3**

## 13 : rescedule all the pods

- kubectl drain k8s-worker2 --ignore-daemonsets --force

## 14 : Find the nodes that are Ready

- kubectl get nodes
- kubectl get node hk8s-m -o yaml | grep -i noschedule
- kubectl get node hk8s-w1 -o yaml | grep -i noschedule

- echo "1" > /var/CKA2023/notaint_ready_node

## 15 : Pod Scheduling

- kubectl run eshop-store --image=nginx --dry-run=client -o yaml > eshop.yaml
- vi eshop.yaml

- documentation
    - nodeselector
    - 5번째 문서 Assign Pods to Nodes | Kubernetes 들어가서(첫번째 Assining~ 아님!)

## 16 : Create a service by exposing with ClusterIP type
- 시험에서는 ClusterIP 대신 다른 타입이 나온다.
- kubectl get namespaces
- kubectl get deployment -n devops
- kubectl expose deployment eshop-order -n devops --port=80 --type=ClusterIP --name=eshop-order-svc

- k get svc -n devops

## 17 : Reconfigure the existing deployment - expose the service via Nodeport
- kubectl get deployment front-end -o yaml > front-deploy.yaml
- vi front-deploy.yaml

- Document “nodeport” 검색 
- 첫번째 문서(Service) 들어가서,
- 두번째 yaml 참고

- deployment > containers>name **(http로)수정, image(nginx로 ) 수정, ports 추가**
- service > metadata > name (front-end-svc로)수정, 
- **type:NodePort** search
    - spec밑에 selector위에 type: NodePort 추가, 
- selector 아래에 app.kubernetes.io/name: proxy → name: 수정(deployment label과 동일), 
- 마지막 targetPort (http로) 수정

```yaml
apiVersion: apps/v1
kind: Deployment
...
template:
	metadata:
		labels:
			run: nginx
		spec:
			containers:
			- image: nginx
			  name: http
			  ports:
			  - containerPort: 80
			 	name: http
---
apiVersion: v1
kind: Service
metadata:
  name: front-end-svc
spec:
  type: NodePort
  selector:
    run: nginx
  ports:
  - name: name-of-service-port
    protocol: TCP
    port: 80
    targetPort: http
```

- kubectl apply -f front-deploy.yaml

## 19 : NetworkPolicy
- kubectl run poc --image=nginx --port=80 --labels=app=poc
- vi n-policy.yaml

- Documentation “network policy” 검색 > 
- 첫 번째 문서 Network Policies | Kubernetes > 
- 첫 번째 yaml 참고
- 처음부터 egress 바로 전까지 복붙 (시험에는 ingress만 나옵니다)

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-web-from-customera
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
    - Ingress
  ingress:
    - from:
        - namespaceSelector:
            matchLabels:
              partition: customera
  ports:
    - protocol: TCP
      port: 80
```

- k apply -f n-policy.yaml
- k get networkpolicies


## 20 : Ingress

- kubectl run nginx --image=nginx --labels=app=nginx --namespace=ingress-nginx --port 80
- **nginx port is 80**

- vi app-ingress.yaml
- Document “ingress” 검색 >
- 첫 번째 문서 Ingress | Kubernetes >
- 첫 번째 yaml 전체 복 붙

- metadata > name 수정, 
- **name위에 namespace 넣기,** 
- annotations 수정, 
- spec > ingressClassName은 문제에 얘기 없으니까 지워,
- path / 로 수정, 
- service > name nginx 로 수정, 
- 포트 수정(아까 kubectl get svc -n ingress-nginx 명령어로 확인한 포트로)
    - kubectl get svc -n ingress-nginx

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
	namespace: ingress-nginx
  name: app-ingress
  annotations:
		nginx.ingress.kubernetes.io/ingress.class: nginx
spec:
  rules:
  - http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: nginx
            port:
              number: 80
      - path: /app
        pathType: Prefix
        backend:
          service:
            name: appjs-service
            port:
              number: 80
```

- kubectl apply -f app-ingress.yaml


## 21 : Service and DNS Lookup
- kubectl run resolver --image=nginx --port=80
- kubectl get pods resolver -o wide
- kubectl expose pod resolver --name resolver-service --port=80

- kubectl run test-nslookup --image=busybox:1.28 --restart=Never -it --rm --nslookup resolver-service
- kubectl run test-nslookup --image=busybox:1.28 --restart=Never -it --rm --nslookup resolver-service > /var/CKA2022/nginx.svc
- k get pods -o wide
- kubectl run test-nslookup --image=busybox:1.28 --restart=Never -it --rm --nslookup 10-244-1-53.default.pod.cluster.local
- kubectl run test-nslookup --image=busybox:1.28 --restart=Never -it --rm --nslookup 10-244-1-53.default.pod.cluster.local > /var/CKA2022/nginx.pod
    - **--restart=Never --it --rm**

## 23 : HostPath Volumne Mount
- cd /data/cka
- vi pod-tensorflow.yaml
- Document **“hostpath”** 검색 >
- 첫 번째 문서 Volumes | Kubernetes >
- 쭉-내려서 warning 두 개 지난 뒤 “hostPath configuration example” 아래 yaml 참고

- worker node의 도커 컨테이너 디렉토리는 **/var/lib/docker/containers** 기억하기!

```yaml
apiVersion: v1
kind: Pod
metadata: 
	name: tensorflow-gpu
spec:
	containers:
	- name: tensorflow
		image: tensorflow/tensorflow:nightly-jupyter
		ports:
		- containerPort: 8888
			protocol: TCP
		volumeMounts:
		- mountPath: /var/lib/docker/containers
			name: varlog
		- mountPath: /var/log
			name: varlog1
	volumes:
	- name: varlog
		hostPath:
			path: /var/lib/docker/containers
	- name: varlog1
		hostPath:
			path: /var/log
```

## 24 : PV 
- Document “pv” 검색 > 
- 첫 번째 문서 Persistent Volumes | Kubernetes >
- 쭉-내려서 Persistent Volumes 아래 yaml 참고

- **vi pv.yaml**

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv001
spec:
  capacity:
    storage: 1Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteMany
	hostPath:
		path: /tmp/app-config
```
- k apply -f pv.yaml


## 25 : PVC
- **vi pvc.yaml**

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pv-volume
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 10Mi
  storageClassName: app-hostpath-sc
```

- kubectl apply -f pvc.yaml

- vi pod.yaml
- 아까 켜두었던 문서  Persistent Volumes | Kubernetes >
- 더 내려서 “Claims As Volumes” 아래 야믈 복 붙

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web-server-pod
spec:
  containers:
    - name: web-server-pod
      image: nginx
      volumeMounts:
      - mountPath: "/usr/share/nginx/html"
        name: mypd
  volumes:
    - name: mypd
      persistentVolumeClaim:
        claimName: pv-volume
```

## 27 : PV
- Document “sort” 검색 >
- 세 번째 문서 kubectl Cheat Sheet | Kubernetes >
- Viewing and finding resources 아래에 sorted by capacity 코드 참고

- List PersistentVolumes sorted by capacity
    - kubectl get pv --sort-by=.spec.capacity.storage

- kubectl get pv --sort-by=.spec.capacity.storage > /var/CKA2023/my-pv-list

## 29 : Find the name of the pod with the highest CPU consumption among the pods 
- Document “cheat” 검색 >
- 첫 번째 문서 kubectl Cheat Sheet | Kubernetes >
- Interacting with running Pods 아래에 kubectl top pod POD_NAME --sort-by=cpu 코드 참고

- kubectl top pod POD_NAME --sort-by=cpu  
- kubectl top pod **-l name=overloaded-cpu** --sort-by=cpu

- echo 'campus-01' > /var/CKA2023/custom-app-log

## 29 : A Kubernetes worker node, named hk8s-w2 is in state NotReady.

- kubectl get nodes
- ssh hk8s-w2
- sudo -i

- docker ps
- systemctl status docker

- **systemctl status kubelet**
- **systemctl enable --now kubelet**

- systemctl status kubelet
- exit
- exit

- k get nodes

## 30 : A Kubernetes worker node, named hk8s-w2 is in state NotReady.

- kubectl get nodes
- ssh hk8s-w2
- sudo -i

- docker ps
- systemctl status docker
- systemctl enable --now docker

- systemctl status docker
- exit
- exit

- k get nodes