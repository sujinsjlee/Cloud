# PODs

> [K8s Concept](https://kubernetes.io/docs/concepts/)  
> [POD Overview](https://kubernetes.io/docs/concepts/workloads/pods/)

- kubernetes does not deploy containers directly on the worker nodes. 
- The containers are encapsulated into a Kubernetes object known as PODs. 
    - A POD is a single instance of an application. A POD is the smallest object, that you can create in kubernetes.

- **PODs usually have a one-to-one relationship with containers running your application.** 
    - To scale UP you create new PODs and to scale down you delete PODs. 
    - You do not add additional containers to an existing POD to scale your application.

- **kubectl**

    ```shell
    > kubectl run nginx --image nginx // get the docker image by specifying image parameter  

    > kubectl get pods // see the lists of availabe PODs  

    > kubectl describe pod nginx // get detail information of the POD  

    > kubectl get pods -o wide // see the list of available PODs with ip information 
    ```

    - cf) AGE shown for a pod when using kubectl get pod , shows the time that the pod has been running since the last restart.