---
title: "Demystifying AWS VPC Key Concepts."
seoTitle: "Mastering AWS VPC: The Complete Guide to Virtual Private Cloud (VPC)"
seoDescription: "Unlock the power of AWS VPC networking with our comprehensive guide. Learn everything you need to know about Virtual Private Cloud (VPC) setup configuration"
datePublished: Sat Jul 15 2023 18:28:13 GMT+0000 (Coordinated Universal Time)
cuid: clk4ccfyn000g08l8e42mbp95
slug: aws-vpc
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1689445246448/8632555a-56c8-423a-a8fc-ef27aefeb76b.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1689445677281/377b8b1e-d84d-4327-9073-64d54738b59b.png
tags: aws, vpc, aws-certified-solutions-architect-associate, cloudengineer, vpc-basics

---

## What is VPC

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689443842073/a59bf592-632b-45c2-b10e-5e42d8e0f676.png align="center")

VPC stands for virtual private cloud, which allows the hosting of AWS resources in a logically isolated virtual network.

<mark>By default, AWS provides a default Virtual Private Cloud (VPC) in each region. This is the reason why we can directly start an EC2 instance, enabling users to quickly launch and run EC2 instances without having to manually set up networking components.</mark>

## CIDR

***Classless Inter-Domain Routing is a key concept behind VPC subnetting and for network isolation. This helps to allocate the IPs efficiently.***

1. Subnetting: CIDR enables subnetting, which involves dividing a larger network into smaller subnetworks or subnets. Each subnet has its own network prefix, allowing for better organization and management of IP addresses within a network.
    
2. IP Address Range Calculation: CIDR notation provides a concise and standardized way to represent IP address ranges. By specifying the network prefix, it becomes easier to calculate the range of IP addresses available within a given network.
    
3. The most commonly used internal IP address range is defined by the Internet Assigned Numbers Authority (IANA) and is known as IPv4 (Internet Protocol version 4) private address space. The IPv4 private address ranges are as follows:
    
    1. 10.0.0.0 to 10.255.255.255 (10.0.0.0/8), a range that provides up to 16 million unique IP addresses.
        
    2. 172.16.0.0 to 172.31.255.255 (172.16.0.0/12), providing about 1 million unique IP addresses.
        
    3. 192.168.0.0 to 192.168.255.255 (192.168.0.0/16), which offers about 65,000 unique IP addresses.
        
    
    <div data-node-type="callout">
    <div data-node-type="callout-emoji">💡</div>
    <div data-node-type="callout-text"><em><mark>Use this for calculating the IP address ranges for the VPC. </mark></em><a target="_blank" rel="noopener noreferrer nofollow" href="https://www.ipaddressguide.com/￼In" style="pointer-events: none"><em><mark>https://www.ipaddressguide.com/</mark></em></a></div>
    </div>
    
4. <mark>In AWS, the largest CIDR block range you can allocate for a virtual private cloud (VPC) is a /16 CIDR block. A /16 CIDR block actually provides a total of 65,536 out of this total IPs AWS reserves 5 IP addresses within each subnet, the first four IP addresses in each subnet are reserved for network addressing purposes, and the fifth IP address is reserved for Amazon DNS resolution.</mark>
    

## Subnet

In the context of AWS, a subnet is a range of IP addresses in a VPC. When you create a VPC, you can divide it into multiple subnets, each with its own IP address range. <mark>Subnets are typically associated with a specific availability zone (AZ) within an AWS region.</mark>

**Here are a few key points about subnets in AWS:**

1. IP Address Range: Each subnet has its own IP address range, which is a subset of the IP address range of the VPC. The IP address range is specified using CIDR notation, such as 10.0.0.0/24, indicating a subnet with 256 IP addresses.
    
2. Availability Zones: Subnets are associated with a specific availability zone within an AWS region. This allows for deploying resources in different availability zones to achieve high availability and fault tolerance.
    
