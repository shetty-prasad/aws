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

  
