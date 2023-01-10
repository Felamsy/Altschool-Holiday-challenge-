# ALTSCHOOL AFRICA

## Holiday Project

## STEPS

1.  Creating a vpc with private and public subnets
2.  Creating private instances in the private subnet of the vpc
3.  Creating a bastion host.
4.  Installation and Configuration of Nginx Server on Private Instances Using Ansible
5.  Creation of target groups
6.  Creating an application load balancer.

#### Ansible is an open-source automation tool that allows you to configure, manage, and deploy software on various systems. It uses a simple, human-readable language called YAML to describe the desired state of the systems it is managing, and then communicates with those systems over SSH to make the necessary changes to bring them into compliance with the desired state.

#### One of the key benefits of Ansible is that it doesn't require a agents to be installed on the target systems, it only needs a ssh connection, that means it is agentless. This makes it very easy to get started with, as there is no software to install or configure on the remote systems.

#### Ansible is often used to manage large numbers of servers, network devices, and cloud resources, but it can be used to automate just about any task that can be scripted. It can be used to automate tasks such as:

1. Installing and configuring software
2. Managing system users and permissions
3. Managing services and daemons
4. Managing network devices
5. Creating and managing cloud resources
6. Ansible also provides a powerful set of tools for managing and organizing automation tasks, including roles and playbooks, which allow you to organize your automation tasks into reusable and modular units.

#### It is used in wide range of companies and industries, from small startup to very large enterprises.

## What is a VPC?

A Virtual Private Cloud (VPC) is a virtual network in the cloud that is dedicated to your AWS account. A VPC allows you to launch Amazon Elastic Compute Cloud (EC2) instances and RDS instances, as well as other resources, into a virtual network that you've defined.

You can customize the network configuration for your VPC. For example, you can create a public-facing subnet for your web servers, and a private-facing subnet for your database servers. You can also configure security and access control for your VPC. For example, you can create a security group that allows specific inbound traffic, and assign that security group to your web servers.

VPCs allow you to have a logically isolated section of the AWS cloud where you can launch AWS resources in a virtual network that you've defined. This virtual network closely resembles a traditional network that you'd operate in your own data center, with the benefits of using the scalable infrastructure of AWS.

It gives you complete control over your virtual networking environment, including selection of your own IP address range, creation of subnets, and configuration of route tables and network gateways.

## what’s a subnet?

In computer networking, a subnet (short for "subnetwork") is a division of an IP network. The IP protocol is designed for use in interconnected systems of packet-switched computer communication networks. In the Internet Protocol (IP) version 4, the address space is divided into four octets, or 32 bits, and each address is written in the form "A.B.C.D" where A, B, C, and D are decimal values between 0 and 255.

Subnetting is the process of allocating a portion of this address space to specific hosts or groups of hosts. This is done by dividing the 32-bit IP address into two parts: the network address and the host address. The network address identifies the network itself, while the host address identifies a specific host within that network. The division between the network address and the host address is indicated by a subnet mask, which is a 32-bit value that separates the IP address into the network and host portions.

For example, in the IP address 192.168.0.1 with a subnet mask of 255.255.255.0, the first three octets (192.168.0) make up the network address, and the last octet (1) represents the host address. This would create a subnet that can contain up to 254 unique IP addresses.

Subnetting allows organizations to break up their larger network into smaller subnetworks, which can help improve network security and organization, and can also help to reduce network congestion.

IPv6 also uses subnetting. But it use a different notation and addressing scheme. In IPv6, the address space is 128 bits, and the address is written in the form "x:x:x:x:x:x:x:x" where x is a hexadecimal value. subnetting in IPv6 is also a little bit different comparing to IPv4.

## How To Create A VPC With Private and Public Subnets

Creating a Virtual Private Cloud (VPC) with private and public subnets in AWS can be done using the AWS Management Console, AWS Command Line Interface (CLI), or an AWS SDK. Here's an example of how to create a VPC with private and public subnets using the AWS Management Console:

1. Open the AWS Management Console and navigate to the VPC dashboard.

2. Click the "Create VPC" button to open the VPC creation wizard.

3. Enter a name and IP range for your VPC (e.g. "MyVPC" and "10.0.0.0/16"). You can also configure the VPC's DHCP options, tenancy, and IPv6 settings, but the default settings will work for this example.

4. Click the "Create" button to create the VPC.

5. Once the VPC has been created, navigate to the "Subnets" page and click the "Create Subnet" button.

6. Enter a name and IP range for your public subnet (e.g. "PublicSubnet" and "10.0.1.0/24"). Select the VPC you just created and choose a availability zone. You also have option to select the IPv6 CIDR Block, but you can skip that for now.

7. Click the "Create" button to create the public subnet.

8. Repeat steps 5-7 to create a private subnet with a different name and IP range (e.g. "PrivateSubnet" and "10.0.2.0/24").

9. Now, You will have to create a Internet Gateway and attach it to your VPC. Then you have to add a route to the internet from your public subnet.

