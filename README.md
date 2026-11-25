# Three-Tier Student Registration Project

## 1. Project Overview

I deployed a Student Registration Web Application using AWS Three-Tier Architecture.

This project uses three layers:

- **Web Tier:** Nginx Reverse Proxy
- **Application Tier:** Java + Apache Tomcat 9
- **Database Tier:** MariaDB / Amazon RDS

Users can register students, view student records, edit records, and delete records.

---

## 2. Architecture

### Architecture Flow

<img width="1536" height="1024" alt="AWS Three-Tier Student Registration Architecture" src="https://github.com/user-attachments/assets/9ad4c0f1-e76d-4227-bd8d-297fa971c071" />


### AWS Resources Used

- VPC
- 3 Subnets (1 Public + 2 Private)
- Internet Gateway
- NAT Gateway
- 2 Route Tables
- 3 EC2 Instances
- Security Groups
- Nginx
- Java
- Apache Tomcat 9
- MariaDB
- Amazon RDS
- MySQL Connector

---

## 3. VPC Creation

Created a VPC named:

`three-tier-vpc`

Configured the VPC for the Three-Tier architecture.

### VPC Configuration

- Created a custom VPC
- Created 3 Subnets
- Configured Public and Private Subnets
- Configured network routing

### VPC Screenshot

<!-- Add VPC screenshot here -->

![VPC]<img width="1919" height="1079" alt="Screenshot 2026-11-24 211551" src="https://github.com/user-attachments/assets/ff498592-a5a9-47a1-a405-2a514ee18e6a" />


---

## 4. Subnet Creation

Created three subnets inside the VPC.

| Subnet | Type | Availability Zone |
|---|---|---|
| public-subnet | Public | us-east-1a |
| private-sub-1 | Private | us-east-1b |
| private-sub-2 | Private | us-east-1c |

## 5. Internet Gateway

Created an Internet Gateway and attached it to the VPC.

The Internet Gateway provides internet connectivity to resources in the Public Subnet when routing and public addressing are configured correctly.

## 6. NAT Gateway

Created a NAT Gateway inside the Public Subnet.

The NAT Gateway allows instances in Private Subnets to access the internet for outbound connections without requiring them to have public IP addresses.

## 7. Route Tables

Created two Route Tables.

### Public Route Table

- Associated with the Public Subnet
- Configured `0.0.0.0/0` route to Internet Gateway

### Private Route Table

- Associated with Private Subnets
- Configured `0.0.0.0/0` route to NAT Gateway

## 8. EC2 Instances

Launched three EC2 instances inside the VPC.

| Instance | Subnet | Purpose |
|---|---|---|
| Proxy Server | Public Subnet | Nginx Reverse Proxy |
| App Server | Private Subnet | Java + Tomcat |
| DB Server | Private Subnet | Database setup |

## 9. Security Groups

Configured Security Groups for communication between the servers.

### Ports Used

| Port | Service | Purpose |
|---|---|---|
| 22 | SSH | Server access |
| 80 | HTTP | Web traffic |
| 3306 | MySQL / MariaDB | Database connectivity |
| 8080 | Custom TCP | Tomcat application traffic |

Security Group access was configured according to the server requirements.

For better security, restrict SSH and application/database access to trusted sources or the appropriate Security Groups.

## 10. Connect to EC2 Instances

Connected to the Proxy Server using SSH.

From the Proxy Server, connected to the private App Server and DB Server using jump/bastion access as required.

## 11. Proxy Server Setup (Nginx)

Connected to the Proxy Server.

Installed Nginx and configured it as a Reverse Proxy.

Nginx receives requests from the browser and forwards them to the Application Server.

### Nginx Installation

Installed Nginx on the Proxy Server.

### Nginx Configuration

Configured `proxy_pass` to forward requests to the private IP address of the App Server and its Tomcat port.

Example:

```nginx
location / {
    proxy_pass http://APP_SERVER_PRIVATE_IP:8080;
}
```

> Replace `APP_SERVER_PRIVATE_IP` with the actual private IP of the App Server. Use the configuration appropriate for your deployed application.

## 12. App Server Setup

Connected to the App Server through the Proxy Server.

Installed Java and configured Apache Tomcat 9.

### Installed Components

- Java
- Apache Tomcat 9

## 13. Download Student WAR File

Downloaded the Student Registration application WAR file from GitHub.

### Download Command

```bash
curl -L -O https://github.com/Sushant123456789/Three-Tier-War-File/raw/refs/heads/main/student.war
```

The `student.war` file contains the web application.

Copied the WAR file into Tomcat's `webapps` directory.

Tomcat deploys the application when it starts.

## 14. Download MySQL Connector

Downloaded MySQL Connector JAR from GitHub.

### Download Command

```bash
sudo wget -O mysql-connector.jar https://github.com/Sushant123456789/Three-Tier-War-File/raw/refs/heads/main/mysql-connector.jar
```

