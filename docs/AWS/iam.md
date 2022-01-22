# AWS IAM
> [IAM Permission Boundaries](#IAM-Permission-Boundaries)  
> [IAM Roles](#IAM-Roles)

## IAM Permission Boundaries
- AWS supports permissions boundaries for IAM entities (users or roles). A permissions boundary is an advanced feature for using a managed policy to set the maximum permissions that an identity-based policy can grant to an IAM entity. An entity's permissions boundary allows it to perform only the actions that are allowed by both its identity-based policies and its permissions boundaries.

## IAM Roles
- When you **assume a role** (user, application or service), you give up your original permissions and take the permissions assigned to the role