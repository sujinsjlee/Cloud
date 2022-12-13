# Replication Controller

## Replication Controller
    - Ensure High availability
    - Load Balancing & Scaling

## Relica Set
    - New recommended version of Replication Controller

- Relication Controller example (rc-definition.yaml)

```yaml
apiVersion: v1
kind: ReplicationController
metadata: # Replication Controller
  name: myapp-rc
  labels:
    app: myapp
    type: front-end

spec:
  template:
    metadata: # POD 
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
      spec:
        containers:
        - name: nginx-container
          image: nginx
  replicas: 3 # Can run three pods
```

- Relica Set example (replicaset-definition.yaml)

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata: # ReplicaSet
  name: myapp-replicaset
  labels:
    app: myapp
    type: front-end

spec:
  template:
    metadata: # POD 
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
      spec:
        containers:
        - name: nginx-container
          image: nginx
  replicas: 3 # Can run three pods
  selector: # Replica Set has selector
    matchLabels:
      type: front-end
```

- **selector** : what PODs are under it
    - need to specify what pods are under replica set

```console
> kubectl "myapp-replicaset" created
> kubectl get replicaset
> kubectl get pods
```

### Scale

- Relica Set example (replicaset-definition.yaml)

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata: 
  name: myapp-replicaset
  labels:
    app: myapp
    type: front-end

spec:
  template:
    metadata: 
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
      spec:
        containers:
        - name: nginx-container
          image: nginx
  replicas: 6 # 3 -> 6
  selector: 
    matchLabels:
      type: front-end
```

```console
> kubectl replace -f replicaset-definition.yaml
```
