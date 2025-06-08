#### VPC

- A Virtual Private Cloud (VPC) provides a secure, isolated section of AWS where you can launch AWS resources in a virtual network that you define. VPCs enable you to isolate resources, ensuring that one customer's data remains separate from another's even within the same AWS account and allow you to securely segment different applications.
- A VPC is specific to a single region .
- By design, resources within one VPC are isolated from those in another. To enable communication with the internet or between VPCs, you must explicitly configure the necessary settings, adding an extra layer of security. 
- AWS provides a default VPC per region with pre-configured subnets, an internet gateway, a security group, and a NACL


#### Subnet

- Subnets are defined as groups of IP addresses within your VPC, allowing you to deploy resources into isolated segments. When you launch an AWS resource—such as an EC2 instance—it must be assigned to a specific subnet.
- By default, resources in subnets can communicate with one another within the same VPC without requiring additional routing configurations in route tables.
- Every subnet is associated with a single availability zone. 

- Public Subnets:
Ideal for resources that need direct internet connectivity, such as web servers or public applications. Public subnets have a direct route to an Internet Gateway.

- Private Subnets:
Best suited for resources that do not require exposure to the internet, such as databases or internal applications. Internet-bound traffic in private subnets typically routes via a NAT Gateway.

- The first four IP addresses in every subnet are reserved:
- The first address serves as the network address.
- The second address is typically used by the VPC router.
- The third address is reserved for DNS.
- The fourth address is held for future use.
- Additionally, the last IP address in a subnet (e.g., 192.168.10.255 in a /24 subnet) is reserved as the broadcast address.

