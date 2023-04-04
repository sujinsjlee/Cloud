# Section 4. Logging & Monitoring

- [Monitor Cluster Components](#Monitoring)  
- [Managing Application Logs](#Logging)  

## Monitoring  
![m](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/mon.PNG)
- Kubernetes does not come with a full-featured built-in monitoring solution.
- However, there are a number of open-source solutions available today such as **Metrics Server, Prometheus, the Elastic Stack** and **proprietary solutions like Datadog and Dynatrace.**

- **The Metrics Server retrieves metrics from each of the Kubernetes nodes and pods,aggregates them, and stores them in memory.**
- Note that the Metrics Server is only an in-memory monitoring solution and does not store the metrics on the disk. And as a result, you cannot see historical performance data.

> *How are the metrics generated for the PODs on these nodes?*

![monitor](https://github.com/kodekloudhub/certified-kubernetes-administrator-course/blob/master/images/ca.PNG)



## Logging