3. Routing: Each subnet has its own route table, which controls the traffic flow within the subnet and between subnets. You can configure routing rules to direct traffic between subnets or to external networks.
    
4. Security: Subnets can be associated with security groups and network access control lists (NACLs) to control inbound and outbound traffic at the subnet level.
    
5. Purpose: Subnets are often used to logically separate different components or tiers of an application or system. For example, you might have separate subnets for web servers, application servers, and database servers.
    

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text"><mark>By dividing a VPC into subnets, you can create a segmented network architecture that provides better security, manageability, and flexibility for deploying and managing your resources within AWS.</mark></div>
</div>

## Routing

Use route tables to determine where network traffic from your subnet or gateway is directed.

Route tables act as a rule book and direct the traffic internally between the subnets or to the internet.

<mark>A route table can be associated with multiple subnets. However, a subnet can only be associated with one route table at a time.</mark> Any subnet not explicitly associated with a table is implicitly associated with the main route table by default.  
We can create one common route table and associate multiple common subnets with it.

### What makes your subnet public or private?

This is one of the key questions we get when working with public and private subnets, here route table plays a crucial role in determining whether a subnet is public or private in AWS.

<mark>The Internet Gateway facilitates connectivity between a subnet and the Internet. However, simply creating and attaching the Internet Gateway to a subnet does not automatically make the subnet public. </mark> ***<mark>To make a subnet public, you need to configure the associated route table to direct all public IP traffic to the Internet Gateway.</mark>***

### Difference between Gateways and Endpoints

| **<mark>Internet Gateway</mark>** | <mark>VPC Endpoint</mark> |
| --- | --- |
| An Internet Gateway is a horizontally scalable, highly available AWS-managed service that enables communication between your VPC and the Internet. | A VPC Endpoint is a highly available and horizontally scalable service that allows you to establish a private connection between your VPC and AWS services, eliminating the need to access them over the public internet. |
| By attaching an Internet Gateway to your VPC, instances within the VPC can communicate with the Internet and be accessed from the Internet. | It allows secure and direct communication between your VPC and supported AWS services (such as S3, DynamoDB, or SNS) without the need for an internet gateway, NAT device, or public IP address. |
| An Internet Gateway serves as a gateway for Internet-bound traffic, allowing resources within the VPC to access the Internet or be accessed from the Internet. | With VPC endpoints, data between your VPC and the AWS service remains within the AWS network, improving security and reducing data transfer costs. |
| It performs network address translation (NAT), allowing instances in the VPC with private IP addresses to communicate with the internet using public IP addresses | VPC endpoints can be interface endpoints or gateway endpoints, depending on the AWS service you are connecting to. |

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text"><em><mark>In summary, an Internet Gateway connects your VPC to the Internet, enabling inbound and outbound Internet connectivity for resources within the VPC. On the other hand, VPC endpoints allow you to privately connect your VPC with specific AWS services without using the public internet, enhancing security and reducing data transfer costs.</mark></em></div>
</div>

### Transit gateways

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689440808439/609ae5a1-1570-4347-91be-70bb943e4cc3.png align="center")

**The transit gateway acts as a central hub, to route traffic between your VPCs, VPN connections, and AWS Direct Connect connections.**

1. If we have 2 VPCs we can connect them by using VPC peering, but if we have more VPCs in order to peer multiple VPCs with each other, the number of connections would be based on the formula ***\[n(n-1)/2\].*** For example, if you have a total of 5 VPCs in your network and all need peering then the total connections would be 10.
    
2. By using Transit gateways, we can consolidate and manage network routing and connectivity in a more efficient and scalable manner. <mark>Instead of maintaining the network routing for each VPC, we can manage at a single point.</mark>
    

## NACLs and Security groups

| <mark>Security Groups:</mark> | <mark>Network Access Control Lists (NACLs):</mark> |
| --- | --- |
| Operate at the instance level (within a VPC) and are stateful. | Operate at the subnet level (within a VPC) and are stateless. |
| Control inbound and outbound traffic based on security group rules. | Control inbound and outbound traffic based on numbered rules. |
| Associate with instances and provide security at the protocol and port level. | Apply to all instances within the associated subnet. |
| Automatically allow outbound traffic for established connections. | Support allow and deny rules for both inbound and outbound traffic. |

