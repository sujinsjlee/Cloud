# Resources and Access

## Google Cloud resource hierarchy

> **Organization node > Folder > Project > Resources**

- **Organization node** : And then at the top level is an organization node, which encompasses all the projects, folders, and resources in your organization.

- **Projects** are the basis for enabling and using Google Cloud services, like managing APIs, enabling billing, adding and removing collaborators, and enabling other Google services.

- Folders let you assign policies to resources at a level of granularity you choose. The resources in a folder inherit policies and permissions assigned to that folder. 

## Identity and Access Management (IAM)

> With IAM, administrators can apply policies that define who can do what and on which resources. 

- An **IAM role** is a collection of permissions. When you grant a role to a principal, you grant all the permissions that the role contains.

## Service Accounts

> What if you want to give **permissions to a Compute Engine virtual machine**, rather than to a person? Well, that’s what service accounts are for.

- In addition to being an identity, a service account is also a resource, so it can have IAM policies of its own attached to it. 

## Cloud Identity
> With a tool called Cloud Identity, organizations can define policies and manage their users and groups using the **Google Admin Console**.


## Interacting with GCP
There are four ways to access and interact with Google Cloud. 
- Google Cloud Console
- Cloud SDK and Cloud Shell
    - Google Cloud CLI
    - gcloud storage
    - bq
- APIs
- Cloud Console Mobile App.

 