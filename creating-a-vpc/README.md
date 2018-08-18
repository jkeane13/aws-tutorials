# Creating a VPC

## Learning networkig in the cloud...

One of the great things about AWS is the ease it is to build up secure networks that are easy to build and secure. 

## Prequisistes

### An AWS Account
You will need an AWS account to be able to build a network. While the VPC, Subnets, Route Tables are all free, the Load Balancer, Elastic IP NAT and any testing EC2 instances are not. After finishing this guide, if you don't use it, tear it down to avoid any substantial cost

## Building a VPC
<img align="right" src="img/VPC%20Part%201.png" width="300" height="300">A VPC is an network where your instances are going to live.
Create a VPC and give it a CIDR range of `10.0.0.0/16`. This will give you a lot of available network space to build out your Network

# Network to be accessed by the outside world 
Now that you have a place where you instances are going to live. We need to create a sub-network inside a network (subnet) where instances you want to be access to the outside world can be accessed

- Go to Subnets
- use it to 10.0.1.0./24

Any instances put into this network will be available to the outside world. But how does it get to the outside world?

## An internet gateway!
An internet gateway to a point in a VPC where you tell you subnet where the gate is to the outside world. All network you want to expose to the outside world, needs an internet gateway!

To create one to do 

- Add an internet gateway and attach it to the VPC

### How will the subnet know how connect to the outside world?

You will needo point a public subnet to the internet gateway our it won't know where it will access the internet and 'route' to it. You can do that through an route table! We can use the default table to create

- Create a route in the default route table to 0.0.0.0/0 - igw (Internet Gateway)

## Lets test it out!

Let's see if we create an EC2 instance is accessable create in EC2 instance, and open up the security groupport 22 and 80 with the following UserData settings

```
yum update
yum install -y httpd
echo "Hello Web server!" > /var/www/html
systemctl httpd start
```

Go to the web address under EC2 instances. You you will be able to connect

## But what if I want private network that can only be accessed from the inside VPC?

You may want to create another subnet that you don't want to be exposed to the outside world, in order to create it

- Create another subnet for private instances - 10.0.2.0/24


## Let's test it out! 
Build a instance to put it into the private subnet. Notice a public IP is not assigned and the subnet isn't assigned a IP

## If it can't get to the outside world... how can I connect to the internet?
You may have to run updates or download software, there needs to be way I can do that through the network 

### Enter Network Access Translation Gateway! (NAT)
- Create a NAT gateway in the public subnet. It will ask for a Elastic IP address. An Elastic IP address is a always changing public IP address which will give your private instances a way to securly send traffic in and out a network

-  Edit the default route table to 0.0.0.0/0

### Test it out!
- Remote back into the instance are you should be able to do updates. 

## Congratulations!
Congratulations! You have created a network with a public and private subnet! Every instance in the public instance can be access by the outside world!

## Locking it down... 
To make sure people can't attack open ports on a network. We need to lock down the network so malicious hackers can't access things they shouldn't.

### Network Access Control Lists

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

