# Tieto SOC Infrastructure Q&A - Ly Duong

### 1. Cloud experience
---

I have practiced and used both AWS and Azure for my studies and projects. Therefore, I am familiar with deploying and managing services on both platforms. Services that I have used on AWS and Azure:

**Platform**|**Services**
:-------|:-------
**AWS**     |EC2 <br> S3 <br> IAM <br> DynamoDB <br> Lambda <br> Cloud Formation <br> SQS <br> CloudFront
**Azure**   |Virtual machine <br> DevOps <br> Active Directory <br> Content Delivery Network <br> App Service <br> SQL Server <br> ACI 

<br>
<br>

### 2. Virtualization experience
---

##### a. VMware/VirtualBox experience

I have used both VMware and VirtualBox for courses before. Some of the tasks I have performed with VMware are:

**Task**|**Sub-tasks**
--------|-------------
**Creating VMs**|Customizing hardware <br> Installing OS, files, and VMware tools
**Modifying VMs**|Renaming, changing **VMDK** file size <br> Adjusting memory allocation <br> Adding/removing raw **LUNs**
**Migrating VMs**|Migrating VM files from Local to Shared Storage <br> Creating a **Virtual Switch** and a **VMkernel Port Group** for vSphere vMotion Migration <br> Performing a **vSphere vMotion Migration** of a VM on Shared Datastore <br> Perform a **Compute Resource and Storage Migration**
**Managing VMs**|Registering/Unregistering a VM from the **vCenter Server Appliance Inventory** <br> Unregister and Delete a VM from the **Datastore**
**Managing Resource Pools**|Creating **CPU Contention** <br> Create **Resource Pools** <br> Verifying Resource Pool Functionality
**Monitoring VM Performance**|Using **User Performance Charts** to Monitor CPU Utilization
**Using Alarms**|Creating a VM Alarm to Monitor a Condition <br> Create a VM Alarm to Monitor an Event
**Using vSphere HA**|Creating a **Cluster Enabled** for vSphere HA <br> Adding **ESXi Host** to the cluster
 
 
 ##### b. Idea, purpose, and benefit of hyper converged platform
 
 Before virtualization, it was hard and expensive to increase capacity of hardware such as UPS, AC units, DC racks to keep up with the need while server utilization is still low. Furthermore, updating application and keeping HA and load balancing was extremely difficult as hardware administration and troubleshooting was challenging.
 <br>
 Hyper converged platform enable multiple independent operating systems to run on the same server by separating physical hardware and software by emulating hardware using software. The hypervisor manages the interactions between each virtual machine and the hardware that the guest operating systems all share.
 <br>
 Benefits of hyper converged platform:
  - Greater datacenter and administrative efficiency
  - Increasing Business Agility
  - Reducing maintenance time
  - High availability
  - Ability to move VMs from one server to another 
  - Automatic load balancing
  - Improving Data Protection and Disaster Recovery
  - Lowering Datacenter and Administrative Costs
  - Reducing Energy Consumption
 
 
<br>
<br>

### 3. Linux/Windows server
---
I have some experience with both Linux and Windows server, and can perform basic operations on both platforms.
Specific tasks I have practiced doing on a Linux server includes:
 - **User and Group Management** (creation, modifying password and configuration files)
 - **Configuring Permission**s (file ownership, ACL with setfacl and getfacl)
 - **Configuring a basic Apache Server** (creating web server content and virtual hosts, modifying configuration files and **TLS** security configuration)
 - Managing **SELinux** (diagnosing and addressing SELinux Policy Violations)
 - Managing **iptables** services and **firewalld** services, configuring **NAT**
 - Intalling, configuring and securing **FTP** with **NFS** security, **Kerberos** requirements, authentication **SMB**, samba shares permissions
 - Installing and setting up **FreeIPA** server for user authentication with **Kerberized NFS**
 - Configuring **LDAP** Authentication with **Kerberos** Authorization
 - Setting up and troubleshooting a **cache-only DNS server**
 - Managing **Samba** file services
 - Configuring **ssh** settings

<br>
<br>

### 4. WordPress 3-tier application
---
My cloud platform of choice for the WordPress application is AWS. 
The architechture for the application for each of the two cases will be detailed below.

##### a. Classroom application

**Function/Tier**|**Components**
-----------------|--------------
**In Tier 1: Public subnets**| **A load balancer and a bastion host**, the bastion host is used to reach the private instances without giving access directly outside the VPC.
**In Tier 2: WordPress servers**| **2 WordPress Intances**.
**Tier 3: Database tier**|**2 RDS for MySQL** for database administrator. This dedicated MySQL virtual server has database connections to the WordPress Instances in Tier 2. 
**Network**|**VPC and Internet Gateway** to allow communication between VPC and the internet, <br> NAT gateways to connect 2 public subnets with the 2 Wordpress Instances in private subnets to allow them to access the internet.
**Load balancing**| An **Application Load Balancer** will spread the user load across EC2 instances in **2 Availability Zones**.
**Back-up**| **Snapshot** mechanism is implemented for the back-up website data. 

