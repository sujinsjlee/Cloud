# PODs with YAML

## Creating a POD using YAML configuration file
- K8s uses YAML file as an input of creation of an object such as PODs, deployments..

- YAML in Kubernetes
    - Most of the yaml files have the following format
    - it's recommended to not use a `tab`, please use `two spaces` when you need a space


```yaml
# pod-definition.yaml
apiVersion: v1
kind: Pod
metadata: # Dictionary
    name: myapp-pod
    labels:
        app: myapp
        type: front-end
spec:
    containers: # List (One POD can have multiple containers)
        - name: nginx-container # first Item in List
          image: nginx
        - name: nginx-container2 # second Item in List
          image: nginx

```

- `apiVersion`, `kind`, `metadata`, `spec` are the required field in yaml file
- **apiVersion**
    - string
    - version of k8s api 

        |kind|version|  
        |:---|:---|  
        |POD|v1| 
        |Service|v1|
        |ReplicaSet|apps/v1|
        |Deployment|apps/v1|

- **kind**
    - string
    - type of the object we're trying to create
    - cf) **P**od (o), **p**od (x) // case sensitive
- **metadata**
    - dictionary
    - data about the object
    - you should define the metadata under keyword of metadata and there must be a space under keyword metadata
- **spec**

- If you run `kubectl create -f pod-definition.yaml` command,  POD will be created 

## See PODS

- `kubectl get pods`
    - after creating the PODs, with the above command, we can check the pod

- `kubectl describe pod myapp-pod`
    - to get detailed informaiton about the pod, run the above command

## IDE
- Visual Studio Code
- Need to install `YAML` extenstion by RedHat

- go to `Extention settings` ->  Click on the `Edit settings.json` of the extension

- settings.json
```yaml
{
    "yaml.schemas": {
        "kubernetes": "*.yaml"
    }
}

```

- you can use the **OUTLINE** function on the left side of VC to check the tree hierarchy


## Command

- if you're not sure about the K8s command

```console
> kubectl run --help
```

- if you want to know which nodes are the pods placed on


```shell
> kubectl get pods -o wide // see the list of available PODs with node information 
```
