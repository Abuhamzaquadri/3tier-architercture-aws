# AWS 3-Tier Secure Architecture  

##  Introduction  
Building a **3-Tier Infrastructure** to launch an application on AWS Cloud and making it available to serve the end users.  

---

## Different Types of Architectures  

###  One-Tier Architecture  
- Server and Client layers are on the same machine.  
- Not used in production or development environments.  

###  Two-Tier Architecture  
- Client Layer and Server Layer are different machines.  
- Client Layer accesses the Server Layer for the application.  
- Server Layer acts as both **Database + Application Server**.  
- Not used in production environments, but can be used in **development and testing**.  

**Diagram:**  
![2-Tier Architecture](images/2%20tier%20architecture.png)
 

---

###  Three-Tier Architecture  
- Server Layer is further separated into:  
  - **Application Layer**  
  - **Database Layer**  
- Both are independent entities, which improves performance.  
- Can be used in production but is **less secure**.  

**Diagram1:**  
![3-Tier Architecture](images/3%20tier%20architecture.png)




**Diagram2:**  

## Nth-Tier / 3-Tier Secure Architecture

![Nth-Tier Architecture](images/Nth-tier%20architecture.png)

---

###  N-Tier / Secure 3-Tier Architecture  
- An extra **Proxy Server Layer** is added between Client and Application Server.  
- Provides security to both **Application Layer** and **Database Layer**.  
- This is the actual **Production Environment Architecture**.  
- Ensures:  
  - Better performance  
  - Security from external threats  

---

## ⚙ Technical Planning for 3-Tier Secure Architecture in AWS Cloud  

###  Step 1: Create VPC  
- Create a **VPC** in AWS Cloud with the network range `10.0.0.0/16`  
  - Range: `10.0.0.0` to `10.0.255.255`
 


# VPC & Subnet Diagrams

## VPC for Subnets

![VPC for Subnets](images/VPC%20and%20Subnet/vpc-for-subnets.png)

## Subnet Creation

![Subnet Creation](images/VPC%20and%20Subnet/subnet-create.png)






###  Step 2: Create Subnets  
- Create **9 Subnets** in the VPC


# Subnet Diagrams

## Subnet 1
![Subnet 1](images/VPC%20and%20Subnet/subnet-1.png)

## Subnet 2
![Subnet 2](images/VPC%20and%20Subnet/subnet-2.png)

## Subnet 3
![Subnet 3](images/VPC%20and%20Subnet/subnet-3.png)

## Subnet 4
![Subnet 4](images/VPC%20and%20Subnet/subnet-4.png)

## Subnet 5
![Subnet 5](images/VPC%20and%20Subnet/subnet-5.png)

## Subnet 6
![Subnet 6](images/VPC%20and%20Subnet/subnet-6.png)

## Subnet 7
![Subnet 7](images/VPC%20and%20Subnet/subnet-7.png)

## Subnet 8
![Subnet 8](images/VPC%20and%20Subnet/subnet-8.png)

## Subnet 9
![Subnet 9](images/VPC%20and%20Subnet/subnet-9.png)




**Subnet Allocation:**  

| Network ID | IP Address Range | Broadcast ID |
|------------|------------------|--------------|
| 10.0.0.0   | 10.0.0.1 – 10.0.15.254   | 10.0.15.255   |
| 10.0.16.0  | 10.0.16.1 – 10.0.31.254  | 10.0.31.255   |
| 10.0.32.0  | 10.0.32.1 – 10.0.47.254  | 10.0.47.255   |
| 10.0.48.0  | 10.0.48.1 – 10.0.63.254  | 10.0.63.255   |
| 10.0.64.0  | 10.0.64.1 – 10.0.79.254  | 10.0.79.255   |
| 10.0.80.0  | 10.0.80.1 – 10.0.95.254  | 10.0.95.255   |
| 10.0.96.0  | 10.0.96.1 – 10.0.111.254 | 10.0.111.255  |
| 10.0.112.0 | 10.0.112.1 – 10.0.127.254| 10.0.127.255  |
| 10.0.128.0 | 10.0.128.1 – 10.0.143.254| 10.0.143.255  |

- **First 3 Subnets → Public Subnets** (Proxy Server Layer)  
- **Next 3 Subnets → Private Subnets** (Application Server Layer)  
- **Last 3 Subnets → Private Subnets** (Database Server Layer)  

---

###  Step 3: Internet Gateway  
- Create **Internet Gateway**  
- Attach it to the VPC




###  Step 4: Public Route Table  
- Create **Public Route Table**  
- Route it to Internet Gateway  
- Associate with **3 Public Subnets**  

###  Step 5: NAT Gateway  
- Create a **NAT Gateway**  
- Associate it with **Elastic IP**  

###  Step 6: Private Route Tables  
- Create **Private Route Table** for Application Layer  
  - Route it to NAT Gateway  
  - Associate with **3 Application Subnets**  

- Create **Private Route Table** for Database Layer  
  - Route it to NAT Gateway  
  - Associate with **3 Database Subnets**

###  Step 7
- EC2 Instances (Compute Layer)
Proxy Layer: EC2 instances or Load Balancer in public subnets to handle incoming traffic
Application Layer: EC2 instances hosting applications in private subnets
Database Layer: EC2 DB (if not using RDS) in private subnets

###  Step 8
- Database Layer
  RDS / EC2 DB → Hosted in private subnets, accessible only by Application Layer

###  Step 9
- Load Balancer
  ALB / ELB → Placed in public subnets to distribute traffic to Application Layer EC2 instances
  Provides fault tolerance and high availability

###  Step 10
= Security Groups & Network ACLs
  Proxy SG: Allow 80/443 from Internet
  App SG: Allow 80/443 from Proxy SG only
  DB SG: Allow 3306 from App SG only
  NACLs: Subnet level filtering for extra security

Flow
Client → Public Internet → Load Balancer / Proxy Layer
Load Balancer → Application Servers (EC2, Private Subnet)
Application Servers → Database Servers (RDS/EC2, Private Subnet)
Database Servers → No direct Internet access

---

##  Conclusion  
- **2-Tier Architecture** → Suitable for Dev/Test only.  
- **3-Tier Architecture** → Can be used in production but less secure.  
- **Secure 3-Tier Architecture** → Best for Production with **Proxy + Application + Database layers** and highest security.  


**Calculation:**  
