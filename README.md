
# Architecture Design

![Amazon Virtual Private Cloud](/image/VPC%20Architecture%20Design.png)

https://github.com/Gerald-Star/Deploy-Amazon-Virtual-Private-Cloud/assets/62772506/f6533f82-1448-4bc9-ac5b-f6159f809265


[Amazon Virtual Private Cloud](https://explore.skillbuilder.aws/learn/course/499/play/41264/configuring-and-deploying-amazon-vpc-with-multiple-subnets) Cloud (Amazon VPC) allows you to provision a logically isolated section of the AWS cloud. This section enables you to launch AWS resources in a virtual network that you define. With Amazon VPC, you have full control over your virtual networking environment. You can select your IP address range, create subnets, and configure route tables and network gateways. 

## Use Cases

* To launch a simple website or blog
* To host multi-tier web applications.
* To use and create hybrid connections.

## Confi



 Read more blogs about [Amazon Virtual Private Cloud](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html).

You can use both IPv4 and IPv6 in your VPC to ensure secure and easy access to resources and applications.

Created a VPC that consists of three subnets; one public and two private subnets. The web server is hosted in the public subnet to enable it to reach the public internet.

On the other hand, the MySQL RDS instance (database) is hosted on private subnets. To use a DB instance, such as MySQL RDS, in a VPC, the VPC must have at least two subnets located in two different availability zones within the AWS region where you intend to deploy your DB instance. 

As the user in the image, you have tested the application running in the new VPC. 

The diagram provided shows certain general items that you have created. There are other VPC items not displayed, such as route tables and security groups. 


## Task 1. Create an Amazon Virtual Private Cloud (VPC)

* Create a public and private subnets
* Create an Internet gateway
* Create a Route Table and add a route to the Internet
* Create a security group for your web server to only allow HTTP * traffic to your web server
* Create a security group for your MySQL RDS instance to only allow MySQL traffic from your public subnet
* Deploy a web server and a MySQL RDS instance
* Configure your application to connect to your MySQL RDS instance


## Task 2: To create Amazon Virtual Private Cloud (VPC)

* Step 1. On the Amazon Management console service, search VPC and select it
* Step 2. Choose Your VPCs from the left navigational side
* Step 3. Choose Create VPC and then configure
* Step 4. You are on the VPC dashboard; select VPC Only. 
* Step 5. On the Name Tag: Write a name of your choice; My first VPC
* Step 6. Write the IPv4 CIDR block: 10.0.0.0/16

### What is IPv4?

### What is CIDR block?

## Task 3: Create a Public Subnet

*Here, to create a public subnet, launch the web server in the public subnet.*

### What is a subnet?

 A subnet is a range of IP addresses in your VPC. You can launch AWS resources into a specified subnet. 
 
 Use a public subnet for resources that must be connected to the internet, and a private subnet for resources that won’t be connected to the internet.

* Step 1.  Find the subnets in the left navigation pane.
* Step  2.  Choose Create Subnet and configure
* Step 3. Select VPC ID: My first VPC
* Step 4: Subnet name: My Public Subnet 1
* Step 5. On the Availability Zone: Select the first AZ 
*On the IPv4 CIDR block: Enter this address 10.0.1.0/24*

**Click: Create subnet**

*You have created your Public subnet*

* Step 6. Select the checkbox: My Public Subnet 1
*Step 7. Now to edit the subnet settings: In the Actions menu on the top right side, select Edit subnet settings.
*Step 8. Checkbox: Select Enable auto-assign public IPv4 address
**Finally, Click save**

*This process, Enable auto-assign public IPv4 address, provides a public IPv4 address for all instances launched into the selected subnet.*

*For your public subnet to access the public internet so that traffic can access the web server.  It must have an Internet gateway attached to it.*

*The next configuration is to create an internet gateway and attach it to the public subnet.*

## Task 4: Create an Internet Gateway

### What is an Internet Gateway?

An Internet gateway is an essential component of a VPC that enables communication between instances within the VPC and the Internet. 
 
It is highly available, horizontally scaled, and redundant, ensuring that network traffic is free from availability risks and bandwidth constraints. 

The Internet gateway has two primary functions, one is to provide a target to direct Internet-routable traffic to your VPC route tables, and the other is to perform network address translation (NAT) for instances that have been assigned public IPv4 addresses.

* Step 1 Choose Internet gateways on the left navigation page
* Step 2. Click on Create Internet gateway
* Step 3. Enter a name of of your choice on the Name Tag: Example My Internet Gateway
* Step 4. Click on Create Internet Gateway down the last page of the dashboard.
* Step 5. Next, click on the Action Menu at the right side top, and select Attach to VPC to configure
* Step 6. On the Available VPC: select the VPC you created:
* Step 6. Click Attach internet gateway

*This process attaches the Internet gateway to your VPC.* 

*Even though you created an Internet gateway and attached it to your VPC, you still have to tell instances within your public subnet how to get to the Internet.*


## Task 5: Create a Route Table, Add Routes, And Associate Public Subnets

Create a route table for internet-bound traffic
Add a route to the route table to direct the internet-bound traffic to the internet gateway
Associate the public subnet to the route table


A route table is a set of rules that direct network traffic. Each subnet in your VPC must be linked to a route table that controls its routing. 

You can link multiple subnets to the same route table, but each subnet can only be associated with one route table at a time. 
If you want to use an Internet gateway, your subnet's route table must have a route that directs Internet-bound traffic to the gateway. 

This route can be set to all destinations not explicitly defined in the route table (0.0.0.0/0 for IPv4 or ::/0 for IPv6), or you can specify a narrower range of IP addresses such as the public IPv4 addresses of your company's public endpoints outside of AWS or the Elastic IP addresses of other Amazon EC2 instances outside your VPC. When your subnet is linked to a route table that has a route to an Internet gateway, it is called a public subnet.

* Step 1. Click Route tables in the left navigation pane.

Currently, there is only one default route table associated with the VPC, My VPC. This table routes traffic internally. To route public traffic to your Internet Gateway, you need to create an additional Route Table.

* Step 2. Cleck create route table
* Step 3: Configure the following under Route table setting section.

*Give the Route Table a name: Name-optional: My Public Route Table*

*Select the VPC you created: VPC: My first VPC*

* Step 4. Click Create Route table
*On the Route, there is one route in your route table that allows traffic within the 10.0.0.0/16 network to flow, but it does not route traffic outside of the network.* 

**Add a new route to enable public traffic.**

* Step 5. Click Edit routes
* Step 6. Click Add routes to configure
* Destination: 0.0.0.0/0
* Target: Select Internet Gateway in the drop-down and then select the displayed Internet Gateway ID.
* Step 7. Click Save changes
* Step 8. Choose the Subnet association tab
* Step 9. Click Edit subnet associations
* Step 10. Select Public Subnet 1
* Step 11. Click save associations.

**The subnet is now publicly accessible through the Internet Gateway**.

## Task 6: Create a Security Group for your Web Server.

To allow users to access your web server via HTTP, you need to add a security group. A security group is like a virtual firewall that controls incoming and outgoing traffic for your instance. 

When you create an instance in a VPC, you can assign up to five security groups to it. 

Remember that security groups are assigned at the instance level, not the subnet level. This means that each instance in a subnet in your VPC can be assigned to a different set of security groups.

 If you don't specify a particular group during the instance launch, the instance is automatically assigned to the default security group for the VPC.

* Step 1. Choose Security group in the left navigation pane
* Step 2. Click Create Security group to configure
* Step 3. Write a Security group name of your choice: Web Server SG
* Step 4. Description: My web server security group
* Step 5. Select the VPC you created
* Step 6. On the Inbound rules to configure
Click Ass rule
* Type: HTTP
* Source: Anywhere IPv4

**Finally, click Create a Security Group.** 

## Task 7: Launch a Web Server in your Public Subnet.

In this task, you launch a web server that runs an address book application. Later in the lab, you connect the address book application to a Amazon RDS for MySQL instance.

**In the AWS Management Console search field, type 
EC2
Select EC2 from the drop down menu.**

* Step 1. Click Launch instances > Launch instances.

*In the Launch an instance page, configure:*

* Step 2. Name and tags section, type: Web Server
Application and OS Images (Amazon Machine Image) Choose Amazon Linux 2 AMI

*Step 3. Key pair (login) section, choose Proceed without a key pair

* Step 4. Network settings section, choose Edit
VPC, choose My VPC

Note - Public 1 populates under the subnet section

* Step 5. On the Firewall (security groups), choose  Select an existing security group
Common security groups, *
* Step 6. choose Web server
* Step 7. Expand  Advanced Details (at the bottom of the page)
*Copy this script into the User data text box:*

#!/bin/bash -ex
yum -y update
yum -y install httpd php mysql php-mysql
chkconfig httpd on
service httpd start
cd /var/www/html
wget https://us-west-2-aws-training.s3.amazonaws.com/courses/spl-13/v4.2.27.prod-ca751432/scripts/app.tgz
tar xvfz app.tgz
chown apache:root /var/www/html/rds.conf.php

*This scripts installs a web server on your EC2 instance, and runs an app that can be configured to point to your MySQL RDS instance. After you configure your RDS instance, it presents an address book that you can edit.*

## Task 7. Choose Launch instance

* Step 1. Choose View all instances
This brings you to the Instances window where you can see your web server details.

Wait for your web server to fully launch. It should display the following:
Instance State:  running
Status check: 2/2 checks passed.

You can choose the refresh  icon to refresh your instances status.
Your instance should be selected  if not, select it.
Copy the Public IPv4 address address of the instance to your clipboard.
Open a new web browser tab and paste the IP address into the browser.

**Press Enter to go the web page.**
An application should appear:

 Congratulations! You should be able to see this page. Currently, you do not have a database. Once you create your RDS instance, you connect it to your web server.

## Task 8: Create Private Subnets for your MySQL Server

To deploy your RDS database, your VPC must have at least two subnets. These subnets must be in two different Availability Zones in the AWS Region where you want to deploy your DB instance. In this task, you create two private subnets for your Amazon RDS instance.


* Step 1. In the AWS Management Console search field, type 
VPC
*Select VPC from the drop down menu*

* Step 2. In the left navigation pane, choose Subnets.
* Step 3 Choose Create subnet then configure:
VPC: My VPC
*Step 4. Subnet name: Private 1
* Step 5. Availability Zone: Select the first AZ in the list
* Step 6. IPv4 CIDR block: 10.0.2.0/24
**Choose Create subnet**

### CREATE YOUR SECOND PRIVATE SUBNET

* Step 1 Choose Create subnet then configure:
* Step 2. VPC: My VPC
* Step 3. Subnet name: Private 2
* Step 4. Availability Zone: Select the second AZ in the list
IPv4 CIDR block: 10.0.3.0/24

Choose Create subnet
Task 8: Create a Security Group for your Database Server

Now that your private subnets are configured, you secure the types of traffic that can access your MySQL database. In this task, you create a security group to only allow MySQL traffic from your Web server.

In the left navigation pane, choose Security Groups.

Copy the Security group ID.
Description: My Database Security Group

VPC: My VPC
Under Inbound rules
Choose Add rule
Type:  MySQL/Aurora
 Note: Select MySQL, not MSSQL,
Source:
Custom
Paste the web server security group ID that you copied to your text editor.

At the bottom of the screen,choose Create security group.

This allows your web server to communicate with the database.

Task 9: Create a Database Subnet Group.

Amazon RDS instances require a database subnet group. In this task, you create a database subnet group.

 A DB subnet group is a collection of subnets (typically private) that you create in a VPC and that you then designate for your DB instances. 
 
 Each DB subnet group should have subnets in at least two Availability Zones in a given region. When creating a DB instance in a VPC, you must select a DB subnet group.

In the AWS Management Console search field, type RDS:

Select RDS from the drop down menu.

In the left navigation pane, choose Subnet groups.
Choose Create DB Subnet Group then configure:
Name: My Subnet Group
Description: My Subnet Group
VPC: My VPC

### ADD YOUR PRIVATE SUBNETS

In the Add subnets section, configure the following:
Availability zones: Select the first and second Availability Zones in the list.

In the Subnets section, select:
 10.0.2.0/24
 10.0.3.0/24
At the bottom of the screen, choose Create
Task 10: Create an Amazon RDS Database
You are now ready to launch an Amazon RDS database running MySQL.
In the left navigation pane, choose Databases.
Choose Create database then configure:

Engine options: MySQL
Version: MySQL 5.7.X
 It is very important to select latest version of 5.7.X. Select the version with highest number. This lab requires it for the application.

In the Templates section, select Dev/Test.

In the Settings section, configure:

DB instance identifier: myDB

Master username: admin
Master password: lab-password
Confirm password: lab-password
In the DB instance class section, configure:
DB instance class: Burstable classes

Select db.t2.micro or db.t3.micro

In the Storage section, de-select  Enable storage autoscaling.

In the Connectivity section, configure:
Virtual Private Cloud (VPC): My VPC
Public access:  No
Existing VPC security groups:
Add the Database security group

Remove the default security group
In the Monitoring section, de-select  Enable Enhanced monitoring.

In the Additional configuration section (located near the bottom), choose  Additional configuration, then configure:
Initial database name: myDB
De-select  Enable automated backups This turn off backups, which launches the database a little bit quicker for your lab.
De-select  Enable auto minor version upgrade
At the bottom of the screen,choose Create database.

Choose refresh  every 60 seconds until the instance has a status of  available.
 Congratulations! You have deployed a MySQL database.
Task 11: Connect Your Address Book Application to Your Database
In this task, you connect the address book application (in your Public subnet) to your database (in your Private subnet).

### OBTAIN YOUR MYSQL DATABASE ENDPOINT

Before you can connect your address book application to your database, you need to know the endpoint of the RDS instance. This is the address of your RDS instance.

Choose your mydb instance.
In the Connectivity & security section, copy the Endpoint to your clipboard.
You RDS endpoint should look similar to:
mydb.ciljcs3yv1rb.us-west-2.rds.amazonaws.com

### CONNECT TO YOUR DATABASE

Return to the browser tab that is displaying your web server, then configure:
Endpoint: Paste your MySQL endpoint

Database: myDB
Username: admin
Password: lab-password
Choose Submit

Once connected, you should see an address book with two entries.
 Congratulations!

