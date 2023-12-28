# Google Associate Cloud Engineer

- [Sample QnA](https://docs.google.com/forms/d/e/1FAIpQLSfexWKtXT2OSFJ-obA4iT3GmzgiOCGvjrT9OfxilWC1yPtmfQ/viewform)

- [Examtopics](https://www.examtopics.com/exams/google/associate-cloud-engineer/view/)
<!--

Q2

You need to create a custom VPC with a single subnet. The subnet's range must be as large as possible. Which range should you use?
A. 0.0.0.0/0
B. 10.0.0.0/8
C. 172.16.0.0/12
D. 192.168.0.0/16

B

In Google Cloud Platform (GCP), when creating a VPC network, you should use the IP ranges that are reserved for private networks as defined by the RFC 1918. Here are the private IP address ranges defined by RFC 1918:

10.0.0.0 to 10.255.255.255 (10.0.0.0/8)
172.16.0.0 to 172.31.255.255 (172.16.0.0/12)
192.168.0.0 to 192.168.255.255 (192.168.0.0/16)
From the provided options:

A. 0.0.0.0/0: This is not a private IP address range. It represents all possible IP addresses.

B. 10.0.0.0/8: This is a private IP range that covers all IP addresses from 10.0.0.0 to 10.255.255.255. It's the largest range among the options.

C. 172.16.0.0/12: This is a private IP range, but it's smaller than 10.0.0.0/8.

D. 192.168.0.0/16: This is also a private IP range, but it's smaller than both B and C.

So, if you want the subnet's range to be as large as possible:

The correct answer is B. 10.0.0.0/8.


Q3

You want to select and configure a cost-effective solution for relational data on Google Cloud Platform. You are working with a small set of operational data in one geographic location. You need to support point-in-time recovery. What should you do?
A. Select Cloud SQL (MySQL). Verify that the enable binary logging option is selected.
B. Select Cloud SQL (MySQL). Select the create failover replicas option.
C. Select Cloud Spanner. Set up your instance with 2 nodes.
D. Select Cloud Spanner. Set up your instance as multi-regional.

A

특정 시점 복구를 사용하려면 바이너리 로깅을 활성화해야 합니다. 

바이너리 로깅을 활성화하면 쓰기 성능이 약간 저하됩니다.

B가 적합하지 않은 이유는 Failover replicas는 특정 시점을 기록하지 않고 복구를 위해 마지막 상태만 백업하기 때문입니다. Cloud Spanner는 대규모 데이터 세트에 적합하므로 C, D가 올바르지 않습니다.


Q4

You want to configure autohealing for network load balancing for a group of Compute Engine instances that run in multiple zones, using the fewest possible steps.
You need to configure re-creation of VMs if they are unresponsive after 3 attempts of 10 seconds each. What should you do?
A. Create an HTTP load balancer with a backend configuration that references an existing instance group. Set the health check to healthy (HTTP)
B. Create an HTTP load balancer with a backend configuration that references an existing instance group. Define a balancing mode and set the maximum RPS to 10.
C. Create a managed instance group. Set the Autohealing health check to healthy (HTTP)
D. Create a managed instance group. Verify that the autoscaling setting is on.

C

A, B에 해당하는 로드 밸런서 상태 확인은 VM을 중지하고 새 VM을 회전시키지 않으며 트래픽을 정상 VM으로 만 리디렉션합니다. 특히 B는 RPS가 균형을 위해 VM에 대한 초당 요청은 치유와 관련 없음

또한, 자동 확장이 관련이 없으므로 D는 잘못되었습니다.


Q5

You are using multiple configurations for gcloud. You want to review the configured Kubernetes Engine cluster of an inactive configuration using the fewest possible steps. What should you do?
A. Use gcloud config configurations describe to review the output.
B. Use gcloud config configurations activate and gcloud config list to review the output.
C. Use kubectl config get-contexts to review the output.
D. Use kubectl config use-context and kubectl config view to review the output.

D

Answer A: Using `gcloud config configurations described` will only show you the details of the current configuration, not the Kubernetes Engine cluster of an inactive configuration.

Answer B: Using `gcloud config configurations activate` and `gcloud config list` to review the output will only show you the list of configurations and activate one of them, but it won't provide you with the details of the Kubernetes Engine cluster of an inactive configuration.

Answer C: Using `kubectl config get-contexts` will only list the available contexts, including their clusters, but it won't provide you with the details of the Kubernetes Engine cluster of an inactive configuration.


Q9

You are deploying an application to App Engine. You want the number of instances to scale based on request rate. You need at least 3 unoccupied instances at all times. Which scaling type should you use?
A. Manual Scaling with 3 instances.
B. Basic Scaling with min_instances set to 3.
C. Basic Scaling with max_instances set to 3.
D. Automatic Scaling with min_idle_instances set to 3.

D

Q11
You need a dynamic way of provisioning VMs on Compute Engine. The exact specifications will be in a dedicated configuration file. You want to follow Google's recommended practices. Which method should you use?
A. Deployment Manager
B. Cloud Composer
C. Managed Instance Group
D. Unmanaged Instance Group

A

Deployment Manager is a configuration management tool that allows you to define and deploy a set of resources, including Compute Engine VMs, in a declarative manner. 

Managed Instance Group (option C) and Unmanaged Instance Group (option D) are Compute Engine features that allow you to group related VM instances and manage them as a single entity. However, they do not provide a dynamic way of provisioning VMs based on a configuration file like Deployment Manager does.

Managed Instance Groups (MIG) and Unmanaged Instance Groups can be used for managing groups of identical instances, but they might not be as suitable for dynamic provisioning based on a configuration file, as they are designed for maintaining a specified number of instances in a group.


Q13
You have a Dockerfile that you need to deploy on Kubernetes Engine. What should you do?
A. Use kubectl app deploy <dockerfilename>.
B. Use gcloud app deploy <dockerfilename>.
C. Create a docker image from the Dockerfile and upload it to Container Registry. Create a Deployment YAML file to point to that image. Use kubectl to create the deployment with that file.
D. Create a docker image from the Dockerfile and upload it to Cloud Storage. Create a Deployment YAML file to point to that image. Use kubectl to create the deployment with that file.

C
도커파일에서 도커 이미지를 만들고 컨테이너 레지스트에 업로드 후 해당 이미지를 가리키는 배포 YAML 파일을 만들어야 함, 또한 kubectl을 사용하여 GKE로 푸시 처리함

참고로 컨테이너 레지스트리는 도커 파일이 저장되는 곳으로 GKE, Cloud Run 또는

App Engine flexible에 배포 가능함

A의 경우는 도커 파일만 언급되었고 컨테이너 이미지가 없어서 불가함


Q20
You created an instance of SQL Server 2017 on Compute Engine to test features in the new version. You want to connect to this instance using the fewest number of steps. What should you do?
A. Install a RDP client on your desktop. Verify that a firewall rule for port 3389 exists.
B. Install a RDP client in your desktop. Set a Windows username and password in the GCP Console. Use the credentials to log in to the instance.
C. Set a Windows password in the GCP Console. Verify that a firewall rule for port 22 exists. Click the RDP button in the GCP Console and supply the credentials to log in.
D. Set a Windows username and password in the GCP Console. Verify that a firewall rule for port 3389 exists. Click the RDP button in the GCP Console, and supply the credentials to log in.

D
RDP 클릭 시 사용자 이름과 암호를 묻는 메시지가 표시되므로
이러한 자격 증명을 설정하고 포트가 열려 있는지 확인 후 연결을 해야 함
RDP 클라이언트를 다운로드해도 콘솔에서 RDP 파일 다운로드가 필요하여 수행 단계가 복잡함

* Remote Desktop Protocol (원격 데스크톱 프로토콜)

Q21
You have one GCP account running in your default region and zone and another account running in a non-default region and zone. You want to start a new
Compute Engine instance in these two Google Cloud Platform accounts using the command line interface. What should you do?
A. Create two configurations using gcloud config configurations create [NAME]. Run gcloud config configurations activate [NAME] to switch between accounts when running the commands to start the Compute Engine instances.
B. Create two configurations using gcloud config configurations create [NAME]. Run gcloud configurations list to start the Compute Engine instances.
C. Activate two configurations using gcloud configurations activate [NAME]. Run gcloud config list to start the Compute Engine instances.
D. Activate two configurations using gcloud configurations activate [NAME]. Run gcloud configurations list to start the Compute Engine instances.

A

두 개의 컨피그를 생성 후 원하는 구성을 활성화하는 것이 맞음

또한 gcloud config list 명령은 현재 활성 구성에 대한 속성을 나열할 뿐 컨피그를 시작하지 않으며

gcloud config configurations create [NAME] 명령어를 사용하는 것이 맞음


Q23
You are building a pipeline to process time-series data. Which Google Cloud Platform services should you put in boxes 1,2,3, and 4?

A. Cloud Pub/Sub, Cloud Dataflow, Cloud Datastore, BigQuery
B. Firebase Messages, Cloud Pub/Sub, Cloud Spanner, BigQuery
C. Cloud Pub/Sub, Cloud Storage, BigQuery, Cloud Bigtable
D. Cloud Pub/Sub, Cloud Dataflow, Cloud Bigtable, BigQuery

D

BigTable은 time-series data 스토리지에 적합함


Q24
You have a project for your App Engine application that serves a development environment. The required testing has succeeded and you want to create a new project to serve as your production environment. What should you do?
A. Use gcloud to create the new project, and then deploy your application to the new project.
B. Use gcloud to create the new project and to copy the deployed application to the new project.
C. Create a Deployment Manager configuration file that copies the current App Engine deployment into a new project.
D. Deploy your application again using gcloud and specify the project parameter with the new project name to create the new project.

A

Option B is wrong as the option to use gcloud app cp does not exist.
Option C is wrong as Deployment Manager does not copy the application, but allows you to specify all the resources needed for your application in a declarative format using yaml


Q27
You have sensitive data stored in three Cloud Storage buckets and have enabled data access logging. You want to verify activities for a particular user for these buckets, using the fewest possible steps. You need to verify the addition of metadata labels and which files have been viewed from those buckets. What should you do?
A. Using the GCP Console, filter the Activity log to view the information.
B. Using the GCP Console, filter the Stackdriver log to view the information.
C. View the bucket in the Storage section of the GCP Console.
D. Create a trace in Stackdriver to view the information.

A

활동 로깅은 앱 사용자의 요청을 참조하기 때문

활동 로그와 감사 로그는 모두 stackdriver 로그이나 활동 로그는 사용자의 요청을 참조하고

감사 로그는 관리자의 요청을 참조함. 질문은 특정 사용자의 활동을 확인하고 싶다고 함


Q28
You are the project owner of a GCP project and want to delegate control to colleagues to manage buckets and files in Cloud Storage. You want to follow Google- recommended practices. Which IAM roles should you grant your colleagues?

A. Project Editor
B. Storage Admin
C. Storage Object Admin
D. Storage Object Creator

​

정답 : B

스토리지 관리자는 버킷 및 객체에 대한 모든 권한을 부여함
delegate를 통해 colleagues(동료)에게 권한을 위임한다는 문제가 핵심
(C) doesn't have control over the buckets.

Q29

You have an object in a Cloud Storage bucket that you want to share with an external company. The object contains sensitive data. You want access to the content to be removed after four hours. The external company does not have a Google account to which you can grant specific user-based access privileges. You want to use the most secure method that requires the fewest steps. What should you do?
A. Create a signed URL with a four-hour expiration and share the URL with the company.
B. Set object access to 'public' and use object lifecycle management to remove the object after four hours.
C. Configure the storage bucket as a static website and furnish the object's URL to the company. Delete the object from the storage bucket after four hours.
D. Create a new Cloud Storage bucket specifically for the external company to access. Copy the object to that bucket. Delete the bucket after four hours have passed.

A
Signed URL 사용하면 시간제한을 둘 수 있음
Answer C is incorrect because configuring the storage bucket as a static website would make the entire bucket contents available publicly, which is not secure.

Q30
You are creating a Google Kubernetes Engine (GKE) cluster with a cluster autoscaler feature enabled. You need to make sure that each node of the cluster will run a monitoring pod that sends container metrics to a third-party monitoring solution. What should you do?
A. Deploy the monitoring pod in a StatefulSet object.
B. Deploy the monitoring pod in a DaemonSet object.
C. Reference the monitoring pod in a Deployment object.
D. Reference the monitoring pod in a cluster initializer at the GKE cluster creation time.

B

Daemonset 개체에 모니터링 pod을 배포하면 모든 컨테니어 측정 항목을 내보낼 수 있음

DaemonSets ensure that one running instance of the pod is scheduled on every node in the cluster. This means that even if the cluster autoscaler scales the cluster up or down, the monitoring pod will continue to run on each node.


Q31
You want to send and consume Cloud Pub/Sub messages from your App Engine application. The Cloud Pub/Sub API is currently disabled. You will use a service account to authenticate your application to the API. You want to make sure your application can use Cloud Pub/Sub. What should you do?
A. Enable the Cloud Pub/Sub API in the API Library on the GCP Console.
B. Rely on the automatic enablement of the Cloud Pub/Sub API when the Service Account accesses it.
C. Use Deployment Manager to deploy your application. Rely on the automatic enablement of all APIs used by the application being deployed.
D. Grant the App Engine Default service account the role of Cloud Pub/Sub Admin. Have your application enable the API on the first connection to Cloud Pub/ Sub.

A
프로젝트에 Pub/Sub API 사용 설정을 먼저 활성화해야 함

Q34
You want to verify the IAM users and roles assigned within a GCP project named my-project. What should you do?
A. Run gcloud iam roles list. Review the output section.
B. Run gcloud iam service-accounts list. Review the output section.
C. Navigate to the project and then to the IAM section in the GCP Console. Review the members and roles.
D. Navigate to the project and then to the Roles section in the GCP Console. Review the roles and status.

C

프로젝트 선택 후 IAM 클릭 후 사용자와 역할을 확인해야 돼서 C가 정답

A는 역할에 대한 정보만 제공해서 오답, B는 서비스 계정만 제공해서 오답

D는 IAM 내부에 있는 내용이라서 오답

To verify the IAM users and roles assigned within a GCP project, you can navigate to the project and then to the IAM section in the GCP Console. In the IAM section, you can review the members and roles assigned to the project. This will allow you to see who has what level of access to the project resources.

Answer A is incorrect because it lists the roles available in the project, but it does not show the IAM users and roles assigned to those roles.

Answer B is incorrect because it lists the service accounts in the project, but it does not show the IAM users and roles assigned to those service accounts.

Answer D is incorrect because it lists the roles available in the project, but it does not show the IAM users and roles assigned to those roles.

Q37
You created a Google Cloud Platform project with an App Engine application inside the project. You initially configured the application to be served from the us- central region. Now you want the application to be served from the asia-northeast1 region. What should you do?
A. Change the default region property setting in the existing GCP project to asia-northeast1.
B. Change the region property setting in the existing App Engine application from us-central to asia-northeast1.
C. Create a second App Engine application in the existing GCP project and specify asia-northeast1 as the region to serve your application.
D. Create a new GCP project and create an App Engine application inside this new project. Specify asia-northeast1 as the region to serve your application.

D

프로젝트 내에 App Engine 애플리케이션이 하나만 있을 수 있기 때문
C는 GCP에 두 개의 앱 엔진 애플리케이션이 있을 수 없기 때문에 오답
또한, 생성된 후 App Engine 애플리케이션의 위치를 변경할 수 없기 때문에 A, B는 오답

Q40
You have an instance group that you want to load balance. You want the load balancer to terminate the client SSL session. The instance group is used to serve a public web application over HTTPS. You want to follow Google-recommended practices. What should you do?
A. Configure an HTTP(S) load balancer.
B. Configure an internal TCP load balancer.
C. Configure an external SSL proxy load balancer.
D. Configure an external TCP proxy load balancer.

A

Google recommends using an HTTP(S) load balancer for terminating SSL sessions and load-balancing traffic to an instance group serving a public web application over HTTPS.

Q41

You have 32 GB of data in a single file that you need to upload to a Nearline Storage bucket. The WAN connection you are using is rated at 1 Gbps, and you are the only one on the connection. You want to use as much of the rated 1 Gbps as possible to transfer the file rapidly. How should you upload the file?
A. Use the GCP Console to transfer the file instead of gsutil.
B. Enable parallel composite uploads using gsutil on the file transfer.
C. Decrease the TCP window size on the machine initiating the transfer.
D. Change the storage class of the bucket from Nearline to Multi-Regional.

B

Correct answer is B as the bandwidth is good and its a single file, gsutil parallel composite uploads can be used to split the large file and upload in parallel.Refer GCP documentation

Q47
You want to configure 10 Compute Engine instances for availability when maintenance occurs. Your requirements state that these instances should attempt to automatically restart if they crash. Also, the instances should be highly available including during system maintenance. What should you do?
A. Create an instance template for the instances. Set the 'Automatic Restart' to on. Set the 'On-host maintenance' to Migrate VM instance. Add the instance template to an instance group.
B. Create an instance template for the instances. Set 'Automatic Restart' to off. Set 'On-host maintenance' to Terminate VM instances. Add the instance template to an instance group.
C. Create an instance group for the instances. Set the 'Autohealing' health check to healthy (HTTP).
D. Create an instance group for the instance. Verify that the 'Advanced creation options' setting for 'do not retry machine creation' is set to off.

A

Q48
C. Set Content-Type metadata to application/pdf on the PDF file objects.


Q49
You have a virtual machine that is currently configured with 2 vCPUs and 4 GB of memory. It is running out of memory. You want to upgrade the virtual machine to have 8 GB of memory. What should you do?
A. Rely on live migration to move the workload to a machine with more memory.
B. Use gcloud to add metadata to the VM. Set the key to required-memory-size and the value to 8 GB.
C. Stop the VM, change the machine type to n1-standard-8, and start the VM.
D. Stop the VM, increase the memory to 8 GB, and start the VM.

D

n1-standard-8은 8GB 메모리가 아니라 8개의 CPU 사용을 의미함

VM 인스턴스를 중지하지 않고 메모리 증설은 불가함

Q50
You have production and test workloads that you want to deploy on Compute Engine. Production VMs need to be in a different subnet than the test VMs. All the
VMs must be able to reach each other over Internal IP without creating additional routes. You need to set up VPC and the 2 subnets. Which configuration meets these requirements?
A. Create a single custom VPC with 2 subnets. Create each subnet in a different region and with a different CIDR range.
B. Create a single custom VPC with 2 subnets. Create each subnet in the same region and with the same CIDR range.
C. Create 2 custom VPCs, each with a single subnet. Create each subnet in a different region and with a different CIDR range.
D. Create 2 custom VPCs, each with a single subnet. Create each subnet in the same region and with the same CIDR range.

A
동일한 지역에서 같은 CIDR IP 범위를 가질 수 없기 때문에 B는 답이 아님

서브넷의 기본 및 보조 범위는 할당 된 범위, 동일한 네트워크에 있는 

다른 서브넷의 기본 또는 보조 범위와 겹칠 수 없음. 

또한, VPC는 본질적으로 글로벌이며, 서브넷은 지역적이지만 전 세계적으로 연결됨

그래서 서로 다른 서브넷에 대해 1개의 VPC와 서로 다른 CIDR을 만드는 것이 목적으로 충분함


Q52
Your company has a Google Cloud Platform project that uses BigQuery for data warehousing. Your data science team changes frequently and has few members.
You need to allow members of this team to perform queries. You want to follow Google-recommended practices. What should you do?
A. 1. Create an IAM entry for each data scientist's user account. 2. Assign the BigQuery jobUser role to the group.
B. 1. Create an IAM entry for each data scientist's user account. 2. Assign the BigQuery dataViewer user role to the group.
C. 1. Create a dedicated Google group in Cloud Identity. 2. Add each data scientist's user account to the group. 3. Assign the BigQuery jobUser role to the group.
D. 1. Create a dedicated Google group in Cloud Identity. 2. Add each data scientist's user account to the group. 3. Assign the BigQuery dataViewer user role to the group.

C

dataViewer는 사용자가 뷰에서 쿼리를 수행하고 데이터를 가져올 수 없어 B, D는 오답

Q53
A. 1. Create an ingress firewall rule with the following settings: ג€¢ Targets: all instances ג€¢ Source filter: IP ranges (with the range set to 10.0.2.0/24) ג€¢ Protocols: allow all 2. Create an ingress firewall rule with the following settings: ג€¢ Targets: all instances ג€¢ Source filter: IP ranges (with the range set to 10.0.1.0/24) ג€¢ Protocols: allow all
B. 1. Create an ingress firewall rule with the following settings: ג€¢ Targets: all instances with tier #2 service account ג€¢ Source filter: all instances with tier #1 service account ג€¢ Protocols: allow TCP:8080 2. Create an ingress firewall rule with the following settings: ג€¢ Targets: all instances with tier #3 service account ג€¢ Source filter: all instances with tier #2 service account ג€¢ Protocols: allow TCP: 8080
C. 1. Create an ingress firewall rule with the following settings: ג€¢ Targets: all instances with tier #2 service account ג€¢ Source filter: all instances with tier #1 service account ג€¢ Protocols: allow all 2. Create an ingress firewall rule with the following settings: ג€¢ Targets: all instances with tier #3 service account ג€¢ Source filter: all instances with tier #2 service account ג€¢ Protocols: allow all
D. 1. Create an egress firewall rule with the following settings: ג€¢ Targets: all instances ג€¢ Source filter: IP ranges (with the range set to 10.0.2.0/24) ג€¢ Protocols: allow TCP: 8080 2. Create an egress firewall rule with the following settings: ג€¢ Targets: all instances ג€¢ Source filter: IP ranges (with the range set to 10.0.1.0/24) ג€¢ Protocols: allow TCP: 8080

B

This question is designed to waste your time during the exam by making you read all those long answers. Remember that part of exam technique is not about knowing the product at all, but understanding multiple choice questions.

For example when two answers are very similar to each other, this can increase the likelihood that the correct answer is one of those two.

In this case it's an easy process of elimination as all answers are similar, we just need to filter out the wrong ones (and whacking the wrong answer in an exam is sometimes the best way to find the right one).

Two answers mention port 8080, and two mention all ports. Obviously we just need port 8080, so we can immediately eliminate those two questions that want all ports open. That gives us a 50/50 chance of getting this question right.

Of the remaining answers, one says "ingress" and the other "egress". We know that by default egress is permitted and ingress is not, so that makes "b" the only surviving choice.

Q54
You are given a project with a single Virtual Private Cloud (VPC) and a single subnetwork in the us-central1 region. There is a Compute Engine instance hosting an application in this subnetwork. You need to deploy a new instance in the same project in the europe-west1 region. This new instance needs access to the application. You want to follow Google-recommended practices. What should you do?
A. 1. Create a subnetwork in the same VPC, in europe-west1. 2. Create the new instance in the new subnetwork and use the first instance's private address as the endpoint.
B. 1. Create a VPC and a subnetwork in europe-west1. 2. Expose the application with an internal load balancer. 3. Create the new instance in the new subnetwork and use the load balancer's address as the endpoint.
C. 1. Create a subnetwork in the same VPC, in europe-west1. 2. Use Cloud VPN to connect the two subnetworks. 3. Create the new instance in the new subnetwork and use the first instance's private address as the endpoint.
D. 1. Create a VPC and a subnetwork in europe-west1. 2. Peer the 2 VPCs. 3. Create the new instance in the new subnetwork and use the first instance's private address as the endpoint.

A

ANSWER A is the correct answer because it follows Google's recommended practices of using a single VPC per project and creating a new subnetwork in the same VPC in the europe-west1 region. This allows the new instance to communicate with the existing instance using its private IP address as the endpoint.
ANSWER D is incorrect because VPC peering only works between VPCs in the same region, so it would not be possible to peer the existing VPC in us-central1 with a new VPC in europe-west1.

Q56

You have a website hosted on App Engine standard environment. You want 1% of your users to see a new test version of the website. You want to minimize complexity. What should you do?
A. Deploy the new version in the same application and use the --migrate option.
B. Deploy the new version in the same application and use the --splits option to give a weight of 99 to the current version and a weight of 1 to the new version.
C. Create a new App Engine application in the same project. Deploy the new version in that application. Use the App Engine library to proxy 1% of the requests to the new version.
D. Create a new App Engine application in the same project. Deploy the new version in that application. Configure your network load balancer to send 1% of the traffic to that new application.

B

--splits는 각 버전으로 이동해야 하는 트래픽 비율을 설명하는 키-값 쌍임

분할된 값이 함께 더해져 가중치로 사용됨

Q59
You are the organization and billing administrator for your company. The engineering team has the Project Creator role on the organization. You do not want the engineering team to be able to link projects to the billing account. Only the finance team should be able to link a project to a billing account, but they should not be able to make any other changes to projects. What should you do?
A. Assign the finance team only the Billing Account User role on the billing account.
B. Assign the engineering team only the Billing Account User role on the billing account.
C. Assign the finance team the Billing Account User role on the billing account and the Project Billing Manager role on the organization.
D. Assign the engineering team the Billing Account User role on the billing account and the Project Billing Manager role on the organization.

you can't link project to billing accounts without Project billing manager.
C is Correct


Q60
You have an application running in Google Kubernetes Engine (GKE) with cluster autoscaling enabled. The application exposes a TCP endpoint. There are several replicas of this application. You have a Compute Engine instance in the same region, but in another Virtual Private Cloud (VPC), called gce-network, that has no overlapping IP ranges with the first VPC. This instance needs to connect to the application on GKE. You want to minimize effort. What should you do?
A. 1. In GKE, create a Service of type LoadBalancer that uses the application's Pods as backend. 2. Set the service's externalTrafficPolicy to Cluster. 3. Configure the Compute Engine instance to use the address of the load balancer that has been created.
B. 1. In GKE, create a Service of type NodePort that uses the application's Pods as backend. 2. Create a Compute Engine instance called proxy with 2 network interfaces, one in each VPC. 3. Use iptables on this instance to forward traffic from gce-network to the GKE nodes. 4. Configure the Compute Engine instance to use the address of proxy in gce-network as endpoint.
C. 1. In GKE, create a Service of type LoadBalancer that uses the application's Pods as backend. 2. Add an annotation to this service: cloud.google.com/load-balancer-type: Internal 3. Peer the two VPCs together. 4. Configure the Compute Engine instance to use the address of the load balancer that has been created.
D. 1. In GKE, create a Service of type LoadBalancer that uses the application's Pods as backend. 2. Add a Cloud Armor Security Policy to the load balancer that whitelists the internal IPs of the MIG's instances. 3. Configure the Compute Engine instance to use the address of the load balancer that has been created.

C
둘 다 다른 VPC에 있기 때문에 VPC 피어링이 필요하며

A는 최소한의 노력이지만, 애플리케이션을 인터넷에 노출해야 되므로 추천하지 않는 방식

Q62
You want to run a single caching HTTP reverse proxy on GCP for a latency-sensitive website. This specific reverse proxy consumes almost no CPU. You want to have a 30-GB in-memory cache, and need an additional 2 GB of memory for the rest of the processes. You want to minimize cost. How should you run this reverse proxy?
A. Create a Cloud Memorystore for Redis instance with 32-GB capacity.
B. Run it on Compute Engine, and choose a custom instance type with 6 vCPUs and 32 GB of memory.
C. Package it in a container image, and run it on Kubernetes Engine, using n1-standard-32 instances as nodes.
D. Run it on Compute Engine, choose the instance type n1-standard-1, and add an SSD persistent disk of 32 GB.

A

ANSWER A is the most cost-effective solution for running a caching HTTP reverse proxy on GCP. Cloud Memorystore for Redis is a managed service that provides an in-memory cache for your applications. It offers a high throughput and low latency access to the Redis protocol. Cloud Memorystore offers an SLA of 99.9% availability and automatic failover for Redis instances. In this case, a 32-GB Redis instance is sufficient to accommodate the 30-GB cache and the additional 2 GB of memory required for the rest of the processes. This solution is highly scalable and allows you to increase the size of the Redis instance as your needs grow.

Answer D is wrong
This option involves using a general-purpose Compute Engine instance type with a relatively small amount of memory (n1-standard-1).
Adding an SSD persistent disk of 32 GB can be more expensive compared to using Cloud Memorystore for Redis.
While it's possible to achieve the desired configuration with Compute Engine, it might be overprovisioned in terms of CPU resources (n1-standard-1 has 1 vCPU).

Q63

You are hosting an application on bare-metal servers in your own data center. The application needs access to Cloud Storage. However, security policies prevent the servers hosting the application from having public IP addresses or access to the internet. You want to follow Google-recommended practices to provide the application with access to Cloud Storage. What should you do?
A. 1. Use nslookup to get the IP address for storage.googleapis.com. 2. Negotiate with the security team to be able to give a public IP address to the servers. 3. Only allow egress traffic from those servers to the IP addresses for storage.googleapis.com.
B. 1. Using Cloud VPN, create a VPN tunnel to a Virtual Private Cloud (VPC) in Google Cloud. 2. In this VPC, create a Compute Engine instance and install the Squid proxy server on this instance. 3. Configure your servers to use that instance as a proxy to access Cloud Storage.
C. 1. Use Migrate for Compute Engine (formerly known as Velostrata) to migrate those servers to Compute Engine. 2. Create an internal load balancer (ILB) that uses storage.googleapis.com as backend. 3. Configure your new instances to use this ILB as proxy.
D. 1. Using Cloud VPN or Interconnect, create a tunnel to a VPC in Google Cloud. 2. Use Cloud Router to create a custom route advertisement for 199.36.153.4/30. Announce that network to your on-premises network through the VPN tunnel. 3. In your on-premises network, configure your DNS server to resolve *.googleapis.com as a CNAME to restricted.googleapis.com.

D

Google Cloud Interconnect is a service that provides a dedicated and high-bandwidth connection between your on-premises network and Google Cloud.

Here's the rationale for choosing Option D:

Secure Connection with VPN/Interconnect: By using Cloud VPN or Interconnect, you establish a secure connection between your on-premises network and a Virtual Private Cloud (VPC) in Google Cloud. This ensures encrypted communication between your on-premises servers and Google Cloud.

Custom Route Advertisement: Cloud Router allows you to create custom route advertisements. By advertising a specific network (199.36.153.4/30), you can control the routing of traffic from your on-premises network to Google Cloud.

DNS Resolution: By configuring your DNS server to resolve *.googleapis.com as a CNAME to restricted.googleapis.com, you effectively redirect DNS requests for Cloud Storage to a restricted domain. This helps in adhering to security policies that prevent direct access to storage.googleapis.com.

Options A, B, and C are less suitable for the described scenario:

Option A: Giving public IP addresses to servers would violate the security policy. Restricting egress traffic to specific IP addresses may be challenging due to the dynamic nature of Google Cloud services.

Option B: While using a proxy is a common practice, it introduces an additional layer and may not be necessary in this scenario. It might also add complexity and latency to the data access.

Option C: Migrating servers to Compute Engine and using an internal load balancer for Cloud Storage access is a significant change that might not be necessary and could involve more complexity than needed.

Q64
You want to deploy an application on Cloud Run that processes messages from a Cloud Pub/Sub topic. You want to follow Google-recommended practices. What should you do?
A. 1. Create a Cloud Function that uses a Cloud Pub/Sub trigger on that topic. 2. Call your application on Cloud Run from the Cloud Function for every message.
B. 1. Grant the Pub/Sub Subscriber role to the service account used by Cloud Run. 2. Create a Cloud Pub/Sub subscription for that topic. 3. Make your application pull messages from that subscription.
C. 1. Create a service account. 2. Give the Cloud Run Invoker role to that service account for your Cloud Run application. 3. Create a Cloud Pub/Sub subscription that uses that service account and uses your Cloud Run application as the push endpoint.
D. 1. Deploy your application on Cloud Run on GKE with the connectivity set to Internal. 2. Create a Cloud Pub/Sub subscription for that topic. 3. In the same Google Kubernetes Engine cluster as your application, deploy a container that takes the messages and sends them to your application.

C

Q65
You need to deploy an application, which is packaged in a container image, in a new project. The application exposes an HTTP endpoint and receives very few requests per day. You want to minimize costs. What should you do?
A. Deploy the container on Cloud Run.
B. Deploy the container on Cloud Run on GKE.
C. Deploy the container on App Engine Flexible.
D. Deploy the container on GKE with cluster autoscaling and horizontal pod autoscaling enabled.

A

CloudRun 서비스는 HTTP 엔드 포인트를 노출하며, 하루에 거의 요청이 없기 때문에 GKE까지는 필요 없음
또한, CloudRun은 트래픽에 따라 즉시 0에서 자동으로 확장 및 축소되며
사용하는 정확한 리소스에 대해서만 비용을 청구함

A, as it does not include the infra services and its cheaper

Cloud Run
Cloud Run is a fully managed compute platform provided by Google Cloud that automatically scales your containerized applications. It is designed to abstract away the underlying infrastructure and allows developers to focus on building and deploying applications without managing the infrastructure.

Q66

Your company has an existing GCP organization with hundreds of projects and a billing account. Your company recently acquired another company that also has hundreds of projects and its own billing account. You would like to consolidate all GCP costs of both GCP organizations onto a single invoice. You would like to consolidate all costs as of tomorrow. What should you do?
A. Link the acquired company's projects to your company's billing account.
B. Configure the acquired company's billing account and your company's billing account to export the billing data into the same BigQuery dataset.
C. Migrate the acquired company's projects into your company's GCP organization. Link the migrated projects to your company's billing account.
D. Create a new GCP organization and a new billing account. Migrate the acquired company's projects and your company's projects into the new GCP organization and link the projects to the new billing account.

A

A is correct because linking all projects of the acquired organization to the main organization’s billing account will generate a single bill for all projects.
D is incorrect because there is no need to create a new organization for this.

Q67

You built an application on Google Cloud that uses Cloud Spanner. Your support team needs to monitor the environment but should not have access to table data.
You need a streamlined solution to grant the correct permissions to your support team, and you want to follow Google-recommended practices. What should you do?
A. Add the support team group to the roles/monitoring.viewer role
B. Add the support team group to the roles/spanner.databaseUser role.
C. Add the support team group to the roles/spanner.databaseReader role.
D. Add the support team group to the roles/stackdriver.accounts.viewer role.

A

Q68
For analysis purposes, you need to send all the logs from all of your Compute Engine instances to a BigQuery dataset called platform-logs. You have already installed the Cloud Logging agent on all the instances. You want to minimize cost. What should you do?
A. 1. Give the BigQuery Data Editor role on the platform-logs dataset to the service accounts used by your instances. 2. Update your instances' metadata to add the following value: logs-destination: bq://platform-logs.
B. 1. In Cloud Logging, create a logs export with a Cloud Pub/Sub topic called logs as a sink. 2. Create a Cloud Function that is triggered by messages in the logs topic. 3. Configure that Cloud Function to drop logs that are not from Compute Engine and to insert Compute Engine logs in the platform-logs dataset.
C. 1. In Cloud Logging, create a filter to view only Compute Engine logs. 2. Click Create Export. 3. Choose BigQuery as Sink Service, and the platform-logs dataset as Sink Destination.
D. 1. Create a Cloud Function that has the BigQuery User role on the platform-logs dataset. 2. Configure this Cloud Function to create a BigQuery Job that executes this query: INSERT INTO dataset.platform-logs (timestamp, log) SELECT timestamp, log FROM compute.logs WHERE timestamp > DATE_SUB(CURRENT_DATE(), INTERVAL 1 DAY) 3. Use Cloud Scheduler to trigger this Cloud Function once a day.

C

Q69
You are using Deployment Manager to create a Google Kubernetes Engine cluster. Using the same Deployment Manager deployment, you also want to create a
DaemonSet in the kube-system namespace of the cluster. You want a solution that uses the fewest possible services. What should you do?
A. Add the cluster's API as a new Type Provider in Deployment Manager, and use the new type to create the DaemonSet.
B. Use the Deployment Manager Runtime Configurator to create a new Config resource that contains the DaemonSet definition.
C. With Deployment Manager, create a Compute Engine instance with a startup script that uses kubectl to create the DaemonSet.
D. In the cluster's definition in Deployment Manager, add a metadata that has kube-system as key and the DaemonSet manifest as value.

A

Q70
You are building an application that will run in your data center. The application will use Google Cloud Platform (GCP) services like AutoML. You created a service account that has appropriate access to AutoML. You need to enable authentication to the APIs from your on-premises environment. What should you do?
A. Use service account credentials in your on-premises application.
B. Use gcloud to create a key file for the service account that has appropriate permissions.
C. Set up direct interconnect between your data center and Google Cloud Platform to enable authentication for your on-premises applications.
D. Go to the IAM & admin console, grant a user account permissions similar to the service account permissions, and use this user account for authentication from your data center.

A

B resolve the requirement, and yes, C is a prerequisite for B, but only C doesn't solve the scenery.

Q73
You are setting up a Windows VM on Compute Engine and want to make sure you can log in to the VM via RDP. What should you do?
A. After the VM has been created, use your Google Account credentials to log in into the VM.
B. After the VM has been created, use gcloud compute reset-windows-password to retrieve the login credentials for the VM.
C. When creating the VM, add metadata to the instance using 'windows-password' as the key and a password as the value.
D. After the VM has been created, download the JSON private key for the default Compute Engine service account. Use the credentials in the JSON file to log in to the VM.

B

VM 생성 후 gcloud compute reset-windows-password를 사용하여

VM의 로그인 사용자 인증 정보를 검색

사용자가 윈도우 가상 머신 인스턴스의 비밀번호를 재 설정하고 검색할 수 있으며

윈도우 계정이 없는 경우 이 명령을 사용하면 계정이 생성되고 해당 새 계정의 암호가 반환됨

Q74
You want to configure an SSH connection to a single Compute Engine instance for users in the dev1 group. This instance is the only resource in this particular
Google Cloud Platform project that the dev1 users should be able to connect to. What should you do?
A. Set metadata to enable-oslogin=true for the instance. Grant the dev1 group the compute.osLogin role. Direct them to use the Cloud Shell to ssh to that instance.
B. Set metadata to enable-oslogin=true for the instance. Set the service account to no service account for that instance. Direct them to use the Cloud Shell to ssh to that instance.
C. Enable block project wide keys for the instance. Generate an SSH key for each user in the dev1 group. Distribute the keys to dev1 users and direct them to use their third-party tools to connect.
D. Enable block project wide keys for the instance. Generate an SSH key and associate the key with that instance. Distribute the key to dev1 users and direct them to use their third-party tools to connect.

A

개인 키를 배포하거나 다른 사람을 위해 SSH 키를 생성하면 안 되므로 C, D는 오답

B는 인스턴스에 SSH가 필요한 개발 그룹과 관련이 없는 서비스 계정에 대한 부분이라 오답


Q75
You need to produce a list of the enabled Google Cloud Platform APIs for a GCP project using the gcloud command line in the Cloud Shell. The project name is my-project. What should you do?
A. Run gcloud projects list to get the project ID, and then run gcloud services list --project <project ID>.
B. Run gcloud init to set the current project to my-project, and then run gcloud services list --available.
C. Run gcloud info to view the account value, and then run gcloud services list --account <Account>.
D. Run gcloud projects describe <project ID> to verify the project value, and then run gcloud services list --available.

A

For those, who have doubts:

`gcloud services list --available` returns not only the enabled services in the project but also services that CAN be enabled. Therefore, option B is incorrect.

Q78
You are using Google Kubernetes Engine with autoscaling enabled to host a new application. You want to expose this new application to the public, using HTTPS on a public IP address. What should you do?
A. Create a Kubernetes Service of type NodePort for your application, and a Kubernetes Ingress to expose this Service via a Cloud Load Balancer.
B. Create a Kubernetes Service of type ClusterIP for your application. Configure the public DNS name of your application using the IP of this Service.
C. Create a Kubernetes Service of type NodePort to expose the application on port 443 of each node of the Kubernetes cluster. Configure the public DNS name of your application with the IP of every node of the cluster to achieve load-balancing.
D. Create a HAProxy pod in the cluster to load-balance the traffic to all the pods of the application. Forward the public traffic to HAProxy with an iptable rule. Configure the DNS name of your application using the public IP of the node HAProxy is running on.

A

HAProxy is HTTP only, doesnt support HTTPS, so you can reject option D
Cluster IP - is an internal IP, you cannot expose public externally. reject option B

out of option A and C
C, port 443 is https but public DNS is not going to give you a load balancing
A is the right choice,
kubernets ingress exposes HTTPS

Q81
You are operating a Google Kubernetes Engine (GKE) cluster for your company where different teams can run non-production workloads. Your Machine Learning
(ML) team needs access to Nvidia Tesla P100 GPUs to train their models. You want to minimize effort and cost. What should you do?
A. Ask your ML team to add the ג€accelerator: gpuג€ annotation to their pod specification.
B. Recreate all the nodes of the GKE cluster to enable GPUs on all of them.
C. Create your own Kubernetes cluster on top of Compute Engine with nodes that have GPUs. Dedicate this cluster to your ML team.
D. Add a new, GPU-enabled, node pool to the GKE cluster. Ask your ML team to add the cloud.google.com/gke -accelerator: nvidia-tesla-p100 nodeSelector to their pod specification.

D

Q82
Your VMs are running in a subnet that has a subnet mask of 255.255.255.240. The current subnet has no more free IP addresses and you require an additional
10 IP addresses for new VMs. The existing and new VMs should all be able to reach each other without additional routes. What should you do?
A. Use gcloud to expand the IP range of the current subnet.
B. Delete the subnet, and recreate it using a wider range of IP addresses.
C. Create a new project. Use Shared VPC to share the current network with the new project.
D. Create a new subnet with the same starting IP but a wider range to overwrite the current subnet.

A

Q84
A. Create two configurations using gcloud config. Write a script that sets configurations as active, individually. For each configuration, use gcloud compute instances list to get a list of compute resources.

Q85
You have a large 5-TB AVRO file stored in a Cloud Storage bucket. Your analysts are proficient only in SQL and need access to the data stored in this file. You want to find a cost-effective way to complete their request as soon as possible. What should you do?
A. Load data in Cloud Datastore and run a SQL query against it.
B. Create a BigQuery table and load data in BigQuery. Run a SQL query on this table and drop this table after you complete your request.
C. Create external tables in BigQuery that point to Cloud Storage buckets and run a SQL query on these external tables to complete your request.
D. Create a Hadoop cluster and copy the AVRO file to NDFS by compressing it. Load the file in a hive table and provide access to your analysts so that they can run SQL queries.
C

Q87
You deployed an LDAP server on Compute Engine that is reachable via TLS through port 636 using UDP. You want to make sure it is reachable by clients over that port. What should you do?
A. Add the network tag allow-udp-636 to the VM instance running the LDAP server.
B. Create a route called allow-udp-636 and set the next hop to be the VM instance running the LDAP server.
C. Add a network tag of your choice to the instance. Create a firewall rule to allow ingress on UDP port 636 for that network tag.
D. Add a network tag of your choice to the instance running the LDAP server. Create a firewall rule to allow egress on UDP port 636 for that network tag.

C

*allow ingress*

LDAP stands for Lightweight Directory Access Protocol. It is a protocol used for accessing and maintaining distributed directory information services over an Internet Protocol (IP) network. LDAP is often used as a standard way for clients to search and update information in a directory service, which is a hierarchical and distributed database that stores information about users, groups, and other resources in a network.


Q90
You want to configure a solution for archiving data in a Cloud Storage bucket. The solution must be cost-effective. Data with multiple versions should be archived after 30 days. Previous versions are accessed once a month for reporting. This archive data is also occasionally updated at month-end. What should you do?
A. Add a bucket lifecycle rule that archives data with newer versions after 30 days to Coldline Storage.
B. Add a bucket lifecycle rule that archives data with newer versions after 30 days to Nearline Storage.
C. Add a bucket lifecycle rule that archives data from regional storage after 30 days to Coldline Storage.
D. Add a bucket lifecycle rule that archives data from regional storage after 30 days to Nearline Storage.

B

Nearline has min storage of 30 days, while Coldline has 90 days.

Q92
You want to select and configure a solution for storing and archiving data on Google Cloud Platform. You need to support compliance objectives for data from one geographic location. This data is archived after 30 days and needs to be accessed annually. What should you do?
A. Select Multi-Regional Storage. Add a bucket lifecycle rule that archives data after 30 days to Coldline Storage.
B. Select Multi-Regional Storage. Add a bucket lifecycle rule that archives data after 30 days to Nearline Storage.
C. Select Regional Storage. Add a bucket lifecycle rule that archives data after 30 days to Nearline Storage.
D. Select Regional Storage. Add a bucket lifecycle rule that archives data after 30 days to Coldline Storage.

D

The main thing here is how often the data is retrieved. The question is saying that data needs to be accessed annually - i.e. once a year. Therefore, you should choose Coldline Storage, as it implies less frequent access than Nearline. (Archival Storage would fit even better but there's no such option)

From the link you provided:

"Nearline Storage is ideal for data you plan to read or modify on average once per month or less."

and

"Coldline Storage is ideal for data you plan to read or modify at most once a quarter. "

Q93
Your company uses BigQuery for data warehousing. Over time, many different business units in your company have created 1000+ datasets across hundreds of projects. Your CIO wants you to examine all datasets to find tables that contain an employee_ssn column. You want to minimize effort in performing this task.
What should you do?
A. Go to Data Catalog and search for employee_ssn in the search box.
B. Write a shell script that uses the bq command line tool to loop through all the projects in your organization.
C. Write a script that loops through all the projects in your organization and runs a query on INFORMATION_SCHEMA.COLUMNS view to find the employee_ssn column.
D. Write a Cloud Dataflow job that loops through all the projects in your organization and runs a query on INFORMATION_SCHEMA.COLUMNS view to find employee_ssn column.

A

Data Catalog lets you search and tag entries such as BigQuery tables with metadata. Some examples of metadata that you can use for tagging include public and private tags, data stewards, and rich text overview.

Q94
You create a Deployment with 2 replicas in a Google Kubernetes Engine cluster that has a single preemptible node pool. After a few minutes, you use kubectl to examine the status of your Pod and observe that one of them is still in Pending status:

What is the most likely cause?
A. The pending Pod's resource requests are too large to fit on a single node of the cluster.
B. Too many Pods are already running in the cluster, and there are not enough resources left to schedule the pending Pod.
C. The node pool is configured with a service account that does not have permission to pull the container image used by the pending Pod.
D. The pending Pod was originally scheduled on a node that has been preempted between the creation of the Deployment and your verification of the Pods' status. It is currently being rescheduled on a new node.
D

When one of the nodes from the node pool is down because its a preemtible, all of their pods go down and the kube controller (which is an element running on control plance ) reallocates the pods to other nodes from their node pools and then those pods rill show as running (if the nodes accomplish the resource quotas) .
So basically what happend here is that we just did a kubectl get pods command inthe mid time where one of the nodes from the node pool went down and the cluster tried to reposition the pods into other nodes

Q96
Your company implemented BigQuery as an enterprise data warehouse. Users from multiple business units run queries on this data warehouse. However, you notice that query costs for BigQuery are very high, and you need to control costs. Which two methods should you use? (Choose two.)
A. Split the users from business units to multiple projects.
B. Apply a user- or project-level custom query quota for BigQuery data warehouse.
C. Create separate copies of your BigQuery data warehouse for each business unit.
D. Split your BigQuery data warehouse into multiple data warehouses for each business unit.
E. Change your BigQuery query model from on-demand to flat rate. Apply the appropriate number of slots to each Project.

BE

Q97
You are building a product on top of Google Kubernetes Engine (GKE). You have a single GKE cluster. For each of your customers, a Pod is running in that cluster, and your customers can run arbitrary code inside their Pod. You want to maximize the isolation between your customers' Pods. What should you do?
A. Use Binary Authorization and whitelist only the container images used by your customers' Pods.
B. Use the Container Analysis API to detect vulnerabilities in the containers used by your customers' Pods.
C. Create a GKE node pool with a sandbox type configured to gvisor. Add the parameter runtimeClassName: gvisor to the specification of your customers' Pods.
D. Use the cos_containerd image for your GKE nodes. Add a nodeSelector with the value cloud.google.com/gke-os-distribution: cos_containerd to the specification of your customers' Pods.
C

gVisor is the way to isolate. 

Q98
C. Change the primary key to not have monotonically increasing values.

Q101
You need to host an application on a Compute Engine instance in a project shared with other teams. You want to prevent the other teams from accidentally causing downtime on that application. Which feature should you use?
A. Use a Shielded VM.
B. Use a Preemptible VM.
C. Use a sole-tenant node.
D. Enable deletion protection on the instance.

D
실수를 방지하기 위해서는 인스턴스에 deleteProtection 속성을 설정하여 삭제되지 않도록 해야 함


Q102

Your organization needs to grant users access to query datasets in BigQuery but prevent them from accidentally deleting the datasets. You want a solution that follows Google-recommended practices. What should you do?
A. Add users to roles/bigquery user role only, instead of roles/bigquery dataOwner.
B. Add users to roles/bigquery dataEditor role only, instead of roles/bigquery dataOwner.
C. Create a custom role by removing delete permissions, and add users to that role only.
D. Create a custom role by removing delete permissions. Add users to the group, and then add the group to the custom role.

D


Q103
You have a developer laptop with the Cloud SDK installed on Ubuntu. The Cloud SDK was installed from the Google Cloud Ubuntu package repository. You want to test your application locally on your laptop with Cloud Datastore. What should you do?
A. Export Cloud Datastore data using gcloud datastore export.
B. Create a Cloud Datastore index using gcloud datastore indexes create.
C. Install the google-cloud-sdk-datastore-emulator component using the apt get install command.
D. Install the cloud-datastore-emulator component using the gcloud components install command.

D

Q104
Your company set up a complex organizational structure on Google Cloud. The structure includes hundreds of folders and projects. Only a few team members should be able to view the hierarchical structure. You need to assign minimum permissions to these team members, and you want to follow Google-recommended practices. What should you do?
A. Add the users to roles/browser role.
B. Add the users to roles/iam.roleViewer role.
C. Add the users to a group, and add this group to roles/browser.
D. Add the users to a group, and add this group to roles/iam.roleViewer role.

C

Browser - Read access to browse the hierarchy for a project, including the folder, organization, and allow policy

hierarchical : Browser


Q105
Your company has a single sign-on (SSO) identity provider that supports Security Assertion Markup Language (SAML) integration with service providers. Your company has users in Cloud Identity. You would like users to authenticate using your company's SSO provider. What should you do?
A. In Cloud Identity, set up SSO with Google as an identity provider to access custom SAML apps.
B. In Cloud Identity, set up SSO with a third-party identity provider with Google as a service provider.
C. Obtain OAuth 2.0 credentials, configure the user consent screen, and set up OAuth 2.0 for Mobile & Desktop Apps.
D. Obtain OAuth 2.0 credentials, configure the user consent screen, and set up OAuth 2.0 for Web Server Applications.

B

Q109
Your organization has user identities in Active Directory. Your organization wants to use Active Directory as their source of truth for identities. Your organization wants to have full control over the Google accounts used by employees for all Google services, including your Google Cloud Platform (GCP) organization. What should you do?
A. Use Google Cloud Directory Sync (GCDS) to synchronize users into Cloud Identity.
B. Use the cloud Identity APIs and write a script to synchronize users to Cloud Identity.
C. Export users from Active Directory as a CSV and import them to Cloud Identity via the Admin Console.
D. Ask each employee to create a Google account using self signup. Require that each employee use their company email address and password.

A
GCDS를 사용하면 AD/LDAP 서비스의 사용자, 그룹 및 기타 데이터를

구글 클라우드 도메인 디렉터리로 동기화할 수 있음

Q110
You have successfully created a development environment in a project for an application. This application uses Compute Engine and Cloud SQL. Now you need to create a production environment for this application. The security team has forbidden the existence of network routes between these 2 environments and has asked you to follow Google-recommended practices. What should you do?
A. Create a new project, enable the Compute Engine and Cloud SQL APIs in that project, and replicate the setup you have created in the development environment.
B. Create a new production subnet in the existing VPC and a new production Cloud SQL instance in your existing project, and deploy your application using those resources.
C. Create a new project, modify your existing VPC to be a Shared VPC, share that VPC with your new project, and replicate the setup you have in the development environment in that new project in the Shared VPC.
D. Ask the security team to grant you the Project Editor role in an existing production project used by another division of your company. Once they grant you that role, replicate the setup you have in the development environment in that project.

A

모범 사례는 프로덕션 환경의 경우 새로운 프로젝트를 만드는 것

공유 VPC는 보안 팀의 규칙을 위반하는 방식이기 때문에 제외

또한, 프로젝트 설정에서 복제하는 것은 모범 사례가 아님

Q115

You need to reduce GCP service costs for a division of your company using the fewest possible steps. You need to turn off all configured services in an existing
GCP project. What should you do?
A. 1. Verify that you are assigned the Project Owners IAM role for this project. 2. Locate the project in the GCP console, click Shut down and then enter the project ID.
B. 1. Verify that you are assigned the Project Owners IAM role for this project. 2. Switch to the project in the GCP console, locate the resources and delete them.
C. 1. Verify that you are assigned the Organizational Administrator IAM role for this project. 2. Locate the project in the GCP console, enter the project ID and then click Shut down.
D. 1. Verify that you are assigned the Organizational Administrators IAM role for this project. 2. Switch to the project in the GCP console, locate the resources and delete them.

A

프로젝트 소유자가 프로젝트를 종료 또는 삭제하는 것이 맞음

Q117
An employee was terminated, but their access to Google Cloud was not removed until 2 weeks later. You need to find out if this employee accessed any sensitive customer information after their termination. What should you do?
A. View System Event Logs in Cloud Logging. Search for the user's email as the principal.
B. View System Event Logs in Cloud Logging. Search for the service account associated with the user.
C. View Data Access audit logs in Cloud Logging. Search for the user's email as the principal.
D. View the Admin Activity log in Cloud Logging. Search for the service account associated with the user.

C

Data Access audit logs provide detailed information about accesses to your Google Cloud resources. 

*ACCESS* Audit logs

Q119
Your company has a large quantity of unstructured data in different file formats. You want to perform ETL transformations on the data. You need to make the data accessible on Google Cloud so it can be processed by a Dataflow job. What should you do?
A. Upload the data to BigQuery using the bq command line tool.
B. Upload the data to Cloud Storage using the gsutil command line tool.
C. Upload the data into Cloud SQL using the import function in the console.
D. Upload the data into Cloud Spanner using the import function in the console.
B

Unstructured : Cloud Storage

Q120
You need to manage multiple Google Cloud projects in the fewest steps possible. You want to configure the Google Cloud SDK command line interface (CLI) so that you can easily manage multiple projects. What should you do?
A. 1. Create a configuration for each project you need to manage. 2. Activate the appropriate configuration when you work with each of your assigned Google Cloud projects.
B. 1. Create a configuration for each project you need to manage. 2. Use gcloud init to update the configuration values when you need to work with a non-default project
C. 1. Use the default configuration for one project you need to manage. 2. Activate the appropriate configuration when you work with each of your assigned Google Cloud projects.
D. 1. Use the default configuration for one project you need to manage. 2. Use gcloud init to update the configuration values when you need to work with a non-default project.
A

Cloud SDK comes with a default configuration.
Multiple configurations. Activate to switch between configurations.
-->