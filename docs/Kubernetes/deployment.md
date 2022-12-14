# Deployment

> The deployment provides us with capabilities to upgrade the underlying instances seamlessly using rolling updates, undo changes, and pause and resume changes to deployments.


- Rolling updates
    - when you **upgrade** your instances, you do not want to upgrade all of them at once as we just did. This may impact users accessing our applications, so you may want to upgrade them one after the other. And that kind of upgrade is known as Rolling Updates.


- Deployment example (deployment-definition.yaml)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata: 
  name: myapp-deployment
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
  selector:
    matchLabels:
      type: front-end
```

## Command

```console
> kubectl create -f deployment-definition.yaml
> kubectl get deployments
> kubectl get all
```