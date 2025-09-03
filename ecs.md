## ECS

Think of containers as lightweight virtual machines that include only what is needed to run your application.

ECS is a fully managed container orchestration service that facilitates the deployment, scaling, and management of containerized applications.

![image](https://github.com/user-attachments/assets/e8db92d7-8218-444a-95e1-35890001cf3d)

With ECS, AWS manages the control plane—the "brains" of the operation—while you provide the compute resources.

There are two primary launch options with ECS:

* **EC2 Launch Type**: You manage your own EC2 instances as a cluster, giving you complete control over the underlying servers.
* **Fargate Launch Type**: AWS handles the compute infrastructure using a serverless model. You only need to specify the required configurations, and AWS provisions the compute resources automatically.

**Understanding ECS Clusters, Task Definitions and services**

A cluster is a logical grouping of resources where your containers run. It can include:
- EC2 instances (if you're using the EC2 launch type)
- Or just be a logical space (if you're using Fargate)
- 
Purpose: It acts as the environment or "home" for your tasks and services. Think of it as the container orchestration arena.


A task definition is like a recipe that tells ECS how to run your containers. It includes:
- Docker image(s)
- CPU and memory requirements
- Environment variables
- Networking and logging configs
  
Purpose: It defines what to run and how to run it. You can think of it as a manifest that ECS uses to launch containers

A service ensures that a specified number of task instances (based on your task definition) are always running.

Purpose: It provides high availability, auto-recovery, and load balancing. If a task crashes, the service automatically replaces it. It’s ideal for long-running applications like web servers or APIs.
