# Overview

> [Container Overview](#Container)  
> [Container Orchestration](#Container-Orchestration)  
> [Kubernetes Architecture](#Kubernetes-Architecture)  

## Container

- the most popoular Containers technology is Docker
- Why do you need containers?
    > *Compatibility/Dependency*  
    > *Long setup time*  
    > *Different Dev/Test/Prod environments*  
    

    - To run our project, we need web server, Database, Messaging(like Redis), Orchestration tool. But When all of these works for a different OS, We face the following issue
    - One service requires A version dependent libraries
    - Whereas, the other service requires B vesrion dependent libraries
    - **Our architecture of our system changes over tiee**
    - Every time there's an update on the service, we should have been checked the compatibility issue 
    - Docker solves the problem
- What Docker do?
    > Containerize Applications  
    > Run each component in separate Container with its own libraries and dependencies  


- What are Containers?
    > Container is an completely isolated environment  


<!--But its also important to note that containers are not new with Docker. Containers have existed for about 10 years now and some of the different types of containers are LXC, LXD , LXCFS etc. Docker utilizes LXC containers. Setting up these container environments is hard as they are very low level and that is were Docker offers a high-level tool with several powerful functionalities making it really easy for end users like us.-->

- Operating system
    - To understand how Docker works,
    - We should understand how OS works.
    
    >  OS : MacOS, Windows, Unix  
    >  OS (OS Kernel + set of Software)  
    
    
    - 커널은 “운영체제의 핵심부로 컴퓨터 자원(CPU, memeroy, file systems, i/o system)들을 관리하는 역할”을 수행
    - 쉘(Shell) : 사용자가 컴퓨터에게 전달하는 명령을 해석하는 프로그램. 즉, 커널과 사용자간의 다리 역할을 수행
    - 쉘을 통해 커널에게 사용자가 명령을 내릴 수 있음
    - 사용자 -> 시스템 프로그램(Shell 등을 사용) -> 커널 -> 컴퓨터 자원 접근

- Docker continaer share the underlying OS kernels
    - Let’s say we have a system with an Ubuntu OS with Docker installed on it. Docker can run any flavor of OS on top of it as long as they are all based on the same kernel 
    - If the underlying OS is Ubuntu, docker can run a container based on another distribution like debian, fedora, suse or centos. Each docker container only has the additional software
    - that makes these operating systems different and docker utilizes the underlying kernel of the Docker host which works with all Oses 
    - Exception : Windows

- Containers vs. Virtual machine
    - (#page 14)

- **Container vs. image**
    - An image is a package or a template
    - Containers are running instances off images that are isolated and have their own environments and set of processes

- Container Advantage
    - *Dockerfile*
        - With Docker, a major portion of work involved in setting up the infrastructure is now in the hands of the developers in the form of a Docker file. The guide that the developers built previously to setup the infrastructure can now easily put together into a Dockerfile to create an image for their applications. This image can now run on any container platform and is guaranteed to run the same way everywhere. 

## Container Orchestration

- The platform needs to orchestrate the connectivity between the containers and automatically scale up or down based on the load. This whole process of automatically deploying and managing containers is known as Container Orchestration.
- Kubernetes is thus a container orchestration technology. 

## Kubernetes Architecture  
