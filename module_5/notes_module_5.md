# Networking Basics
* A computer network is two or more client machines that are connected together to share resources. A network can be logically partitions into subnets.
* They require a networking device (router or switch) to connect clients together and enable comms.
* each machine has an IP address
  * IPv4 (32-bit)
  * IPv6 (128-bit) - can accommodate more devices.
  * Classless inter-domain routing (CIDR) is a way to express a group of IP addresses that are consecutive to each other.
*   OCTETS
* Open systems interconnection (OSI) model is used to explain how data travels over a network.
* Firewall
  * Controls ports and access
  * Software & hardware
  * Demilitarized Zone
  * Implicit deny - if we dont allow a type of traffic were going to deny it (whitelist/blacklist similarity)
* TCP vs UDP
  * TCP is best used for dirrect comms in which a reliable connection is needed. (Web browsing, downloading files)
  * UDP is best used for real-time data transmission (live streaming)

# Amazon VPC
Enables you to provision a logically isolated section of the aws cloud where you can launch aws resources in a virtual network that you define.

Gives control over IP range selection, creation of subnets, and configuration of route tables and network gateways.
Logically isolated from other VPCs, belonging to a single AWS Region and can span multiple Availability Zones
## Subnets: 
Range of IP addresses that divide a VPC.

They belong to a single Availability Zone and are classified as public or private.

When creating a VPC, you assign it to an IPv4 CIDR block (range of private IPv4 addresses) and you cant change the range after creation.

The largest IPv4 CIDR block size is /16 or 65,536 addresses

The smallest is /28 or 16 addresses

When you create a subnet, it requires its own CIDR block. For each CIDR block that you specify, AWS reserves 5 IPs within that block for:

    * Network address
    * VPC local router (internal comms)
    * Domain Name System (DNS) resolution
    * Future use
    * Network broadcast address
## Route Tables & Routes
A route table contains a set of rules that you can configure to direct network traffic from your subnet.

Each route specifies a destination and a target, and by default, every route table contains a local route for comms within the VPC. 

Each subnet must be associated with, at most, one route table.
## Section 2 Takeaways
    * A VPC is a logically isolated section of the AWS Cloud.
    * A VPC belongs to one Region and requires a CIDR block.
    * A VPC is subdivided into subnets.
    * A subnet belongs to one Availability Zone and requires a CIDR block.
    * Route tables control traffic for a subnet.
    * Route tables have a built-in local route.
    * You add additional routes to the table.
    * The local route cannot be deleted.

# VPC Networking
## Internet Gateway
An Internet Gateway (IGW) is a critical component within Amazon Web Services (AWS) that allows communication between an Amazon Virtual Private Cloud (VPC) and the internet.

### Functionality:

It serves two primary purposes:
1. Provide a path for outbound traffic from the VPC to the internet.
2. Facilitate inbound traffic from the internet to the VPC.


### Attachment:
For a VPC to have internet connectivity, you must attach an Internet Gateway to it. A VPC can be associated with only one Internet Gateway at a time, but an IGW can be detached and reattached if needed.

### Routing:
Once the IGW is attached, you need to update the VPC's route tables to direct traffic to the IGW. For instance, a common route added to a VPC's route table is 0.0.0.0/0 targeting the IGW, which means all traffic not destined for the VPC itself should be sent to the Internet Gateway.

### Public and Private Subnets:
Public Subnet: If a subnet's traffic is routed to an Internet Gateway, it's known as a public subnet. Typically, resources in a public subnet (like an EC2 instance) would have an Elastic IP or Public IP to communicate with the internet.
Private Subnet: If a subnet doesn't have a route to the IGW, it's considered a private subnet. Resources in a private subnet can't directly communicate with the internet. However, they can access the internet using Network Address Translation (NAT) gateways or NAT instances located in a public subnet.

### Security:
While the Internet Gateway allows traffic to flow in and out, the actual permissions for such traffic are governed by security groups and network access control lists (NACLs) within the VPC.

It's important to note that the IGW itself doesn't perform any filtering. The responsibility for permitting or denying traffic rests with the associated security mechanisms (security groups and NACLs).

### Charges:
There's no additional charge for creating and using the Internet Gateway itself. However, standard data transfer charges apply for data moving through the IGW.

### High Availability:
Internet Gateways are designed to be highly available. They are inherently redundant, spread across multiple Availability Zones (AZs), ensuring continuous operation and fault tolerance.

