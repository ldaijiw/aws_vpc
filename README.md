# Virtual Private Cloud

Amazon Virtual Private Cloud (Amazon VPC) allow a user to provision a logically isolated section of the AWS Cloud, allowing to launch AWS resources in a virtual defined network 

Complete control over virtual networking environment:
- Selection of IP address range
- Creation of subnets
- Configuration of route tables and network gateways
- Can use both IPv4 and IPv6 in VPC for secure and easy access to resources and applications
- Multiple layers of security, with security groups and network access control lists (NACL) to help control access to EC2 instances in each subnet

Can create a public-facing subnet for web servers that have access to the internet, with the backend systems, such as databases or application servers, in a private-facing subnet with no internet access



## Configuration of a VPC

### Create VPC

- Use a sensible _Name tag_
- IPv4 CIDR Block (``x.x.0.0/16``)

### Create Internet Gateway

- Use sensible naming
- Attach to newly create VPC

### Create Subnets

- After selecting VPC to create subnets in
- Availability zone is optional

**PUBLIC**
- IPv4 CIDR Block (``x.x.1.0/24``)

**PRIVATE**
- IPv4 CIDR Block (``x.x.2.0/24``)

### Route Tables

Select the VPC when creating a route table

**PUBLIC**
- Allow internet access (``0.0.0.0/0``) with target set to IGW
- Associate to Public Subnet

**PRIVATE**
- Temporarily allow internet access for sake of setting up database, but once completed remove this route
- Associate to Private Subnet

### Network ACL

- Select VPC to create network ACL for and select appropriate subnet association
- Rule #s convention start from 100 and increment by 10 (120, 130...)

**PUBLIC**
- Inbound
    - HTTP (80) from any source
    - HTTPS (443) from any source
    - SSH (22) from my ip
    - (For specific project) Port 3000 from my ip
    - Port 1024-65535 from any source for ephemeral ports

- Outbound
    - HTTP (80) to any source
    - HTTPS (443) to any source
    - SSH (22) to any source
    - Port 27017 to Private subnet for MongoDB
    - Port 1024-65535 to any source for ephemeral ports

**PRIVATE**

After set up is complete only necessary rules should be
- Inbound
    - Port 27017 from Public subnet for MongoDB

- Outbound
    - Port 1024-65535 to any source for ephemeral ports

### Creating EC2 Instances

Create EC2 instances as normal (will need to create new SG) but making sure to select correct VPC and subnet in configuration