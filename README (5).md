# AWS Three-Tier Student Registration Application

A beginner-friendly AWS project that deploys a Student Registration application using a **3-tier architecture**: Proxy/Web tier, Application tier, and Database tier.

## Architecture

- **VPC:** `three-tier-vpc`
- **Subnets:** 1 public subnet + 2 private subnets
- **Internet Gateway (IGW):** Internet access for the public subnet
- **NAT Gateway:** Outbound internet access for resources in private subnets
- **Route Tables:** Public and private routing
- **EC2 instances (3):**
  - Proxy/Web Server (public subnet): Nginx
  - App Server (private subnet): Java + Tomcat 9
  - DB Server (private subnet): MariaDB client/server setup for the lab
- **Amazon RDS:** MariaDB database engine
- **Security Groups:** Rules for HTTP, Nginx, MySQL/MariaDB, and custom TCP (Tomcat)

> **Traffic flow:** Browser → Proxy EC2 (Nginx) → App EC2 (Tomcat) → RDS (MariaDB)

## 1. Create the VPC and networking

1. Create a VPC named `three-tier-vpc`.
2. Create 3 subnets:
   - `public-subnet`
   - `private-sub-1`
   - `private-sub-2`
3. Create and attach an **Internet Gateway** to the VPC.
4. Create a **NAT Gateway** in the public subnet.
5. Create 2 route tables:
   - Public route table: route internet traffic (`0.0.0.0/0`) through the Internet Gateway.
   - Private route table: route internet traffic (`0.0.0.0/0`) through the NAT Gateway.
6. Associate the route tables with the appropriate subnets.

## 2. Launch 3 EC2 instances

Launch three EC2 instances and assign them according to their role:

| Instance | Subnet | Role |
|---|---|---|
| Proxy Server | Public subnet | Nginx reverse proxy |
| App Server | Private subnet | Java + Tomcat 9 + application |
| DB Server | Private subnet | Database/lab server |

Configure Security Groups to allow only the required traffic. For this lab, the rules used include HTTP, MySQL/MariaDB (3306), and custom TCP for Tomcat (typically 8080). Prefer restricting source addresses to the relevant security groups instead of opening ports to everyone.

## 3. Connect to the servers

Connect to the public Proxy Server first. From the Proxy Server, connect to the private App Server and DB Server as required (jump/bastion access).

## 4. Install Nginx on the Proxy Server

Install Nginx on the Proxy Server and configure it as a reverse proxy.

In the Nginx configuration, set `proxy_pass` to the App Server's private IP and Tomcat application port (for example, `http://APP_SERVER_PRIVATE_IP:8080`).

After saving the configuration, restart Nginx and verify the configuration if needed.

## 5. Install Java and Tomcat 9 on the App Server

1. Install Java on the App Server.
2. Download and extract Tomcat 9.
3. Put the application WAR file in Tomcat's `webapps` directory.
4. Make sure Tomcat is configured to listen on the expected port (usually `8080`).

## 6. Download the application and MySQL connector

On the App Server, download the WAR file and connector JAR from the provided repository:

```bash
curl -L -O https://github.com/Sushant123456789/Three-Tier-War-File/raw/refs/heads/main/student.war
sudo wget -O mysql-connector.jar https://github.com/Sushant123456789/Three-Tier-War-File/raw/refs/heads/main/mysql-connector.jar
```

Copy the MySQL Connector JAR into Tomcat's `lib` directory. (Use the actual Tomcat installation path on your instance.)

Copy the WAR file into Tomcat's `webapps` directory. Tomcat deploys/extracts the application when it starts.

## 7. Configure database connection in Tomcat

1. Create the MariaDB database and table for the student application.
2. Note the RDS endpoint, database name, username, and password.
3. Edit Tomcat's `conf/context.xml`.
4. Add/configure the application's JDBC resource with the RDS endpoint, username, password, database name, and JDBC driver settings.
5. Save the file.

**Important:** Do not commit real database passwords or other secrets to GitHub.

## 8. Start Tomcat and restart Nginx

On the App Server, start Tomcat using the appropriate Tomcat `bin` directory command (for example, `./catalina.sh start`).

On the Proxy Server, restart Nginx after the reverse-proxy configuration is saved:

```bash
sudo systemctl restart nginx
```

## 9. Test the application

1. Copy the Proxy Server's public IP address.
2. Open the IP address in a browser.
3. Test the student registration form.
4. Open the students list and verify that the submitted record appears.

Example pages from the lab:
- Student Registration Form
- Students List
- Edit student record
- Delete student record

## Project result

The application was accessed through the Proxy Server's public IP. Nginx forwarded requests to Tomcat on the private App Server, and the application used MariaDB/RDS to store student data.

## Skills practiced

- AWS VPC and subnet design
- Public and private subnet networking
- Internet Gateway and NAT Gateway
- Route tables
- EC2 provisioning and server access
- Security Groups
- Nginx reverse proxy
- Java and Apache Tomcat 9
- MariaDB / Amazon RDS
- JDBC database connection configuration
- Deploying a WAR file

## Cleanup

To avoid unnecessary AWS charges, stop or delete lab resources when finished, especially EC2 instances, NAT Gateway, RDS, and any allocated Elastic IP addresses.
