<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# VPC Peering

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-peering)

**Author:** Bao Luong  
**Email:** baodevops21@gmail.com

---

## VPC Peering

![Image](http://learn.nextwork.org/stimulated_brown_festive_kaffir_lime/uploads/aws-networks-peering_88727bef)

---

## Introducing Today's Project!

### What is Amazon VPC?

(Virtual Private Cloud) is a service that lets you provision a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define. Think of it as your own private, isolated network within AWS.

It's incredibly useful because it gives you complete control over your virtual networking environment, including your own IP address range, subnets, route tables, and network gateways.

### How I used Amazon VPC in this project

In today's project, I used Amazon VPC to established the peering connection between them, troubleshoot connection errors, and ran Ping command test. 

### One thing I didn't expect in this project was...

One thing I didn't expect in this project was is that the default security group for a new VPC does not allow incoming traffic from outside of the VPC by default. This means you have to explicitly allow inbound SSH traffic on port 22 yourself!

### This project took me...

This project took me about two hours to complete.

---

## In the first part of my project...

### Step 1 - Set up my VPC

In this step, I will use the visual VPC resource map to create the twoVPCs from stratch.

### Step 2 - Create a Peering Connection

In this step, I will set up the VPC peering to establish the connection link and bridge them together with a peering connection.

### Step 3 - Update Route Tables

In this step, I will set up a way for traffic to flow between VPC1 and VPC2. 

### Step 4 - Launch EC2 Instances

In this step, I will launch an EC2 instance in each VPC, so we can use them to test the VPC peering connection later.

---

## Multi-VPC Architecture

I started my project by launching 2 VPCs with one subnet in each. 

The CIDR blocks for VPCs 1 and 2 are different, 10.1.0.0/16
and 10.2.0.0/16. They have to be unique so that the IP addresses of their resources don't overlap. Having overlapping IP addresses could cause routing conflicts and connectivity issues!

### I also launched 2 EC2 instances

I didn't set up key pairs for these EC2 instances because we learnt that with EC2 Instance Connect, AWS actually manages a key pair for us! We don't need to manage key pairs ourselves. Since we've already learnt how to set up key pairs twice in the last two projects, we don't need to do it again this time.

![Image](http://learn.nextwork.org/stimulated_brown_festive_kaffir_lime/uploads/aws-networks-peering_11111111)

---

## VPC Peering

A VPC peering connection is  a direct connection between two VPCs.

VPCs would use peering connections to let the VPCs and their resources route traffic between them using their private IP addresses. This means data can now be transferred between VPCs without going through the public internet.

Without a peering connection, data transfers between VPCs would use resources' public address - meaning VPCs have to communicate over the public internet. 

The Requester is the VPC that initiates the peering connection and sends an invitation to connect.

The Accepter is the VPC that receives the peering connection request and can either accept or decline the invitation. The connection is only established if the Accepter approves it.

![Image](http://learn.nextwork.org/stimulated_brown_festive_kaffir_lime/uploads/aws-networks-peering_1cbb1b88)

---

## Updating route tables

After accepting a peering connection, my VPCs' route tables need to be updated because traffic in VPC 1 won't know how to get to resources in VPC 2 without a route in your route table! You need to set up a route that directs traffic bound for VPC 2 to the peering connection you've set up.

My VPCs' new routes have a destination of 10.1.0.0/16 and 10.2.0.0/16 for VPC 1 and VPC 2 respectively. 

![Image](http://learn.nextwork.org/stimulated_brown_festive_kaffir_lime/uploads/aws-networks-peering_4a9e8014)

---

## In the second part of my project...

### Step 5 - Use EC2 Instance Connect

In this step, I will test our VPC peering connection, we'll need to get one of our EC2 instances to try talk to the other.

### Step 6 - Connect to EC2 Instance 1

In this step, I will use EC2 Instance Connect to connect to Instance 1.

### Step 7 - Test VPC Peering

In this step, I will get Instance 1 to send test messages to Instance 2. Solve connection errors until Instance 2 is able to send messages back.



---

## Troubleshooting Instance Connect

Next, I used EC2 Instance Connect to Instance - NextWork VPC 1.

I was stopped from using EC2 Instance Connect as no public IPv4 address assigned. With no public IPv4 address, you can't use EC2 Instance Connect

![Image](http://learn.nextwork.org/stimulated_brown_festive_kaffir_lime/uploads/aws-networks-peering_7685490c)

---

## Elastic IP addresses

To resolve this error, I set up Elastic IP addresses. Elastic IP addresses are static IPv4 addresses that get allocated to your AWS account, and is yours to delegate to an EC2 instance.

An Elastic IP being static is a key word here! EC2 instances by default have dynamic IPs, which means their Public IPv4 addresses change every time they're restarted. Having an Elastic IP is like having a permanent address in a city, instead of having to move from location to location every time your instance restarts.

Associating an Elastic IP address resolved the error because it provided the necessary public IPv4 address, allowing the EC2 instance to establish a connection.

![Image](http://learn.nextwork.org/stimulated_brown_festive_kaffir_lime/uploads/aws-networks-peering_45663498)

---

## Troubleshooting ping issues

To test VPC peering, I ran the command ping.

A successful ping test would validate my VPC peering connection because it is confirming that the instances in different VPCs can communicate directly using their private IP addresses.

I had to update my second EC2 instance's security group because the connection error indicates that the default security group only only inbound traffic from within the VPC while blocking the internet traffic needed for EC2 Instance Connect. I added a new rule that allow SSH and set source to Anywhere-IPv4. 

![Image](http://learn.nextwork.org/stimulated_brown_festive_kaffir_lime/uploads/aws-networks-peering_7a29d352)

---

---
