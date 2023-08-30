# Developing and Deploying in the Cloud

## Development in the cloud
- Many Google Cloud customers use Git repositories to store, version, and manage their source code trees.

- you could keep code private to a Google Cloud project and use IAM permissions to protect it, but not have to maintain the Git instance yourself? 
    - **Cloud Source Repositories**
    - Cloud Source Repositories provides full-featured Git repositories hosted on Google Cloud that support the collaborative development of any application or service, including those that run on App Engine and Compute Engine.

- **Cloud Functions**
    - Cloud Functions is a lightweight, event-based, asynchronous compute solution
    -  allows you to create small, single-purpose functions that respond to cloud events, without the need to manage a server or a runtime environment. 
    - You can use these functions to construct application workflows from individual business logic tasks. You can also use Cloud Functions to connect and extend cloud services. You’re billed to the nearest 100 milliseconds, but only while your code is running. 
    - Events from Cloud Storage and Pub/Sub can trigger Cloud Functions asynchronously, or you can use HTTP invocation for synchronous execution.

## Deployment: Infrastructure as Code
- **Terraform**

- To use Terraform, you create a template file using HashiCorp Configuration Language, HCL, that describes what you want the components of the environment to look like. 
- Terraform then uses that template to determine the actions needed to create the environment your template describes. If you need to change the environment, you can edit your template and then use Terraform to update the environment to match the change. 

- You can store in version-control your Terraform templates in Cloud Source Repositories.