Copied the MySQL Connector JAR file into Tomcat's `lib` directory.

The JDBC driver allows the Java application to connect to the MariaDB/MySQL databases

## 15. Tomcat Configuration

Configured Tomcat to connect to the database.

Edited the Tomcat `conf/context.xml` file.

Configured the JDBC Resource with:

- Database Name
- Database Endpoint
- Database Username
- Database Password
- JDBC Driver

### context.xml

Configured the database connection details in `context.xml` according to the application requirements.

## 16. Amazon RDS Setup

Created an Amazon RDS database using the MariaDB engine.

Configured:

- MariaDB Engine
- Database Instance
- Database Username
- Database Password
- Endpoint
- Database Connectivity

Configured the database connection between the Application Server and the database.

## 17. MariaDB Setup

Installed MariaDB on the Database Server for the lab setup.

Connected to MariaDB and configured the `studentapp` database.

### Database Commands

```sql
SHOW DATABASES;

USE studentapp;

SHOW TABLES;

DESC students;
```

### Students Table

The `students` table contains:

| Column | Description |
|---|---|
| student_id | Student ID |
| student_name | Student Name |
| student_addr | Student Address |
| student_age | Student Age |
| student_qual | Student Qualification |
| student_percent | Student Percentage |
| student_year_passed | Student Year Passed |

### Database Screenshot

<!-- Add Database screenshot here -->

![Database]<img width="1908" height="1071" alt="Screenshot 2026-11-24 205341" src="https://github.com/user-attachments/assets/e7e21579-6d9e-4c5e-aa4e-7f8aed57f16b" />


---

## 18. Start Tomcat

Started Tomcat from the `bin` directory.

### Command

```bash
./catalina.sh start
```

Tomcat starts the Java web application.

### Tomcat Screenshot

<!-- Add Tomcat start screenshot here -->

![Tomcat]<img width="1915" height="1070" alt="Screenshot 2026-11-24 201837" src="https://github.com/user-attachments/assets/264d843a-1fda-4194-84fb-5d21a0319229" />


---

## 19. Restart Nginx

Restarted Nginx on the Proxy Server after configuration.

### Command

```bash
sudo systemctl restart nginx
```

Nginx forwards the incoming browser requests to the App Server.

## 20. Application Testing

Copied the Public IP address of the Proxy Server and opened it in the browser.

Successfully accessed the Student Registration Application.

### Application Features

- Student Registration
- Students List
- Edit Student Details
- Delete Student Details

### Student Registration Form

<!-- Add Registration Form screenshot here -->

![Registration Form]<img width="1896" height="1057" alt="Screenshot 2026-11-24 202838" src="https://github.com/user-attachments/assets/824e326c-6dbb-42ff-9e2d-66d4cdbdaa10" />


### Students List

<!-- Add Students List screenshot here -->

![Students List]<img width="1919" height="1069" alt="Screenshot 2026-11-24 210630" src="https://github.com/user-attachments/assets/a03012eb-f54e-4263-9b52-38aa4f8aff09" />


---

## 21. Project Result

Successfully deployed a Student Registration Application using AWS Three-Tier Architecture.

The Nginx Reverse Proxy forwards the requests to the private Application Server running Java and Tomcat 9.

The Java application connects to the configured MariaDB database to store and retrieve student records.

The application was accessed using the Public IP address of the Proxy Server.

<img width="1919" height="1069" alt="Screenshot 2026-11-24 210630" src="https://github.com/user-attachments/assets/c6536bd7-4b87-43e0-a8dd-0ab3fcdbae3d" />

<img width="1901" height="239" alt="Screenshot 2026-11-24 211059" src="https://github.com/user-attachments/assets/11b93a0b-36a9-4f4d-aec8-469f1d712593" />




## 22. Technologies Used

- Amazon Web Services (AWS)
- VPC
- EC2
- Subnets
- Internet Gateway
- NAT Gateway
- Route Tables
- Security Groups
- Nginx
- Java
- Apache Tomcat 9
- MariaDB
- Amazon RDS
- Linux
- JDBC
- GitHub

---

## 23. Skills Learned

- AWS Networking
- VPC Configuration
- Public and Private Subnets
- EC2 Management
- Linux Commands
- SSH and Jump Server Connectivity
- Nginx Reverse Proxy
- Java Application Deployment
- Apache Tomcat Configuration
- MariaDB Database
- Amazon RDS
- JDBC Connectivity
- Three-Tier Architecture
- AWS Infrastructure Setup

---

## 24. Cleanup

To avoid unnecessary AWS charges, delete or stop the resources when the project is no longer needed.

Review these resources:

- EC2 Instances
- NAT Gateway
- Amazon RDS
- Elastic IP Addresses
- Other AWS resources created for this project

---

## Author

AWS Cloud & DevOps Learner
Harshavardhan Chavhan