## Network Address Translation
A Network Address Translation (NAT) Gateway in the context of Amazon Web Services (AWS) is a managed service that allows instances in a private subnet to initiate outbound traffic to the internet, while also preventing unsolicited inbound traffic from reaching those instances. Here's a detailed overview:

### Purpose:
The primary purpose of a NAT Gateway is to enable instances in a private subnet to access the internet for tasks like software updates, while ensuring that the internet cannot initiate connections to those instances.

### Functionality:
It works by translating the private IP addresses of instances in the private subnet to its own public IP address (and vice versa for the return traffic). This ensures that while the instances can access the internet, they aren't directly addressable or reachable from the internet.

### Creation and Configuration:
When you create a NAT Gateway, you must specify the public subnet in which the NAT Gateway should reside, and you must also allocate an Elastic IP address to the NAT Gateway.

You then update the route tables associated with your private subnet to point outbound traffic (typically 0.0.0.0/0) to the NAT Gateway.

### High Availability:
NAT Gateways are designed to be highly available within an Availability Zone (AZ). They automatically handle failover to another NAT Gateway in case of failures.

However, if you want redundancy across AZs, you'd need to create a NAT Gateway in each AZ and configure your routing to ensure that resources use the NAT Gateway in the same AZ.

### Bandwidth and Scaling:
NAT Gateways are designed to handle up to 45 Gbps of bursty traffic. They automatically scale based on the level of traffic.

### Security:
While the NAT Gateway allows outbound traffic and the corresponding return traffic, it doesn't allow unsolicited inbound traffic. This makes it a secure solution for instances that need to initiate outbound internet traffic without being exposed to the internet.
As with other AWS resources, the actual permissions for such traffic are also governed by security groups and network access control lists (NACLs) within the VPC.

### Billing:
You are billed for each hour that a NAT Gateway is provisioned and available, and also for the amount of data processed by the NAT Gateway.

### Difference from NAT Instances:
Before NAT Gateways, AWS users often used EC2 instances with NAT configured (known as NAT Instances) to achieve similar functionality. However, NAT Gateways are preferred because they're managed services that don't require patching, offer higher bandwidth, and are inherently highly available within an AZ.

### Limitations:
NAT Gateways do not support IPv6 traffic. For that, you'd use an Egress-Only Internet Gateway.

They cannot be associated with security groups, but you can use NACLs for controlling the traffic.

## VPC Sharing
VPC sharing is a feature within Amazon Web Services (AWS) that allows multiple AWS accounts to share a single Virtual Private Cloud (VPC). This is particularly useful for organizations with multiple accounts that want to streamline their network architecture and reduce operational overhead. Here's an overview of VPC sharing:

### Primary Benefit:
VPC sharing allows organizations to use a single shared VPC across multiple AWS accounts, rather than setting up multiple VPCs in each account. This centralizes the network architecture, making it easier to manage and monitor.

### Owner and Participants:
**VPC Owner**: The AWS account that creates the VPC is the owner. The owner retains full control over the VPC's CIDR blocks, its associated IPv6 CIDR blocks, its subnets, and the data flowing across the VPC.

**VPC Participants**: Other AWS accounts that are given permission to use the shared VPC. Participants can launch, manage, and operate their AWS resources in the shared VPC as if it was their own, but they can't modify the VPC's core attributes.

### Resource Sharing:
Subnets within the VPC are the primary resources shared with participant accounts. Once a subnet is shared, participants can deploy their resources, such as EC2 instances, RDS databases, and Lambda functions, within those subnets.
However, some resources, like the VPC itself, internet gateways, and transit gateways, are inherently shared and cannot be explicitly shared with participants.

### Security and Isolation:
Each AWS account (both owner and participants) maintains its own set of security groups, network ACLs, and route tables.

This ensures that resources launched by one account are isolated from resources launched by another account, even if they are in the same subnet.

The VPC owner can also set up Resource Access Manager (RAM) permissions to control which accounts can access the shared VPC and its subnets.

### Service Limitations:
While many AWS services support VPC sharing, there are some limitations. For instance, as of my last update in September 2021, services like AWS Direct Connect and VPC Traffic Mirroring do not support shared VPCs. It's essential to check the AWS documentation for the most up-to-date list of supported services.

### Billing:
Each participant account is billed for the AWS resources it operates within the shared VPC. The VPC owner is not billed for the participant's resources.