## Elastic IP

An Elastic IP (EIP) is a static, public IPv4 address that you can allocate and assign to your AWS resources such as EC2 instances, NAT gateways, or load balancers. Unlike the dynamic public IP addresses assigned by default to EC2 instances, an EIP remains associated with your AWS account until you release it.

Elastic IPs will not change until and unless you release them. So resources associated with elastic IPs will have the same IP address even after restarting.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text"><mark>Considering the cost optimization, make sure to release Elastic IP if not in use to avoid unwanted billing.</mark></div>
</div>

## NAT Gateway

**<mark>The NAT Gateway is typically placed within a public subnet</mark>** in your Amazon VPC. The NAT gateway allows private instances to connect internet but blocks the inbound connection from the network, It's a one-way communication.

By placing the NAT Gateway in a public subnet and configuring the appropriate routing, you enable instances in private subnets to access the internet for tasks such as installing external packages and performing upgrades, while effectively blocking incoming requests. This setup ensures a secure environment for private instances, allowing them to communicate with external resources as needed.

*NAT gateways in AWS require an Elastic IP address (EIP) to function properly. An Elastic IP address is a static public IP address that you can allocate and associate with your NAT gateway.*

*So, when setting up a NAT gateway in AWS, ensure you have an available Elastic IP address to associate with it.*

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text"><mark>Make sure to remove/delete the Elastic IP if not required or not in use, because AWS imposes charges for Elastic IP addresses under certain circumstances.</mark></div>
</div>

### Bastion Host in AWS VPC

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1689442254203/87095ab2-2cb3-4290-9c8e-3988b6707d48.png align="center")

A bastion host is an EC2 instance that acts as a secure gateway to access private instances within an AWS VPC (Virtual Private Cloud) environment. <mark>It is a dedicated server that provides secure access to resources in the private network from outside the VPC.</mark>

We can follow agent forwarding for connecting to any instance that is using the same key pair, this is also known as SSH agent forwarding.

<div data-node-type="callout">
<div data-node-type="callout-emoji">💡</div>
<div data-node-type="callout-text">I found this beautiful article on connecting to private instances using bastion host. <a target="_blank" rel="noopener noreferrer nofollow" href="https://digitalcloud.training/ssh-into-ec2-in-private-subnet/#what8217s-ssh-and-how-is-it-used-with-amazon-ec2-instances" style="pointer-events: none">Blog</a></div>
</div>

## Conclusion:

These are the few key concepts that we need to understand in AWS VPC. Also, have attached some resources that I referred to my learning. Please let me know if I am missing anything.

## Resources:

* [VPC Peering Lab](https://catalog.workshops.aws/networking/en-US/beginner/lab1/020-vpcpeering)
    
* [Transit Gateway](https://catalog.workshops.aws/networking/en-US/beginner/lab1/030-tgw)
    
* [VPC peering, VPN connection, and Direct connect.](https://medium.com/geekculture/aws-vpc-peering-vpn-connection-and-direct-connect-abf1b3e533a)
    
* [Subnet Mask](https://youtu.be/s_Ntt6eTn94)
    
* [CIDR](https://youtu.be/N-ywmOpWehE)
    
* [EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/managing-users.html)
    
* [Connect to EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connection-prereqs.html)
    
* [Bastion Host in AWS VPC](https://dev.to/aws-builders/bastion-host-in-aws-vpc-2i63)
    
* [Securing your bastion hosts with Amazon EC2 Instance Connect](https://digitalcloud.training/ssh-into-ec2-in-private-subnet/#what8217s-ssh-and-how-is-it-used-with-amazon-ec2-instances)
    
* [SSH agent forwarding](https://youtu.be/cHg6LWOzH98)