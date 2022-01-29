# AWS Container on AWS - ECS, Fargate, ECR & EKS

- [Fargate is cheaper than EC2](https://aws.amazon.com/ko/blogs/containers/theoretical-cost-optimization-by-amazon-ecs-launch-type-fargate-vs-ec2/)
- No infra overhead is needed --> so no EC2

## Docker Containers Management
- To manage containers, we need a container management platform
- Three choices:
    - **ECS**: Amazon’s own container platform
    - **Fargate**: Amazon’s own Serverless container platform
    - **EKS**: Amazon’s managed Kubernetes (open source)

### What is Fargate?
- Launch Docker containers on AWS
- You do not provision the infrastructure *(no EC2 instances to manage)* – simpler!
- Serverless offering

### Amazon ECS task placement
- By default, **Fargate tasks are spread across Availability Zones**.