### Use Cases:
**Centralized Network Architecture**: Organizations can centralize their network design, making it easier to manage and monitor.
**Multi-account Strategies**: Organizations using AWS Organizations can easily share a VPC across accounts, ensuring consistent network setup.
**Service Limit Workarounds**: Some AWS services have per-VPC limits. By sharing a VPC, organizations can effectively bypass some of these limits.

### Considerations:
Proper planning is crucial before implementing VPC sharing, especially concerning security and resource management. It's essential to ensure that sharing does not inadvertently expose resources or create conflicts between accounts.

## VPC Peering
### Definition and Functionality:
A VPC peering connection allows two VPCs to communicate privately, making instances in both VPCs feel as if they're in the same network.

This private connection ensures that traffic between the VPCs doesn't traverse the public internet.

### Flexibility:
VPC peering can be established between:

- Two VPCs in the same AWS account.
- VPCs in different AWS accounts.
- VPCs in different AWS regions.

### Configuration:
To enable communication between the peered VPCs, you must update the route tables of each VPC.

The route table for each VPC should have a route pointing to the IP address range (CIDR block) of the other VPC, with the target set as the VPC peering connection ID.

### Restrictions:
**Non-overlapping IP Addresses**: The CIDR blocks of the VPCs being peered cannot overlap. If they do, the peering request will be rejected.

**Non-transitive Nature**: If VPC A is peered with VPC B and VPC A is also peered with VPC C, it doesn't mean VPC B can communicate with VPC C. Each peering connection is direct and non-transitive. To allow VPC B and VPC C to communicate, a separate peering connection must be established between them.

**Unique Peering Connection**: There can only be one active peering connection between two specific VPCs. If a peering connection is deleted, a new one can be created, but two VPCs can't have multiple active peering connections simultaneously.

## AWS Site-to-Site VPN
By following these steps, you can securely extend your on-premises network to your AWS VPC, creating a hybrid environment. This allows resources in AWS and your corporate data center to communicate as if they were on the same network.

### Create a VPN Gateway:
A Virtual Private Network (VPN) gateway is a virtual router that connects a VPC to a VPN connection. By creating and attaching a VPN gateway to your VPC, you're setting up the AWS side of the VPN connection.

### Define the Customer Gateway Configuration:
The Customer Gateway is a logical representation of your VPN device in AWS. While it's not a physical or virtual device, it contains details about your VPN device, such as its public-facing IP address and the type of device.

This step is crucial because AWS uses this information to communicate with your VPN device and establish the VPN connection.

### Update Route Table and Security Group Rules:
**Route Table**: Once the VPN connection is established, you need to ensure that traffic destined for your corporate data center (or other remote network) is routed through the VPN gateway. This is done by adding specific routes to your VPC's route table.

**Security Groups**: These act as virtual firewalls for your instances to control inbound and outbound traffic. You might need to update security group rules to allow traffic from your corporate data center to reach instances in your VPC and vice versa.

### Establish the Site-to-Site VPN Connection:
With the VPN gateway and customer gateway defined, you can now create the Site-to-Site VPN connection. This connection consists of two VPN tunnels for redundancy. If one tunnel becomes unavailable, traffic will automatically switch to the other tunnel.

AWS provides configuration scripts and details for various VPN devices to help you set up the connection on your side.

### Configure Routing on the Remote Network:
While the previous steps ensure that traffic from your VPC can reach your corporate data center, you also need to ensure that the reverse is true. This means configuring your on-premises VPN device and routers to route traffic destined for your VPC's IP range through the VPN connection.

## AWS Direct Connect
In essence, AWS Direct Connect is a solution for businesses that prioritize consistent network performance, security, and cost efficiency. It's especially valuable for enterprises with significant data transfer needs or those looking to establish a hybrid cloud environment.

### Network Performance Challenges:
The farther your data center is from the AWS Region, the higher the latency due to the increased distance data must travel. This can lead to slower response times and reduced application performance.

Internet-based connections can be inconsistent due to network congestion, leading to variable performance.

### Dedicated and Private Connection with DX:
AWS Direct Connect provides a dedicated line, meaning the connection is exclusively for your use. This contrasts with typical internet connections, which are shared among many users.

Being private, the connection doesn't traverse the public internet. This not only enhances security but also ensures a more consistent network experience.

### Cost Efficiency:
With DX, data transfer costs can be lower compared to transferring data over the internet, especially if you're moving large volumes of data.

It can also reduce the need for high-speed internet connections, leading to further savings.