10. You also need to create a security group for your public subnet, which will allow incoming traffic on ports like HTTP, HTTPS, SSH etc.

11. Create a network ACL for your private subnet and make sure to add rules so that it is only accessible from your public subnet.

You have now created a VPC with a public subnet that can access the internet, and a private subnet that cannot.

You can also use AWS CLI or SDKs to perform the same actions with scripts, which can be useful for automating the process of creating a VPC with private and public subnets.

![Creating VPC](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/vpc1.jpg)
![Creating VPC2](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/vpc2.jpg)
![Creating VPC3](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/vpc3.jpg)
![Creating VPC4](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/creating%20vpc.jpg)
![Created VPC](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/created%20vpc.jpg)
![Subnet Created with VPC](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/subnet%20created%20with%20vpc.jpg)

## Creating our EC2 instance (server)

Yes, once you have created a VPC with private and public subnets, you can then launch an EC2 instance in one of those subnets.
You will have to make sure that the instances are in the correct subnet and has appropriate security group rules that allows the necessary traffic.

Here are the additional steps you can follow to create an EC2 instance in a private subnet:

1. Open the EC2 dashboard in the AWS Management Console.

2. Click the "Launch Instance" button to open the EC2 instance launch wizard.

3. Select the Amazon Machine Image (AMI) that you want to use for your EC2 instance.

4. Choose an instance type that is appropriate for your workload.

5. In the "Configure Instance" page, select the VPC and the subnet that you want to launch the instance in. Also make sure to enable the Auto-assign Public IP option to "Use subnet setting"

6. On the "Add Storage" page, configure the storage for the EC2 instance.

7. On the "Add Tags" page, you can add key-value pairs to help you identify your instance.

8. On the "Configure Security Group" page, you can choose the security group that you created for your private subnet, and add any necessary rules for incoming traffic.

9. Review your settings and click "Launch" to launch the instance.

10. When prompted, select an existing key pair or create a new one.

After the instance is launched, you will not be able to connect to it via the public IP address, since it is in a private subnet and not directly accessible from the internet.
Instead you will have to connect to the instance via VPN or use the bastion host in the public subnet or use the Systems Manager Session Manager to access the instance.

If you're running any services that need to be accessed from outside of the VPC, you will have to configure a NAT Gateway or VPN Connection.

![Creating EC2](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/creating%20ec2.jpg)
![Creating EC2](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/creating%20ec2%201.jpg)
![Creating EC2](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/creating%20ec2%202.jpg)
![Instance type](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/c8667c97e40e39b06b35f506e7e7e4876908df21/images/ec2%20creation%203.jpg)
![Creating KeyPair](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/c8667c97e40e39b06b35f506e7e7e4876908df21/images/creating%20keypair.jpg)
![EC2 Network Settings](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/ec2%20network%20settings.jpg)

#### NOTE:

there will have to be two servers that the load balancer will have to connect to. you need to create the second one.

## Creating a Bastion Host

A bastion host is a special-purpose EC2 instance that is used to securely connect to other instances within your VPC, typically those in a private subnet. Here's an example of how to create a bastion host in a public subnet using the AWS Management Console:

1. Open the EC2 dashboard in the AWS Management Console.

2. Click the "Launch Instance" button to open the EC2 instance launch wizard.

3. Select the Amazon Machine Image (AMI) that you want to use for your bastion host. A Linux distribution like Amazon Linux or Ubuntu LTS is a good choice.

4. Choose an instance type that is appropriate for your workload.
5. In the "Configure Instance" page, select the VPC and the public subnet that you want to launch the bastion host in. Also make sure to enable the Auto-assign Public IP option to "Use subnet setting"

6. On the "Add Storage" page, configure the storage for the EC2 instance.

7. On the "Add Tags" page, you can add key-value pairs to help you identify your instance.

8. On the "Configure Security Group" page, you can choose the security group that you created for your public subnet, and add any necessary rules for incoming traffic, like SSH.

9. Review your settings and click "Launch" to launch the bastion host.

10. When prompted, select an existing key pair or create a new one.

After the bastion host is launched, you can use it to connect to other instances in your private subnet. You can use the bastion host to SSH or RDP into your private instances, or you can use it as a jump box to access other resources in your VPC.

You can also automate the process of logging into the bastion host by using SSH Agent Forwarding and by adding your ssh key to the bastion host.
This way you dont have to manually enter the ssh key every time you want to connect to the instances in the private subnet via the bastion host.

It is important to remember that you will have to configure your security group rules for the bastion host to only allow traffic from your IP address or from a specific security group. This will add an extra layer of security to your infrastructure.
![Bastion Host](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/bastion%20host.jpg)
![Bastion Host Settings](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/baastion%20host%20confiq.jpg)
![Bastion Host config](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/bastoin%20host%20config%202.jpg)
![Connecting Bastoin Host](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/connecting%20bastoin%20host.jpg)
![launching Bastoin Host](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/launching%20bastoin%20host.jpg)

