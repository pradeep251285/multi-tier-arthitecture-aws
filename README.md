# Deploying-multitier-architecture-AWS
 Deploying A Multi-Tier Website Using AWS EC2

# Project Description

#### Project Title: Migration of Company ABC's Product to AWS with High Availability and Auto Scaling

# Objective:
- To migrate Company ABC's existing MySQL database and PHP website to AWS, ensuring high availability and scalability by leveraging Amazon EC2, Auto Scaling, and Amazon RDS services.

#### Components:
- **MySQL Database:** To be migrated to Amazon RDS for managed database services.
- **PHP Website:** To be hosted on Amazon EC2 instances with Auto Scaling for high availability.

#### Goals:
- Ensure high availability of the website by enabling Auto Scaling
- Migrate the MySQL database to Amazon RDS.
- Configure necessary security groups and network settings for seamless communication between EC2 instances and the RDS instance.

# Key Features of the Project

#### Scalable Computing Capacity:
- Utilizes Amazon EC2 instances to provide scalable computing capacity, allowing for dynamic adjustment based on traffic and resource requirements.

#### High Availability:
- Implements Auto Scaling to ensure the website remains highly available by maintaining a minimum of two instances at all times. This helps in handling traffic spikes and ensures reliability.

#### Managed Database Service:
- Migrates the MySQL database to Amazon RDS, offering automated backups, patching, and scalability, reducing the administrative burden on managing the database.

#### Automated Scaling:
- Configures Auto Scaling policies to automatically adjust the number of EC2 instances in response to traffic patterns, ensuring optimal performance and cost-efficiency.

#### Secure Networking:
- Sets up appropriate security groups and network configurations to allow secure communication between EC2 instances and the RDS instance, and to manage inbound and outbound traffic effectively.

#### Optimized Storage:
- Utilizes Amazon EBS (Elastic Block Store) for reliable and high-performance storage attached to EC2 instances.

#### Configuration Management:
- Provides a streamlined process to configure and modify the PHP website to use the new RDS database endpoint, ensuring seamless integration.

#### Step1.VPC SETUP
 **Creating VPC is better to secure our architecture safe**
- Create vpc and provide CIDR range ` 10.0.0.0/24 `
- Create subnets one for public and another for private
- Create internet gateway and attach
- Create route table then edit routes add IGW to the route table
- For public subnets click on subnet associations and add public subnet
- For private subnet click on subnet associations and add private subnet

#### Step2.Launch an EC2 Instance
- Select an Amazon Machine Image (AMI): Choose Amazon Linux 2 AMI.
- Select Instance Type: Choose ` t2.micro `
- Configure Instance: Ensure selection of the correct VPC.
- Add Storage: Default 8 GB is sufficient.
- Configure Security Group: Add rules for HTTP (port 80) and SSH (port 22).


**Connect to the ec2 instance previously created and update the machine and install apache2 commands given below**
```
sudo yum update
sudo yum install https -y
sudo systemctl start httpd service

```

- Check the HTTPD installed or not by copying the public IP of the instance & paste on browser
- We need to mve the ` index.php ` and the images that we have in the local machine to ec2 terminal we can use gitbash or command prompt for this by moving to downloads directory in our local machine through the command ` cd downloads ` ` scp -i -r "keypairname.pem" "file-name" ec2-user@(IP of the instance ` by running these command we can able to copy files in the terminal
- Then we need to replace the ` index.html ` to ` index.php
- We can use this command to move index.php to html path
```
sudo mv index.php images /var/www/html
```

#### Step.3 Create an RDS Instance
- Create Database:
- Choose MySQL.
- Specify DB details:
  - DB instance identifier: ` intel-db `
  - Master username: ` admin. `
  - Master password: ` intel123. `
- Instance specifications: ` Choose db.t2.micro `
- Storage: Allocate storage as per requirement (default 20 GB).
- Connectivity: Ensure the database is in the same VPC as the EC2 instances.
- Create DB instance

**Installing softwares on EC2**
- We need to install some softwares like ` MYSQL ` and ` PHP ` commands given below
```
sudo apt-add-repository-y ppa:ondrej/php
sudo apt install php5.6 mysql-client php5.6-mysqli
```
- In the instance move to ` cd /var/www/html ` and edit file as per our requirements by using command
```
sudo nano index.php
```

#### step.4  Create Database and Table in RDS Instance
- Connect to the RDS instance using the endpoint provided.
- We can connect database in the instance with the command
```
mysql -h (end-point of RDS) -u (USERNAME) -p (PASSWORD)
```
- After connecting the database enter ` show databases ` we can see databases that we are available
- Create Database and Table
```
CREATE DATABASE intel;
USE intel;
CREATE TABLE data (
    id INT AUTO_INCREMENT PRIMARY KEY,
    value VARCHAR(25) NOT NULL
);

```
#### Step.5  Change Hostname in Website
- SSH into the EC2 instance.
- **Modify the PHP website configuration**  to connect to the RDS instance:
- Change the database host in the configuration file to the RDS endpoint.

#### Step.6  Allow Traffic from EC2 to RDS Instance
- **Modify RDS Security Group**
- Add an inbound rule to allow MySQL/Aurora (port 3306) from the EC2 instances' security group.
- Allow All-Traffic to EC2 Instance
- Modify EC2 Security Group:
- Add inbound rules to allow all necessary traffic.

#### Step.7 Enable Auto Scaling
- Navigate to EC2 Dashboard.
- Auto Scaling Groups
- Create an Auto Scaling Group.
- Attach the previously created EC2 instance template.
- Configure group size: Minimum 2 instances.
- Set scaling policies to manage the desired capacity automatically.

#### Step.8 Create loadbalancer
- By using previously created autoscaling group we can attach a new load balacer to that ASG
- The load balancer helps us to distribute loads based on situation
- After creating loadbalancer we should copy DNS of the load balancer 
- We can get ouyput as the below image shown




  


