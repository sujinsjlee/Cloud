# Containers in the Cloud

## Google Kubernetes Engine

- **K8s**
    - A product that helps manage and scale containerized applications is Kubernetes
    - Kubernetes is an open-source platform for managing containerized workloads and services. It makes it easy to orchestrate many containers on many hosts, scale them as microservices, and easily deploy rollouts and rollbacks. 
    - ex: scale, rollingUpdate

## GKE 
-  GKE is a Google-hosted managed Kubernetes service in the Cloud.
- The GKE environment consists of multiple machines, specifically Compute Engine instances grouped together to form a cluster. You can create a Kubernetes cluster with Kubernetes Engine by using the Google Cloud Console or the gcloud command that's provided by the Cloud software development kit.

-  Running your replication in GKE clusters is also a good foundation to have if you'll need to bridge your on-prem and Cloud resources. To start up kubernetes is on a cluster in GKE all you do is run this command, gcloud container clusters create k1.

## Hybrid and multi-cloud
- a typical on premises **distributed systems architecture**, which is how businesses traditionally met their enterprise computing needs before Cloud computing. Most enterprise scale applications are designed as distributed systems, spreading the computing workload required to provide services over two or more network service.

- a modern hybrid or multi-cloud architecture
    - it allows you to keep parts of your system's infrastructure on-premises while moving other parts of the cloud. 
    - Take advantage of the flexibility, scalability and lower computing costs
    
## Anthos
- Anthos is a hybrid and multi-cloud solution powered by the latest innovations in distributed systems and service management software from Google. The Anthos framework rests on Kubernetes and GKE On-Prem. 
- Anthos also provides a rich set of tools for monitoring and maintaining the consistency of your applications across all of your network, whether on-premises, in the cloud, or in multiple clouds. 