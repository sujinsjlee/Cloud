# Section 4. Logging & Monitoring

- [Monitor Cluster Components](#Monitoring)  
- [Managing Application Logs](#Logging)  

## Monitoring  
- [www.devopsschool.com/blog/what-is-metrics-server-and-how-to-install-metrics-server/](https://www.devopsschool.com/blog/what-is-metrics-server-and-how-to-install-metrics-server/) 
![mm](https://www.devopsschool.com/blog/wp-content/uploads/2020/08/metrics-server-cadvisor.jpg)  

![m](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/mon.PNG)
- Kubernetes does not come with a full-featured built-in monitoring solution.
- However, there are a number of open-source solutions available today such as **Metrics Server, Prometheus, the Elastic Stack** and **proprietary solutions like Datadog and Dynatrace.**

- **The Metrics Server retrieves metrics from each of the Kubernetes nodes and pods,aggregates them, and stores them in memory.**
- Note that the Metrics Server is only an in-memory monitoring solution and does not store the metrics on the disk. And as a result, you cannot see historical performance data.

> *How are the metrics generated for the PODs on these nodes?*

![monitor](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/ca.PNG)

- Kubernetes runs an agent on each node known as the `kubelet`, which is responsible for receiving instructions from the Kubernetes API master server and running pods on the nodes
- The `kubelet` also contains a sub component known as the **cAdvisor** or Container Advisor. **cAdvisor** is responsible for retrieving performance metrics from pods and exposing them through the kubelet API to make the metrics available for the Metrics Server.

- Clone the metric server from github repo
  ```
  $ git clone https://github.com/kubernetes-incubator/metrics-server.git
  ```
- Deploy the metric server
  ```
  $ kubectl create -f metric-server/deploy/1.8+/
  ```
    - This command deploys a set of pods, services, and roles to enable Metrics Server to pull for performance metrics from the nodes in the cluster.

- View the cluster performance
  ```
  $ kubectl top node
  ```
- View performance metrics of pod
  ```
  $ kubectl top pod
  ```

## Logging

- To view the logs
  ```
  ~# kubectl logs -f <pod-name>
  ```
- If there are multiple containers in a pod then you must specify the name of the container explicitly in the command.
  ```
  ~# kubectl logs -f <pod-name> <container-name>
  ~# kubectl logs -f even-simulator-pod event-simulator
  ```