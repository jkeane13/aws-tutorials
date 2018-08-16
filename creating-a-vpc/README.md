# Creating a VPC

 Creating a VPC

1. Create a VPC - CIDR 10.0.0.0/16. By default a route table, network acl with full access and a security group will be created.

<p align="center"><src="https://github.com/jkeane13/aws-tutorials/tree/master/creating-a-vpc/img/VPC%20Part%201.png"></p>

2. Create a subnet for public instances - 10.0.1.0/24

3. Create another subnet for private instances - 10.0.2.0/24

4. Add an internet gateway

5. Attach the internet gateway to the VPC

6. Create a route in the default route table to 0.0.0.0/0 - igw (Internet Gateway)

7. Add the public subnet to the newly create route table

8. Set the public subnet to assign public IP addresses

9. Create an EC2 in the public subnet, with a security to allow port 22 traffic for testing through SSH

10. Create a EC2 in the private subnet, with a security group to allow port 22 from *inside the VPC at 10.0.0.0/16*

11. SSH into the public subnet into the EC2 instance to test network conectivity

13. Create a NAT gateway and assign an Elastic IP address

14. Edit the default route table to 0.0.0.0/0

15. Check internet on the private instance.

## Network ACL
Create a NACL. By default, every port will be *denied*. Remember NACLS are _stateless_, meaning you will need to put in inbound and outbound rules

## Example Network ACL
### Inbound
|Rule| Protocol | Port         | Source IP | Permission |
|----|----------|--------------|-----------|------------|
|100 | HTTP     | 80           | 0.0.0.0/0 |ALLOW       |
|200 | HTTPS    | 443          | 0.0.0.0/0 |ALLOW       |
|300 | SSH      | 22           | 0.0.0.0/0 |ALLOW       |
|400 | Custom   | 1024 - 65535 | 0.0.0.0/0 |ALLOW       |

### Outbound
|Rule| Protocol | Port         | Source IP | Permission |
|----|----------|--------------|-----------|------------|
|100 | HTTP     | 80           | 0.0.0.0/0 |ALLOW       |
|200 | HTTPS    | 443          | 0.0.0.0/0 |ALLOW       |
|300 | SSH      | 22           | 0.0.0.0/0 |ALLOW       |
|400 | Custom   | 1024 - 65535 | 0.0.0.0/0 |ALLOW       |

Set the Subnet Association of the NACL to the Public Subnet