Now, before you paste it to log in. you need to check if the .pem file you downloaded is in the same directory you want to log in to.
if the file is not there you can use the command below to create a new file. after creating it. then you can open the downloaded file and copy the whole contents in it and paste it inside this file you created on your local machine

NOTE: the name of the file should match the name of the file you created and downloaded.

![SSH Bastoin Host](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/ssh%20basition%20host.jpg)

With the command I copied from AWS, I am now inside my bastion host. from here we can now login into our private servers.

## Installation and Configuration of Nginx Server on Private EC2 Instances Using ansible

For the ansible playbook to work we need four things.

first, we need to update and upgrade our bastion host with the command below.

sudo apt update; sudo apt dist-upgrade -y

second, we would install ansible on the bastion host using the command below.

sudo apt install ansible -y

thirdly, we will need to create three files on our servers. the first is the ansible config file. and it should look like this.

nano ansible.cfg

[defaults]

inventory = inventory private_key_file = ~/filename.pem

The second file we will need to create is our inventory file where we store the IP address of our other two private instances

nano inventory(IP address stored inside the file)

note that the name of the file must be inventory.

and now for the final file. we will need to create is a YAML (Yet Another Markup Language) file where we will be storing our ansible playbook

you can name this file whatever you like. I just like naming mine site.yml but one thing you cannot change is the .yml ending of the file.

now that we have created the file. let’s write our playbook to configure Nginx

---

- hosts: all
  become: yes
  tasks:

  - name: update & upgrade server
    apt:
    update_cache: yes
    upgrade: yes

  - name: install nginx
    apt:
    name: nginx
    state: latest

The task above is to update and upgrade the private servers before installing Nginx.

but that’s not enough we also need a task to change the Nginx default website and write our own so we can distinguish between the two private servers when the load balancer balances the load.

- name: add hostname to server
  tags: print
  shell: echo "< h1>This is my server $(hostname -f)</ h1>" > /var/www/html/index.nginx-debian.html

- name: restart nginx
  tags: restart
  service:
  name: nginx
  state: restarted
  enabled: yes

#### This last service above helps us restart the Nginx server.

correct syntax will be in a .yml File

let's run our playbook!

to run our playbook we will need just this command.

ansible-playbook -i inventory filename.yml

## Creating your Target Groups

Going back to AWS. we will need to create a target group to connect our load balancer and our IP addresses together.

![Target Group](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/target%20group.jpg)

on your EC2 dashboard. Scroll down until you see target groups. click on it and click on create target groups

![TArget Group Config](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/targets%20group%201.jpg)

Select the vpc we created in this blog. leave everything as default.

then at the bottom of this page click on next.

![Include as pending](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/include%20as%20pending.jpg)

Now, select the two private IP addresses.

Click on include as pending below. then create a target group

Currently, there is no load balancer configured for this target group.

Click on the None associated and select the new load balancer.

## Creating An Application Load Balancer

In this part of the blog, we are going to be creating an application load balancer to communicate with our private servers

![CReating Load Balancer](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/creating%20load%20balancer.jpg)

Type in the name of the application load balancer. make sure it is on internet facing.

![adding VPC](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/adding%20vpc%20to%20load%20balancer.jpg)

scrolling down. select the vpc we created and click those two boxes and input your first public IP address and the second public address

![Creating NEw Security Group](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/creating%20new%20security%20group.jpg)

in the security group section, we are going to click on create a new security group.

When creating your inbound rule create it this way.

there should be two ports 80 and two ports 443.

then on the first port 80.

we will be selecting anywhere IPv4.

on the second port 80, we should be selecting anywhere IPv6.

we do the same for port 443 when you are done.

you create the security group.

![secuirty group config](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/inboud%20rule%20for%20security%20rule.jpg)

leave the outbound rule as it is.

just click on Create

now we go back to our load balancer and update the security groups we just created.

if you check and it’s not there just reload it with the reload button at the side. after adding our security groups.

we move down to target groups and select the target group we created previously

Leave everything as a default. and create the load balancer

## Configure Instance Security Group to Face Load Balancer

now going back to our EC2 dashboard.

just above our target groups, we will see security groups.

Click on it.

now navigate to the security group you used for your private instance.

Click on edit inbound rule.
edit the inbound rules by adding a new rule.

and on the port type in port 80.

then in the custom space select the security group you used for your load balancer.

and click create.

you are good to go!
now go back to your target groups. as you can see the two servers are both healthy or by any means if they are not.

you can restart the Nginx server in the two private servers with our ansible task that we wrote before.

we can do this in our bastion host using

ansible-playbook -i inventory FileName.yml --tags restart

now we head back to our load balancer and copy the DNS name. put it on the browser.

![REsult](https://github.com/Felamsy/Altschool-Holiday-challenge-/blob/main/images/lfg.jpg)