### Increased Bandwidth Throughput:
DX allows for faster data transfer rates, especially beneficial for workloads that require significant data movement, such as data migration, backup, or real-time analytics.
### Consistent Network Experience:
By bypassing the public internet, DX avoids its inherent variability and congestion, leading to a more predictable and consistent network experience.
### Use of 802.1q VLANs:
AWS Direct Connect uses the 802.1q VLAN standard, which allows multiple virtual interfaces to be created on a single DX connection. This means you can use the same DX connection to access public resources (like Amazon S3) and private resources (like EC2 instances in a VPC) by segregating them into different VLANs.

## VPC Endpoints
### Definition of VPC Endpoint:
A VPC endpoint is essentially a virtual gateway allowing for private connections between your VPC and supported AWS services without the need to traverse the public internet.
### Benefits:
**Security**: Since traffic doesn't traverse the public internet, it's less exposed to potential threats.

**Efficiency**: The direct connection can lead to faster and more consistent data transfer speeds.

**Cost-Effective**: Eliminates the need for NAT or VPN devices for specific AWS service access, potentially reducing costs.

### Types of VPC Endpoints:
#### Interface VPC Endpoint (Interface Endpoint):
- Powered by AWS PrivateLink.
- Provides private connectivity to AWS services, third-party services in AWS Marketplace, and services hosted by other AWS customers or APN Partners.
- Charges apply for creating and using an interface endpoint, including hourly usage and data processing rates.
- Interface endpoints create an Elastic Network Interface (ENI) in a specified subnet with a private IP address, allowing communication with the service using Amazon's private network.
#### Gateway Endpoint:
- Specifically designed for Amazon S3 and DynamoDB.
- Acts as a target for a specific route in your route table, directing traffic to the supported AWS service.
- There's no additional charge for using gateway endpoints, but standard data transfer and resource usage charges apply.
### Billing and Costs:
As you mentioned, while gateway endpoints have no additional charges, interface endpoints come with associated costs. It's essential to monitor these costs, especially if you have multiple interface endpoints or high data transfer volumes.

## AWS Transit Gateway
### Complexity in Scaling VPCs:
As organizations grow, they often end up with numerous VPCs spread across different AWS accounts and regions, catering to various teams, projects, and business lines.

The native connectivity options in AWS, like VPC peering, internet gateways, NAT gateways, and AWS Direct Connect, are point-to-point. This means that as the number of VPCs grows, managing these connections becomes increasingly complex.

### Challenges with Point-to-Point Connectivity:
VPC peering, while useful for connecting pairs of VPCs, can become operationally challenging and costly when dealing with multiple VPCs. This is because each connection has to be managed individually.

For on-premises connectivity, connecting a VPN to each VPC individually is time-consuming and becomes harder to manage as the number of VPCs increases.

### AWS Transit Gateway as a Solution:
AWS Transit Gateway offers a centralized approach to manage connectivity, acting as a hub in a hub-and-spoke model.

Instead of managing multiple point-to-point connections, you only need to manage connections between the Transit Gateway (hub) and each VPC or on-premises network (spokes).

This model simplifies the management of network connections, reduces operational overhead, and makes it easier to enforce consistent network and security policies.

### Benefits of Using AWS Transit Gateway:
**Simplified Management**: Only one connection is needed from each VPC or on-premises network to the Transit Gateway.
Reduced Operational Costs: With fewer connections to manage and a centralized routing table, operational overhead is significantly reduced.

**Scalability**: As you add new VPCs or on-premises networks, you simply connect them to the Transit Gateway. They automatically gain access to all other networks connected to the Transit Gateway, making scaling more straightforward and efficient.

**Consistent Policies**: Centralized routing and connectivity make it easier to apply consistent security and compliance policies across your entire network infrastructure.

## Section 3 Takeaways
There are several VPC networking options, which include:

    •Internet gateway: Connects your VPC to the internet
    •NAT gateway: Enables instances in a private subnet to connect to the internet
    •VPC endpoint: Connects your VPC to supported AWS services
    •VPC peering: Connects your VPC to other VPCs
    •VPC sharing: Allows multiple AWS accounts to create their application resources into shared, centrally-managed Amazon VPCs
    •AWS Site-to-Site VPN: Connects your VPC to remote networks
    •AWS Direct Connect: Connects your VPC to a remote network by using a dedicated network connection
    •AWS Transit Gateway: A hub-and-spoke connection alternative to VPC peering
You can use the VPC Wizard to implement your design


# VPC Security
# Amazon Route 53
# Amazon CloudFront