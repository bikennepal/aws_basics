# aws_basics
Aws basics learning

## Vpc: virutal private cloud: It is the resources that we have in our cloud with public access and private access (ex: ec2 instance as a public and database as a private resource)
let's go through the steps how we can create a custom vpc.

follow the steps below:
1.Go to VPc Section under(network and content delivery)
don't click on (launch vpc wizard) instead click your vpcs on left side and click on create vpc.
There are couple of things you have to add going inside creation dashboard like(name,ipv4 CIDR block and tenancy).
when we create our VPc by default these things are created(Route-table, Nacl and security-group)
when we create sub-net and if you check the ipv4 id it will show only 251 instead of 256 this is because amazon reserved 5 ip address for vpc router network address etc.
after creating sub-net assign one ip as publicly available by selecting the desire one.
There must be an internet-gateway to enter inside Vpc so create a gateway and a vpc can have only one igtw.
after creating subnets launch two ec2 instances one for public and another for private.
Connect two the instance using putty.
by default the private and public instance can't communicate so we need new security group to allow communication.

===========================================================================

Nat instances vs nat gateway.

Nat instances are just a single ec2 instance and nat-gateway are whole instances.

===========================================================================
nacl vs sg

===========================================================================
custome vpc vs elb

To create a load balancer there must be at least two sub-net.

============================================================

VPC flow Logs
vpc flow logs is a feature that enables to capture the information about ip traffic going to and from network interfacein your VPC.
flow logs can be created at three levels:
vpc
subnet and Network Interface Level

=================================================================
Bastions Host.

=================================================================
Direct connect

=================================================================
VPC end points.
VPC endpoints anables you to privately connect your VPC to supported AWS services and VPC endpoint services powered by Privatelink without requiring an internet gateway,NAT device,VPN connection, or AWS Direct conect connedtion.
There are two types of VPC endpoints:
1.Interface endpoints and GateWay endpoints

Interface endpoint: An Interface endpoint is an elastic network interface with a private IP address that serves as an entry point for traffic destined to a supported service. The following services are supported:
Gateway endponts: Amazon s3 and DynamoDB

==================================================================

HA Architecture.
Elastic Load balancer Theory:
There are three types of load balancer:
1.Application Load balancer,Network load balancer and classic load balancer.
Application load balancer are best suited for balancing of HTTP and HTTPS traffic. They operate at Layer 7 and are application aware. They are intelligent, and you can create advanced request routing,sending specified request to specific web servers.
Network Load balancer are best suited for load balancing of TCP traffic where extreme performance is required.
classic load balancer operates on both the request and connection levles it is best for classic EC2 instances.

==============================================================================================================
Load balancer and health checks Lab.
Launch two ec2 instance in different availavility zone with the bootstrap script given below.

#!/bin/bash
yum update -y
yum install httpd -y
service httpd start
chkconfig httpd on
cd /var/www/html
echo "<html><h1>This is Webserver 01</h1></html>" > index.html

To identify change the h1 tag as webserver 02.

After the creation of two ec2 instances go to Load balancer and note that there are three Load balancer: Application load balancer, Network Load balancer and Classic Load balancer.
Let's create a classic Load balancer.Mind that Load balancer dont have Ip address instead it will have DNS.
Create target group according to your customer region Eg:Europe customer etc.know more about Elb from other sources.

======================================================================================================================
Advance Load Balancer Theory
What is Sticky Sessions?

CLB routes each request independently to thr registered EC2 instance with the smallest load.
Sticky Sessions allow you to bind users's sessions to specific EC2 instance this ensure that all request from the user during the season are sent to the same instance.
What is cross zone load balancing?
What are path patterns?
you can create a listener with rules to forward request based on the URL path.
This is known as path-based routing. If you are running microservices, you can route traffic to multiple back-end services using path based-routing.

==========================================================================================================================
HA Wordpress site.
First of all set a S3 bucket and create cloud-front, before RDS go to security-group and check once the inbound and outbound rules after that create RDS and give an IAM role.
Lab 2 process.
While creating an instance run this script.

#!/bin/bash  
yum install httpd php-mysql -y  
amazon-linux-extras install -y php7.3  
cd /var/www/html  
echo "healthy" > healthy.html  
wget https://wordpress.org/latest.tar.gz  
tar -xzf latest.tar.gz  
cp -r wordpress/* /var/www/html/  
rm -rf wordpress  
rm -rf latest.tar.gz  
chmod -R 755 wp-content  
chown -R apache:apache wp-content  
wget https://s3.amazonaws.com/bucketforwordpresslab-donotdelete/htaccess.txt  
mv htaccess.txt .htaccess  
service httpd start  
chkconfig httpd on 

go to putty and connect to the instance and acces the instance on chrome using Ipaddress.
set the wordpress username and password and in the Database host section give the RDS End point.

Getting this error:

Error establishing a database connection
This either means that the username and password information in your wp-config.php file is incorrect or we can’t contact the database server at acloudguru.c4jjcc4u2mmv.ap-south-1.rds.amazonaws.com. This could mean your host’s database server is down.

Are you sure you have the correct username and password?
Are you sure that you have typed the correct hostname?
Are you sure that the database server is running?
If you’re unsure what these terms mean you should probably contact your host. If you still need help you can always visit the WordPress Support Forums.

follow the below given steps.



























