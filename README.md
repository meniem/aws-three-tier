# Cloudformation template to provision three-tier App

The CloudFormation template will create TomCat Web Server, Jenkins Server, and 2 MySQL multi-master replication:

 - Download, install and configure Tomcat on Ubuntu instance.
 - Download, install Jenkins server.
 - Download MySQL on 2 ubuntu instances and create multi-master replication between these 2 instances


# The template accepts the below parameters from the user:

 - Instance Type (only t2.micro and t2.nano, and can be changed in the parameters section or using cfn mappings).
 - SSH Key (select one from existence keys).
 - Jenkins port number.
 - Root volume size.
 - Default availability zone.
 - The Class B of the VPC (10.xxx.0.0/16), so if you enter 10, it will be 10.10.0.0/16


![N|Solid](https://raw.githubusercontent.com/meniem/aws-three-tier/master/parameters.jpg)


# The CloudFormation will provision:
 - VPC.
 - 2 public subnets
 - 2 private subnets
 - Internet gateway for the public subnet
 - Route tables for all subnets
 - Route tables associations
 - NACL for all subnets
 - Security group to allow internet traffic from port 443 to the public subnet
 - Security group to allow internal traffci from port 3360 t othe private subnet
 - ##### 4 instances as shown below

| Instance | Subnet | Details |
| ------ | ------ |------ |
| Jenkins Instance | Public Subnet A | Install Jenkins server |
| Tomcat server | Private Subnet A | Install and configure Tomcat server |
| MySQL master 1 | Private Subnet A | MySQL multi-master replication |
| MySQL master 2 | Private Subnet B | MySQL multi-master replication |


# The below are the IP ranges used for the subnets created:

| Public Subnet A | Private Subnet A | Private Subnet A |
| ------ | ------ |------ |
| 10.xxx.0.0/20 | 10.xxx.16.0/20 | 10.xxx.48.0/20 |


# NACLs and SGs are configured to allow all outbound, and inbound as below:

|  | NACL Public A | NACL Private A | NACL Private B |
| ------ | ------ |------ | ------ |
| Port | 443 | 443 | 3306 |
| CIDR | 0.0.0.0/0 | 10.xxx.0.0/20 | 3306 | 10.xxx.16.0/20