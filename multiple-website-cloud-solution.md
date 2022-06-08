### CREATE VPC
---
- I created a VPC which will house our entire network and named it acme-vpc

- Next I enabled the DNS hostname which is disabled by default.

-- Actions

-- Edit DNS Hostname

-- I checked the enable checkbox and saved the changes  
![Acme VPC](./images/acme-vpc.JPG)  



---
### CREATE SUBNETS FOR THE VPC
---

### The next task is to create the subnets for the VPC

We shall be creating 6 subnets in 2 Availability zones (A and B).(3 in each zone)

The VPC CIDR is 10.0.0.0/16 (65,536 IP V4 addresses)

2 Public subnets 
- public-subnet-1        [In Availability Zone A eu-west-2a]   10.0.1.0/24
- public-subnet-2        [In Availability Zone B eu-west-2b]   10.0.2.0/24

4 private subnets
- private-subnet-1       [In Availability Zone A eu-west-2a]   10.0.3.0/24 
- private-subnet-2       [In Availability Zone B eu-west-2b]   10.0.4.0/24
- private-subnet-3       [In Availability Zone A]   10.0.5.0/24
- private-subnet-4       [In Availability Zone B]   10.0.6.0/24


![Subnets](./images/subnets.JPG)



---
### CREATE ROUTE TABLES FOR THE PRIVATE AND PUBLIC SUBNETS
---

I created 2 route tables. One for the public subnets and the other for the private subnets

- acme-private-rtb
- acme-public-rtb

### ASSOCIATE ROUTE TABLES WITH SUBNETS
---
acme-private-rtb
- private-subnet-1       [In Availability Zone A eu-west-2a]   10.0.3.0/24 
- private-subnet-2       [In Availability Zone B eu-west-2b]   10.0.4.0/24
- private-subnet-3       [In Availability Zone A]   10.0.5.0/24
- private-subnet-4       [In Availability Zone B]   10.0.6.0/24

acme-public-rtb

- public-subnet-1        [In Availability Zone A eu-west-2a]   10.0.1.0/24
- public-subnet-2        [In Availability Zone B eu-west-2b]   10.0.2.0/24

![Subnet Association](./images/subnet-associations.JPG)

Route Table > Subnet Associations > Edit Routes > Save Associations




---
### CREATE INTERNET GATEWAY AND ATTACH TO THE VPC acme-vpc
---
- I created an internet gateway acme-igw and attached it to acme-vpc


![Internet Gateway](./images/igw-vpc.JPG)
---
### CONFIGURE PUBLIC ROUTE TABLE FOR INTERNET ACCESS
---
- For the destination 0.0.0.0/0 (allow from anywhere) with target as the internet gateway acme-igw

![Public route table for inernet access](./images/public-internet.JPG)

---
### CREATE A NAT GATEWAY IN THE PUBLIC SUBNET AND ATTACH AN ELASTIC IP 
---
- I created an elastic IP address named acme-NAT-elastic-ip

- I created the NAT gateway and named it acme-ngw

- Next, I attached the elastic ip to the NAT gateway in public subnet 1.
- A NAT gateway must be in a public subnet so that it can reach the internet.

 For the destination 0.0.0.0/0 (allow from anywhere) with target as the nat gateway acme-ngw


 ---
 ### CREATE THE SECURITY GROUPS
 ---

 1. External Load Balancer   acme-ext-alb-sg
 Allow http/https from anywhere. OPen port 80 and 443 from 0.0.0.0/0

 ![External LB \security group](./images/ext-alb-sg.JPG)



2. Bastion     acme-bastion-sg
Allow SSH from our device IP only

![Bastion SG](./images/bastion-sg.JPG)  

3. Nginx Reverse  proxy server acme-nginx-sg
Allow access from port 80 and 443 from the external load balancer only 
It will only allow access from the external load balancer security group acme-ext-alb-sg  
It will allow SSH access from our Bastion

![Nginx SG](./images/nginx-sg.JPG)  


4. Internal Load Balancer  acme-int-lb
Allow Port 80/443 access from Nginx sg









