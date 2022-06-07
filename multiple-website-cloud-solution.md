### CREATE VPC
---
- I created a VPC which will house our entire network and named it acme-vpc

- Next I enabled the DNS hostname which is disabled by default.

-- Actions

-- Edit DNS Hostname

-- I checked the enable checkbox and saved the changes  
![Acme VPC](./images/acme-vpc.JPG)  

---
### CREATE INTERNET GATEWAY
---
- The next step is to create an Internet Gateway and attach it to the VPC.

- I created an internet gateway and named it acme-igw