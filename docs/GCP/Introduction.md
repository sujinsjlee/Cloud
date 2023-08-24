# Introducing Google Cloud

## Cloud computing

- Cloud computin is a way of using IT technology.

- Customers get access to computing resources over the internet, from anywhere they have a connection.
- The provider allocates the cloud computing resources to the users 
- Customers don't have to know or care about the exact physical location of those resources. 
- The resources are elastic–which means they’re flexible, so customers can be. If they need more resources they can get more, and quickly. If they need less, they can scale back.
- Customers pay only for what they use, or reserve as they go.
 
## Limitation of the virtualization model
 - With virtualization, enterprises still maintain the infrastructure; but it also remains a user-controlled and user-configured environment. 

## Container-based architecture
 - So Google switched to a container-based architecture—a fully automated, elastic cloud that consists of a combination of automated services and scalable data. Services automatically provision and configure the infrastructure used to run applications. 

 > *Google believes that, in the future, every company—regardless of size or industry— will differentiate itself from its competitors through technology. Increasingly, that technology will be in the form of software. Great software is based on high-quality data. This means that every company is, or will eventually become, a data company.*

## Region, Zone

> Locaion > Region > Zone

- Each of these locations is divided into several different regions and zones. 

- Regions represent independent geographic areas and are composed of zones. For example, London, or `europe-west2`, is a region that currently comprises three different zones. A zone is an area where Google Cloud resources are deployed.

- [Google Cloud 37 regions 112 zones, 187 locations](cloud.google.com/about/locations)

- **Cloud Spanner** multi-region configurations allow you to replicate the database's data not just in multiple zones, but in multiple zones across multiple regions, as defined by the instance configuration. These additional replicas enable you to read data with low latency from multiple locations close to or within the regions in the configuration, like The Netherlands, and Belgium. 

## IaaS and PaaS

- There are 3 main types of cloud computing as-a-service options and each one covers a degree of management for you: infrastructure-as-a-service (IaaS), platform-as-a-service (PaaS), and software-as-a-service (SaaS).


### IaaS

- Infrastructure-as-a-service, or IaaS, is a step away from on-premises infrastructure. It’s a pay-as-you-go service where a third party provides you with infrastructure services, like storage and virtualization, as you need them, via a cloud, through the internet. 

### PaaS

- Platform-as-a-service (PaaS) is another step further from full, on-premise infrastructure management. 

- You write the code, build, and manage your apps, but you do it without the headaches of software updates or hardware maintenance. The environment to build and deploy is provided for you. 

- In the IaaS model, customers pay for the resources they allocate ahead of time. In the PaaS model, customers pay for the resources they actually use. As cloud computing has evolved, the momentum has shifted towards managed infrastructure and managed services.

- Serverless technologies offered by google include **cloud functions**, which manages event driven code as a pay as you go service. And **Cloud Run**, which allows customers to deploy their containerized microservices based application, in a fully managed environment.

## SaaS
- Software as a service applications, aren't installed on your local computer. Instead, they run in the cloud as a service and are consumed directly over the internet by end users.
Play video starting at :2:3 and follow transcript2:03
Popular google applications such as Gmail, Docs and Drive, that are part of Google Workspace, are all examples of SaaS.

## Open source ecosystems
- Some organizations are afraid to bring their workloads to the cloud because they're afraid they'll get locked into a particular vendor. However, if, for whatever reason, a customer decides that Google is no longer the best provider for their needs, we provide them with the ability to run their applications elsewhere. 
- Google publishes key elements of technology using open source licenses to create ecosystems that provide customers with options other than Google. 

- For example, **TensorFlow**, an open source software library for machine learning developed inside Google, is at the heart of a strong open source ecosystem. 

-  Google provides interoperability at multiple layers of the stack. **Kubernetes** and **Google Kubernetes Engine** give customers the ability to mix and match microservices running across different clouds, while Google Cloud’s operations suite lets customers monitor workloads across multiple cloud providers.

## Pricing and billing
- Now, you’re probably thinking, “How can I make sure I don’t accidentally run up a big Google Cloud bill?” We provide a few tools to help. You can define **budgets** at the billing account level or at the project level. 
- **Alerts** are generally set at 50%, 90% and 100%, but can also be customized. **Reports** is a visual tool in the Google Cloud Console that allows you to monitor expenditure based on a project or services. Finally, Google Cloud also implements **quotas**, which are designed to prevent the over-consumption of resources because of an error or a malicious attack, protecting both account owners and the Google Cloud community as a whole.

- Rate Quota: Resets after a specific time
- Allocation Quota: Governs number of resources 

