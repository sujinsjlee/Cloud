# Applications in the Cloud

## App Engine
> App Engine is a fully managed, serverless platform for developing and hosting web applications at scale.

- App Engine provides built-in services and APIs like NoSQL data stores, memcache, load balancing, health checks, application logging and a user authentication API that's common to most applications. 

- App Engine also offer software development kits or SDKs to help you develop, deploy, and manage your apps on your local machine.


## Google Cloud API management tools
- APIs : Application Programming Interface

- **Cloud Endpoints** is a distributed API management system that uses a distributed Extensible Service Proxy, which is a service proxy that runs in its own Docker container. The goal is to help you create and maintain even the most demanding APIs with low latency and high performance. Cloud Endpoints provides an API console, hosting, logging, monitoring, and other features to help you create, share, maintain, and secure your APIs.

- Cloud Endpoints supports applications running in App Engine, Google Kubernetes Engine, and Compute Engine. Clients include Android, iOS, and Javascript.

- **API Gateway**
    - API Gateway enables you to provide secure access to your backend services through a well-defined REST API that is consistent across all of your services, regardless of the service implementation. Clients consume your REST APIS to implement standalone apps for a mobile device or tablet, through apps running in a browser, or through any other type of app that can make a request to an HTTP endpoint. 

- **Apigee API**
    - Unlike Cloud Endpoints, Apigee API Management has a specific focus on business problems, like rate limiting, quotas, and analytics. In fact, many Apigee API Management users provide a software service to other companies. 

## Cloud Run

> Cloud Run : a managed compute platform that lets you run stateless Containers via web requests or Pub/Sub events. Cloud Run is serverless. That means it removes all infrastructure management tasks so you can focus on developing applications. 
