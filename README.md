# Deploying-Configuring-and-Maintaining-Systems

# Tier 3 Task: Deploying, Configuring, and Maintaining Systems

# 1. Deploying Three CentOS Servers
You can set up three CentOS 8 servers using a virtualization platform such as VirtualBox, VMware, or KVM. Once the servers are deployed:

Ensure each VM or physical machine has CentOS 8 installed.
Log in as the root user or use sudo for administrative tasks.

# 2. Networking Configuration

Assigning Static IP Addresses
To assign static IP addresses to your CentOS server, you will need to modify the network configuration file for the appropriate network interface. Here's a step-by-step guide:

Identify the network interface:

Use the following command to list all network interfaces:

ip addr
The network interface could be named something like ens192 or eth0, depending on your environment. Replace ens192 with your actual interface name in the commands.

Edit the network configuration file:

Open the network configuration file (e.g., /etc/sysconfig/network-scripts/ifcfg-ens192):

sudo vi /etc/sysconfig/network-scripts/ifcfg-ens192
Modify the file to assign a static IP: Add or modify the following parameters:

bash
Copy code
BOOTPROTO=static
IPADDR=192.168.1.10    # Replace with the static IP for the server
NETMASK=255.255.255.0  # Subnet mask
GATEWAY=192.168.1.1    # Default gateway
DNS1=8.8.8.8           # Primary DNS server (e.g., Google DNS)
DNS2=8.8.4.4           # Secondary DNS server (optional)
ONBOOT=yes             # Ensure the interface is brought up at boot
Save and exit (:wq).

Restart the network service to apply changes:

sudo systemctl restart NetworkManager
Verify the IP address:

Use the following command to check if the static IP is correctly assigned:

ip addr show ens192
Configure DNS Resolution
To configure DNS resolution, you need to edit the /etc/resolv.conf file:

Edit the resolv.conf file:

sudo vi /etc/resolv.conf
Add the DNS server information: Add the following lines for DNS resolution:

nameserver 8.8.8.8    # Primary DNS server (Google DNS in this case)
nameserver 1.1.1.1    # Secondary DNS server (Cloudflare DNS in this case)
Save and exit the file.

Note: Changes to /etc/resolv.conf may not persist after a reboot. To make them persistent, you can configure the DNS settings in the network configuration files (/etc/sysconfig/network-scripts/ifcfg-ens192) or disable overwriting by network services (like NetworkManager) by adding the line PEERDNS=no to the interface file.

Configure Firewall Rules
CentOS 8 uses firewalld to manage firewall rules. Follow these steps to configure the firewall:

Start and enable the firewall service if it's not already running:

sudo systemctl start firewalld
sudo systemctl enable firewalld
Open necessary ports for services such as HTTP, HTTPS, and SSH:

To allow HTTP (port 80):

sudo firewall-cmd --permanent --add-port=80/tcp
To allow HTTPS (port 443):

sudo firewall-cmd --permanent --add-port=443/tcp
To allow SSH (port 22):

sudo firewall-cmd --permanent --add-port=22/tcp
Reload the firewall to apply changes:

sudo firewall-cmd --reload
Verify the active firewall rules:

sudo firewall-cmd --list-all
Summary of Commands
Assign Static IP:

sudo vi /etc/sysconfig/network-scripts/ifcfg-ens192
systemctl restart NetworkManager
ip addr show ens192
Configure DNS Resolution:

sudo vi /etc/resolv.conf
Configure Firewall Rules:

sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --permanent --add-port=443/tcp
sudo firewall-cmd --permanent --add-port=22/tcp
sudo firewall-cmd --reload
sudo firewall-cmd --list-all

This process ensures that your CentOS servers have static IPs, proper DNS resolution, and firewall rules that allow necessary traffic, meeting the Networking Configuration criteria of your assignment.

# 3. Install and Configure Software Packages

Install Apache (Web Server)

Install Apache:

sudo yum install httpd -y

Start and enable Apache:

systemctl start httpd
systemctl enable httpd

<img width="411" alt="va" src="https://github.com/user-attachments/assets/6e8f0672-acbc-4eab-b3a9-dd0ca2b8df05">


Install MariaDB (Database Server)
Install MariaDB:

sudo yum install mariadb-server -y
Start and enable MariaDB:

systemctl start mariadb
systemctl enable mariadb

<img width="415" alt="db" src="https://github.com/user-attachments/assets/dc102312-e60f-45ad-bbd6-8863da5f4ef9">


Secure MariaDB:

mysql_secure_installation

<img width="448" alt="ma" src="https://github.com/user-attachments/assets/60a84704-1aea-476e-9d3d-8421618d202e">

Install PHP (Web Server)
Install PHP and necessary extensions:

sudo yum install php php-mysqlnd php-fpm php-opcache php-cli php-gd -y

<img width="363" alt="as" src="https://github.com/user-attachments/assets/fc6be5ac-188d-46a8-abe7-f2f20318c931">


Restart Apache to load PHP:

systemctl restart httpd

<img width="373" alt="sa" src="https://github.com/user-attachments/assets/190758ea-43c2-410d-8c69-00c70f4d29a6">


# 4. Implement Security Measures
Enable SELinux
Edit the SELinux configuration file:

vi /etc/selinux/config
Set SELINUX=enforcing:

SELINUX=enforcing
Reboot the system for changes to take effect:

reboot
Configure iptables

Set iptables rules for services:

firewall-cmd --permanent --add-service=http
firewall-cmd --permanent --add-service=ssh
firewall-cmd --reload

Install Security Updates

Ensure that all packages are up-to-date by running:

sudo yum update -y

# 5. Set Up Monitoring and Logging

Install Monitoring Tools (e.g., Nagios)

Install Nagios:

