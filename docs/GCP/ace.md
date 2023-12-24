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
-->