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
* Enables you to provision a logically isolated section of the aws cloud where you can launch aws resources in a virtual network that you define.
  * Gives control over IP range selection, creation of subnets, and configuration of route tables and network gateways.
* Logically isolated from other VPCs, belonging to a single AWS Region and can span multiple Availability Zones
* Subnets: 
  * Range of IP addresses that divide a VPC.
  * They belong to a single Availability Zone and are classified as public or private.
  * When creating a VPC, you assign it to an IPv4 CIDR block (range of private IPv4 addresses) and you cant change the range after creation.
    * The largest IPv4 CIDR block size is /16 or 65,536 addresses
    * The smallest is /28 or 16 addresses
  * When you create a subnet, it requires its own CIDR block. For each CIDR block that you specify, AWS reserves 5 IPs within that block for:
    * Network address
    * VPC local router (internal comms)
    * Domain Name System (DNS) resolution
    * Future use
    * Network broadcast address
* Route Tables & Routes
  * A route table contains a set of rules that you can configure to direct network traffic from your subnet.
    * Each route specifies a destination and a target, and by default, every route table contains a local route for comms within the VPC. 
    * Each subnet must be associated with, at most, one route table.
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
# VPC Security
# Amazon Route 53
# Amazon CloudFront