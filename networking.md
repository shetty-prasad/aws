#### VPC

- A Virtual Private Cloud (VPC) provides a secure, isolated section of AWS where you can launch AWS resources in a virtual network that you define. VPCs enable you to isolate resources, ensuring that one customer's data remains separate from another's even within the same AWS account and allow you to securely segment different applications.
- A VPC is specific to a single region .
- By design, resources within one VPC are isolated from those in another. To enable communication with the internet or between VPCs, you must explicitly configure the necessary settings, adding an extra layer of security. 
- AWS provides a default VPC per region with pre-configured subnets, an internet gateway, a security group, and a NACL


#### Subnet

- Subnets are defined as groups of IP addresses within your VPC, allowing you to deploy resources into isolated segments. When you launch an AWS resource—such as an EC2 instance—it must be assigned to a specific subnet.
- By default, resources in subnets can communicate with one another within the same VPC without requiring additional routing configurations in route tables.
- Every subnet is associated with a single availability zone. 

* Public Subnets:
  * Ideal for resources that need direct internet connectivity, such as web servers or public applications. Public subnets have a direct route to an Internet Gateway.

* Private Subnets:
  * Best suited for resources that do not require exposure to the internet, such as databases or internal applications. Internet-bound traffic in private subnets typically routes via a NAT Gateway.

- The first four IP addresses in every subnet are reserved:
- The first address serves as the network address.
- The second address is typically used by the VPC router.
- The third address is reserved for DNS.
- The fourth address is held for future use.
- Additionally, the last IP address in a subnet (e.g., 192.168.10.255 in a /24 subnet) is reserved as the broadcast address.

#### Route 

