# EC2

* An EC2 instance is a virtual server that offers computing power equivalent to a physical host. AWS provides various instance types to match your application's CPU, memory, and storage needs.
* These include:
  * General Purpose: Offers a balance of compute, memory, and networking resources for diverse workloads, such as web servers.
  * Compute Optimized: Designed for compute-bound applications requiring high-performance processors.
  * Memory Optimized: Ideal for workloads that need to process large data sets in memory.
  * Storage Optimized: Suited for applications with high I/O demands that frequently access disk storage.
  * GPU Instances: Perfect for high-performance tasks like machine learning and deep learning that require powerful GPUs.
 
**Amazon Machine Images (AMIs)**
The AMI includes the operating system and may come pre-configured with additional software or settings you require. AMIs enable rapid deployment of identical servers at scale.

**Elastic IP Addresses**

* Elastic IPs remain constant even if you move the underlying instance between physical hosts, and can be re-assigned to different instances as needed.

**EC2 Instance Pricing Options**

* AWS offers a range of pricing models designed to suit various workloads and budgets:
  * On-Demand: Pay per hour or second with no long-term commitment, ideal for short-term or unpredictable workloads.
  * Spot Instances: Bid on unused AWS capacity at discounts of up to 90% compared to on-demand prices. Because spot instances can be interrupted, they are best suited for flexible applications like batch processing.
  * Savings Plans: Commit to a consistent hourly cost over a one- or three-year term in exchange for lower prices, offering predictability and cost savings for steady-state usage.
  * Reserved Instances: Reserve a specific amount of compute capacity for one or three years, providing cost savings for predictable workloads.
  * Dedicated Hosts: Rent an entire physical server exclusively for your use—ideal for licensing requirements and compliance, ensuring your instances run on the same physical server.
  * Dedicated Instances: Similar to Dedicated Hosts, these instances run on hardware isolated for your use, although the specific physical host may change over time.
 
## Elastic Network Interfaces

An ENI is a virtual network interface that can be attached to EC2 instances within a VPC. It decouples network configurations from the compute instances, enabling you to seamlessly move an interface—with all its associated settings such as IP addresses, security groups, and more—across different instances.
Its main properties include:
* A primary private IPv4 address automatically selected from the subnet's CIDR block.
* Optionally, a primary IPv6 address.
* Secondary private IPv4 addresses.
* The ability to associate an Elastic IP address.
* Public IP address allocation when enabled.
* A unique MAC address.
* Configurable flags like the source/destination check.

**Elastic Beanstalk**
AWS Elastic Beanstalk is the ideal solution for developers who want a hassle-free deployment process. Its powerful features, such as managed platform updates, autoscaling, and integrated monitoring, allow you to launch and maintain web applications with confidence.