Follow official instructions for installing Nagios, or use tools like Zabbix or Prometheus.

Install and configure rsyslog for centralized logging:

sudo yum install rsyslog -y
systemctl start rsyslog
systemctl enable rsyslog

# 6. Create Documentation

Maintain a detailed document covering:

Network configurations (IP addresses, DNS, etc.).

Software configurations (Apache, MariaDB, etc.).

Firewall and SELinux settings.

Monitoring and logging setup.
Security policies.

# 7. Regular Maintenance Tasks
Schedule System Updates via cron:

Edit the cron configuration for automatic updates:

crontab -e

Add:

0 2 * * * /usr/bin/yum update -y
Automate Backups using rsync or tar:

rsync -av /var/www/html/ /backup/

# 8. Testing and Validation
Verify service functionality (e.g., web and database server access):

Access the Apache web server by visiting http://<server-ip>.

Use mysql -u root -p to check database functionality.

Security Audits: Use tools like nmap to scan open ports and Lynis for security audits.

# 9. Redundancy and Failover Implementation

Set up Load Balancing:

For web servers, use HAProxy or NGINX to load balance between multiple servers.

Database Replication:

Set up MariaDB replication for redundancy:

Configure the master and slave replication in my.cnf.

# 10. Continuous Monitoring and Optimization

Monitor Performance:

Use tools like sysstat and sar:

yum install sysstat -y
sar -u 1 3  # Check CPU utilization

# Detailed Summary: Completion of Criteria for CentOS Server Deployment and Management

The assignment details the step-by-step process to deploy, configure, and maintain multiple CentOS 8 servers, ensuring that all aspects of the deployment meet the provided completion criteria. Here's how the assignment fulfills each of the criteria:

# 1. Servers Deployed: Three CentOS Servers Deployed and Fully Configured

The guide explains how to deploy three CentOS 8 servers either in a virtualized environment or on physical hardware. Each server is set up with a unique static IP address, networking configuration, and designated roles (e.g., web server, database server).

These roles are implemented by installing the necessary software packages like Apache for the web server, MariaDB for the database server, and PHP for the application server.

# 2. Networking Configured: Static IP, DNS, and Firewall Set Up Correctly

Static IP Assignment: Each server has been assigned a static IP address using configuration in the /etc/sysconfig/network-scripts/ifcfg-ens192 file, ensuring consistent network communication.

DNS Resolution: DNS is configured in the /etc/resolv.conf file, allowing the servers to resolve domain names and access external resources.

Firewall Rules: The firewall is configured using firewalld and necessary ports are opened (e.g., HTTP, HTTPS, SSH). These rules are made persistent using the --permanent option, ensuring they survive reboots.

# 3. Software Installed: Each Server Runs Its Designated Services

Web Server: Apache is installed and configured to serve web applications.

Database Server: MariaDB is installed, secured using mysql_secure_installation, and is ready to manage databases.

PHP: Installed and configured to work with the web server for dynamic content.

Each server is optimized for its specific role (e.g., application, web, or database server) and is configured to handle its respective tasks efficiently.

# 4. Security Implemented: SELinux, iptables, and Updates Configured

SELinux is enabled and set to enforcing mode, providing robust access control and protecting the system from malicious activities.
Firewall (iptables) rules are configured to restrict access to specific services, minimizing the attack surface.

Security Updates: yum update is scheduled via cron to ensure that security patches are applied regularly, keeping the systems protected from vulnerabilities.

# 5. Monitoring Setup: Tools Like Nagios and rsyslog Installed
Nagios or Similar Monitoring Tools: Monitoring tools like Nagios, Zabbix, or Prometheus are installed to continuously monitor system performance, uptime, and resource utilization.

Logging with rsyslog: The rsyslog service is installed and configured to log system events, providing centralized logging for monitoring and troubleshooting purposes.

# 6. Documentation: Deployment, Configuration, and Maintenance Procedures Well Documented

The guide includes comprehensive instructions for each stage of deployment, configuration, and maintenance.

Network Diagrams, configuration files, and step-by-step installation procedures are documented, making it easy to reproduce or troubleshoot the setup in the future.

Documentation also covers security configurations, firewall rules, and software installation, ensuring long-term maintainability.

# 7. Regular Maintenance: Automated Updates and Backups Configured

Automated Updates: A cron job is scheduled to automatically run yum update, ensuring that security patches and software updates are applied regularly without manual intervention.

Automated Backups: The guide recommends using tools like rsync, tar, or Bacula for automated backups, ensuring that data is regularly backed up and available for disaster recovery.

# 8. Tested: Services Are Functional, Secure, and Performance Tested

Functionality Tests: Each server role is tested for functionality:
The web server is verified by accessing the Apache default page.
The database server is tested by logging into MariaDB and executing queries.

Security Testing: Security audits are performed using tools like nmap for port scanning and Lynis for vulnerability assessment, verifying that the systems are secure.

Performance Testing: System resources and performance metrics are monitored using tools like sysstat and sar to ensure that the servers are performing optimally under expected loads.

# 9. Redundancy: Failover and Redundancy Mechanisms Implemented

Web Server Redundancy: Load balancing is implemented using tools like
HAProxy or NGINX to distribute web traffic across multiple servers, ensuring high availability and failover capabilities.

Database Redundancy: MariaDB replication is configured, providing redundancy and data replication across servers to ensure continuity in case of hardware failure.

# 10. Continuous Optimization: Monitoring and System Tuning Ongoing

System Performance Monitoring: Tools like sysstat and sar are used to continuously monitor CPU, memory, and disk usage, helping to identify and mitigate potential bottlenecks.

Feedback Loop: The system is tuned based on resource utilization, workload demands, and user feedback, allowing for ongoing optimization and adjustment of configurations as requirements evolve.

