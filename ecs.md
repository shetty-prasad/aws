## ECS

Think of containers as lightweight virtual machines that include only what is needed to run your application.

![image](https://github.com/user-attachments/assets/e8db92d7-8218-444a-95e1-35890001cf3d)

With ECS, AWS manages the control plane—the "brains" of the operation—while you provide the compute resources.

There are two primary launch options with ECS:

* **EC2 Launch Type**: You manage your own EC2 instances as a cluster, giving you complete control over the underlying servers.
* **Fargate Launch Type**: AWS handles the compute infrastructure using a serverless model. You only need to specify the required configurations, and AWS provisions the compute resources automatically.

**Understanding ECS Tasks and Task Definitions**