![alt text](https://github.com/khanhly2901/Tieto-SOC-Infra-Q-A/raw/master/WP%20Architecture%20Diagrams/class-fix.png "Classroom WordPress Architecture")*Fig 1. Classroom-tier WordPress application architecture*
##### b. University application
For a large scale deployment of the app, the considerations and solutions to the application architecture are as follow:

**Consideration**|**Solution**
-----------------|------------
**Network latency** with remote access|Deploying **CloudFront** to distribute content with low latency and high data transfer speed
**Personalized content** management|Configuring CloudFront to maximize the caching efficency
**Increased workload**|Employing **Auto Scaling** to automately scale Amazon EC2s
Durability, scalability and reliability| **S3** is deployed for static content offload
Speed up the installation and recovery of WordPress instances|Deploying **AWS EFS** provides scalable network file systems for storing the WordPress content
WordPress **limited caching** capabilities|Deploying **ElasticCache** to reduce latency and increase throughput for readheavy application workloads
Increasing **data tier performance**|Replace RDS with **Aurora** for its fault-tolerant and self-healing abilities

![alt text](https://github.com/khanhly2901/Tieto-SOC-Infra-Q-A/raw/master/WP%20Architecture%20Diagrams/uni-fix.png "University WordPress Architecture")*Fig 2. University-tier WordPress application architecture*

<br>
<br>

### 5. Approach for network troubleshooting application
---

I often find Cisco's Troubleshooting Model usefull. The model is as followed:
1.  **Define the problem.** Verify problem to solve and create Problem Report
2.  **Gather information.** Identify targets: where, who, when, frequency, what (router, switch); identify tools (debug commnand, show commnand, packet capture); gain access to get information
3.  **Analyze information.** Check documentation (As-built Document), check baseline, research
4.  **Eliminate potential causes.** Question assumptions, list possible causes
5.  **Propose a hypothesis.** Identify most likely cause
6.  **Test hypothesis.** Action to prove of disprove hypothesis
7.  **Solve and Document**. Details the issue's symptoms, cause and resolution

Some more specific cases, approach and possible course of action when a client cannot access the web application:

**Approach**|**Resolution Example**
------------|----------------------
**Top-down approach:** Start at the OSI application layer and go down|Check if problem is widespead among users. Check if the server can be access through **telnet port 80**. If the telnet is successfull, the problem is at the upper layers, else check the lower layers.
**Bottom-up approach:** Starting at the physical layer and go down|Check the cables and power of routers and switches. If everything is inplace, check if switches have the correct MAC address. Then, check if the router have routes to get to destination and so on.
**Divide and conquer:** Seperate the upper layers and lower layers|**Ping** the web app server. If successful, focus on the upper layer, else check the lower level.
**Follow the path**|Use *'traceroute'* command to check the paths of packets from a source to a destination.
**Spot the different**|Compare current configuration with a good configuration.
**Swapping component**|Swap network components to see if the problem stays when the component is changed.


<br>
<br>

### 6. CI/CD experience
---
##### a. Tools I am familiar with
Listed below are CI/CD tools which I had the chance to practice with:
 - Git
 - Maven
 - Docker
 - Jenkins
 - Ansible
 - Kubernetes
 - Tomcat


##### b. Pipeline for web application
Below is my pipeline for a Java web application:

**Role**|**Tool**
--------|--------
Source code management|Git <br> GitLab
Build|Maven
Repository management|Docker Hub
Continuous integration|Jenkins <br> GitLabCI
Testing|JUnit <br> Selenium
Provisioning and configuration management|Ansible
Deployement automation|Terraform <br> AWS CloudFormation
Container environment|Docker <br> Kubernetes
Cloud environment|AWS

<br>
<br>

### 7. Container experience
---
##### a. Tasks I have done with Docker

 - Installation on Linux/Windows
 - Writing **Dockerfile**
 - Buidling **Docker images**
 - Using **Docker Hub** and **Docker Registry** to store Docker images
 - Launching, managing containers
 - Using *'attach'*, *'exec'* commands to interact with containers
 - Setting resource constraints
 - Configuring private container networking, exposing ports
 - Connecting containers
 - Using **Docker Compose** to configure a multi-container application


##### b. VMs vs containers

**Characteristic**|**Virtual machines**|**Containers**
------------------|--------------------|--------------
**Operating System**|Heavyweight- complete OS: Apps, Services, Kernel|Lightweight - only apps, OS API and services
**Hardware Requirement**|Need more memory and storage space|Need less memory and storage space
**Performance**|Limited performance|Native performance
**Virtualization**|Hardware-level virtualization - share hardware|OS virtualization - sharer OS capacity
**Isolation/Security**|Complete isolation, strong security boundary|Process-level isolation, possibly less secure
**Start-up time**|Minutes|Milliseconds