* Routing in a VPC is governed by a route table, a collection of routing rules that directs how network traffic should be forwarded.
* Each rule in this table is referred to as a route.
* The router inspects the destination IP address of each outbound packet and then matches it against the routes defined in the table.
* Every VPC is initialized with a default route table, which includes a mandatory local route. This local route ensures that traffic destined for other devices within the same VPC (as defined by the VPC's CIDR block) is routed internally.
* Multiple subnets can share the same route table when they adhere to identical routing rules but a single subnet cannot be assigned to multiple routes .

#### Internet Gateway
* By default, subnets in a VPC are created as private. Devices within these subnets cannot access the Internet, and external resources cannot reach them. To convert a subnet into a public subnet, you must attach an Internet Gateway to your VPC.
* An Internet Gateway is a horizontally scaled, redundant, and highly available component that spans all Availability Zones within a region.
* A subnet is converted to a public subnet when its route table includes a default route pointing to the Internet Gateway.
* Each VPC is limited to one Internet Gateway, and an Internet Gateway can be attached to only a single VPC.

#### NAT Gateway
* NAT Gateways allow instances within private subnets to initiate outbound connections while blocking unsolicited inbound traffic, thereby keeping internal servers shielded from external threats.
* Essentially, the NAT Gateway allows instances to initiate connections but blocks external entities from starting a conversation with them.
* Dependency on an Internet Gateway: Even though a NAT Gateway is used, an Internet Gateway is necessary for external connectivity.
* Deployment on Public Subnets: NAT Gateways are deployed within public subnets and thus receive a public IP address. This setup is essential for routing outbound traffic to the internet.
* Routing Setup: Ensure that the private subnet’s route table directs all outbound traffic to the NAT Gateway.
* Fully Managed Service: Being a managed service by AWS, NAT Gateways require minimal ongoing management, with AWS handling maintenance and scaling.

#### DNS VPC
* Automatic DNS Entries: Private IP addresses are automatically mapped to DNS entries.
* AWS DNS Server Access: The DNS servers can be accessed at the second IP (e.g 10.0.0.2) in the VPC CIDR block or via 169.254.169.253.
* Enable DNS Hostnames:
  * By default, only private IP addresses receive a DNS entry. To assign public DNS hostnames to instances with public IP addresses, ensure the "enable DNS hostnames" option is activated during VPC creation. This option is crucial for instances that need to be accessed publicly.
* Enable DNS Support:
  * This setting determines whether the VPC supports DNS resolution using Amazon-provided DNS servers. When enabled, DNS queries sent to the AWS DNS servers (either via the second IP in the VPC CIDR block or the special IP 169.254.169.253) will be resolved successfully. Disabling this option prevents DNS queries from reaching these servers.  

#### Elastic IP
* Elastic IP addresses are static IPv4 addresses allocated to your AWS account.
* Elastic IPs are tied to specific AWS regions and cannot be transferred across regions.
* public IP addresses assigned to EC2 instances are dynamic, i.e they change after system restart.

#### Security Group & Network Access Control List (NACL)

##### Firewall 
* Stateless Firewalls:
  * Evaluate each packet individually. Rules must be configured for both inbound and outbound traffic as there is no tracking of connection states.
  
![image](https://github.com/user-attachments/assets/68ac03be-62e8-4492-9010-96bff0167780)

* Stateful Firewalls:
  * Monitor active connections. When an inbound request is allowed, the corresponding outbound response is automatically permitted without the need for an explicit rule.

![image](https://github.com/user-attachments/assets/b2fa368a-630d-47a3-994a-9dbc68f4c28d)

##### AWS Network Access Control Lists (NACLs)
* Subnet-Level Filtering:
  * NACLs are associated with subnets rather than individual resources.
* Stateless Nature:
  * Being stateless, NACLs require explicit rules for both inbound and outbound traffic.

#### AWS Security Groups
* Security groups act as virtual firewalls for individual AWS resources, such as EC2 instances, load balancers, and RDS databases. They differ from NACLs in the following ways:
* Resource-Level Protection:
  * Security groups protect individual resources rather than an entire subnet.
* Stateful Behavior:
  * Only the initial inbound traffic (or outbound traffic) needs to be explicitly allowed since the return traffic is automatically permitted.
* Allow Rules Only:
  * Security groups only contain rules that allow traffic. There is no provision to create explicit deny rules; any traffic not expressly permitted is blocked.
 
    ![image](https://github.com/user-attachments/assets/1522e1ee-37e4-48ca-9650-66b5a32d81da)

### Load Balancer 

![image](https://github.com/user-attachments/assets/5eb67414-6c51-4a41-ad61-0b4a3c77dc80)

* By centralizing the entry point to your application, a load balancer abstracts away the complexity of managing multiple endpoints.
* ELBs enable you to manage your application's traffic seamlessly by routing requests to healthy EC2 instances, thus ensuring high availability.
  
There are three types of load balancers offered by AWS:

##### Classic Load Balancer (CLB) 
* It is not used much so ignore .
   
##### Application Load Balancer (ALB)
* Application Load Balancers are optimized for web-based applications that rely on HTTP, HTTPS, and WebSockets. Operating at the application layer (Layer 7), ALBs inspect and forward requests based on URL paths, host domains, HTTP methods, query parameters, and other HTTP-specific attributes.
* When a client sends an HTTP or HTTPS request, the load balancer terminates the connection.
* For HTTPS, this includes SSL/TLS termination—meaning the SSL certificates are managed on the ALB.
* After decryption, the ALB typically forwards the request over HTTP to the backend.
* If end-to-end encryption is required, secure connections can also be established at the instance level by configuring HTTPS on the backend targets.

![image](https://github.com/user-attachments/assets/3a193951-1f75-4c78-a66e-2b2d4a8de4cf)

![image](https://github.com/user-attachments/assets/bf38aff1-9c1e-4d3f-94e9-a9978fce34d3)

##### Network Load Balancer (NLB)
* The Network Load Balancer works at the transport layer (Layer 4), handling TCP and UDP traffic. Unlike the ALB, the NLB does not inspect higher-level protocols like HTTP or HTTPS.
* It is ideal for applications that require support for non-HTTP/HTTPS protocols or scenarios where high throughput and low latency are critical.
* Since the NLB does not terminate traffic, TCP sessions persist end-to-end between the client and the EC2 instance.
* When you require HTTPS with an NLB, SSL certificates must be managed on the server side, as the SSL/TLS handshake occurs directly between the client and the target. Health checks are conducted using TCP or ICMP to confirm the accessibility of targets at the transport layer.
  
![image](https://github.com/user-attachments/assets/c0849dfc-ce21-460f-8c67-debba768b26b)

![image](https://github.com/user-attachments/assets/e9c71214-a7a2-468a-b125-eec5574c4415)

#### Public vs. Private Load Balancers
Elastic Load Balancers can be deployed as either public or private, depending on your application's requirements:

* Public Load Balancer:
  * Deployed within a public subnet, it is accessible from the internet. This is ideal for web applications, APIs, or any service that needs to serve external users.

* Private Load Balancer:
  * Deployed within a private subnet, it is only accessible from your internal network. This configuration is beneficial for back-end services, such as database servers, that should not be directly exposed to the internet.

#### Listeners and Target Groups
  When configuring an Elastic Load Balancer, two key components that define its behavior are listeners and target groups:

* Listeners:
  * A listener is a process that checks for incoming connection requests based on a specified protocol and port. For example, if you want to handle requests for "app1.com", a listener can be configured to detect those requests and forward them appropriately.

* Target Groups:
  * A target group comprises a set of registered targets—such as EC2 instances, ECS containers, or Lambda functions—that receive the forwarded traffic according to the listener's rules.
  * You can configure health checks for each target group to ensure only healthy targets receive traffic.

#### VPN
 
* VPNs provide secure connectivity between AWS VPCs and on-premises data centers.
* The VPN Gateway (AWS side) and Customer Gateway (on-premises side) serve as the endpoints for the encrypted IPsec tunnel.
* Routing can be managed either statically or dynamically using BGP to ensure proper packet flow.
* AWS charges for VPN usage based on connection uptime and data egress, and VPN tunnels have defined performance limits.

#### Direct Connect  
Direct Connect's architecture involves three primary components:

* Your On-Premise Network:
  * This can be a data center, corporate office, or any facility with a dedicated edge router or firewall. The router used here may serve both your internet connection and Direct Connect, or it can be exclusively dedicated to Direct Connect.

* Direct Connect Location:
  * Typically a regional data center where AWS installs its routers. This is not a full AWS data center but rather a location where the physical connection is established.

* AWS's Direct Connect Router:
  * This router is located on the AWS side of the connection. When you purchase Direct Connect, you lease a port on the AWS router. Your on-premise router then forms a "cross connect" with AWS’s router at the Direct Connect location.
 
#### VPC Peering 
* It enables direct communication between Virtual Private Clouds (VPCs) by connecting them through a private network.
* One critical aspect of VPC peering is its non-transitive nature. For example, if you have three VPCs (VPC One, VPC Two, and VPC Three) and establish peering between VPC One & VPC Two and between VPC Two & VPC Three, VPC One will not automatically communicate with VPC Three.
* VPC peering supports connections between VPCs in the same region, across different regions & between VPCs in different AWS accounts.
* After establishing the connection, you need to manually update the route tables in both VPCs.

####  Transit Gateway
* AWS Transit Gateway service, simplifies connectivity and routing between multiple VPCs and on-premises environments.
* AWS Transit Gateway acts as a centralized router, allowing you to attach each VPC to the gateway instead of wiring a full mesh of peerings .
* In a scenario with four VPCs, each VPC requires only a single connection to the Transit Gateway, which then efficiently routes traffic between them.

![image](https://github.com/user-attachments/assets/230f0d88-78b5-4b7f-8061-2b66fef75d85)

* Transit Gateway reduces the number of VPN connections needed for on-premises connectivity.
* Integration with both VPN and AWS Direct Connect enhances on-premises connection options.

#### PrivateLink

* PrivateLink is a secure and efficient method that allows your Virtual Private Cloud (VPC) to connect directly to AWS services (like S3, CloudWatch, etc.) and even third-party services hosted in other VPCs using private IP addresses.
* This direct connectivity eliminates the need for routing traffic through the public Internet, reducing exposure and potential vulnerabilities.

#### CloudFront

* When delivering content globally, your web server may be situated in one region (for example, North America). If a user in India sends a request, the data must travel a long distance, resulting in high latency. To mitigate this, AWS uses edge locations: smaller, geographically dispersed sites that cache data from your origin (such as a web server or an S3 bucket). Users receive content from the nearest edge location, ensuring minimal delay.
* CloudFront is a web service that accelerates the distribution of both static and dynamic content—including HTML, CSS, JavaScript, images, videos, and music—by delivering it from the closest edge location.
* Distribution
  * A distribution in CloudFront is a configuration block where you define the origin settings. CloudFront generates a unique domain name (e.g., xyz.cloudfront.net) that users leverage to access cached content.

![image](https://github.com/user-attachments/assets/d118c481-fc27-4233-964e-e4041bc1870c)

* Time to Live (TTL)
  * The Time to Live (TTL) defines how long content remains cached at an edge location. By default, the TTL is set to 24 hours, meaning content is served from the cache for that duration before it is considered stale.
  * If a user requests the content after the TTL expires, CloudFront must fetch a fresh copy from the origin and update its cache.
 
* Cache Invalidation
  * Sometimes you need to update content before the TTL expires.
  *  CloudFront enables manual cache invalidation to remove outdated content from all edge locations.
 
  #### LambdaEdge

  * CloudFront functions and Lambda@Edge—that enable you to run code at Amazon CloudFront edge locations.
  * These services allow you to process requests and modify responses right at the edge, reducing the delay caused by routing traffic back to your origin servers.

![image](https://github.com/user-attachments/assets/5fa80b85-f50a-46a1-a190-ca74fcbd1b26)

#### Route 53

* It acts as a domain registrar, allowing you to purchase domain names much like popular services such as GoDaddy and Namecheap. For example, you can register a domain like KodeKloud.com directly through Route 53.
* It manages all DNS records for your domains, so once you register a domain, you can easily configure its DNS settings within Route 53.
* Being a global service, Route 53 is not limited to a single region, ensuring worldwide accessibility.

* Hosted Zones
  * A hosted zone is a container for all the DNS records associated with a specific domain (e.g., KodeKloud.com).
  * When you create a hosted zone:

  * AWS reserves four dedicated name servers for that domain.
  * For any additional domain, such as fastcars.com, AWS allocates another distinct set of four name servers to manage its DNS records and rules.

![image](https://github.com/user-attachments/assets/d91ff8ba-3456-455f-b081-0483646259db)


