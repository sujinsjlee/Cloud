# Virtual Machines and Networks in the Cloud

## Virtual Private Cloud networking
> **VPC**  


- A virtual private cloud, or VPC, is a secure, individual, private cloud-computing model hosted within a public cloud – like Google Cloud! 

- VPC networks connect Google Cloud resources to each other and to the internet. 

- They can also have subnets, which is a segmented piece of the larger network, in any Google Cloud region worldwide. Subnets can span the zones that make up a region.

## Compute Engine

- Google Cloud’s IaaS solution: Compute Engine. 
    - With Compute Engine, users can create and run virtual machines on Google infrastructure.

## Scaling virutal machines
- Compute Engine has a feature called **Autoscaling**, where VMs can be added to or subtracted from an application based on load metrics. The other part of making that work is balancing the incoming traffic among the VMs. 

- Specifications for currently available VM machine types can be found at cloud.google.com/compute/docs/machine-types

## Important VPC compatibilities
- Much like physical networks, VPCs have routing tables. VPC routing tables are built-in so you don’t have to provision or manage a router.

- Another thing you don’t have to provision or manage for Google Cloud is a **firewall**. VPCs provide a global distributed firewall, which can be controlled to restrict access to instances through both incoming and outgoing traffic. Firewall rules can be defined through network tags on Compute Engine instances, which is really convenient.

- what if your company has several Google Cloud projects, and the VPCs need to talk to each other? With **VPC Peering**, a relationship between two VPCs can be established to exchange traffic.

- Alternatively, to use the full power of Identity Access Management (IAM) to control who and what in one project can interact with a VPC in another, you can configure a **Shared VPC**.

## Cloud Load Balancing
- Cloud Load Balancing
    - The job of a load balancer is to distribute user traffic across multiple instances of an application. 

## Cloud DNS and Cloud CDN
- One of the most famous free Google services is `8.8.8.8`, which provides a public Domain Name Service to the world. 

- But what about the internet hostnames and addresses of applications built in Google Cloud? Google Cloud offers **Cloud DNS** to help the world find them. It’s a managed DNS service that runs on the same infrastructure as Google. 

- Google also has a global system of edge caches. Edge caching refers to the use of caching servers to store content closer to end users. You can use this system to accelerate content delivery in your application by using **Cloud CDN** - Content Delivery Network. 

## Connecting networks to Google VPC

- Many Google Cloud customers want to connect their Google Virtual Private Clouds to other networks in their system, such as on-premises networks or networks in other clouds. There are several effective ways to accomplish this. 

    - **IPsec VPN protocol** 
    - **Direct Peering**
    - **Carrier Peering**
    - **Dedicated Interconnect**
    - **Partner Interconnect**
