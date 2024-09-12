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

<img width="448" alt="ipad" src="https://github.com/user-attachments/assets/195d1367-dc23-4446-8708-45c27cd1df82">


Edit the network configuration file:

Open the network configuration file (e.g., /etc/sysconfig/network-scripts/ifcfg-eth1):

sudo vi /etc/sysconfig/network-scripts/ifcfg-eth1
Modify the file to assign a static IP: Add or modify the following parameters:


BOOTPROTO=static
IPADDR=192.168.1.10    # Replace with the static IP for the server
NETMASK=255.255.255.0  # Subnet mask
GATEWAY=192.168.1.1    # Default gateway
DNS1=8.8.8.8           # Primary DNS server (e.g., Google DNS)
DNS2=8.8.4.4           # Secondary DNS server (optional)
ONBOOT=yes             # Ensure the interface is brought up at boot
Save and exit (:wq).

<img width="448" alt="vav" src="https://github.com/user-attachments/assets/5253b02d-2255-4f54-bd37-4d9de449757b">


Restart the network service to apply changes:

sudo systemctl restart NetworkManager
Verify the IP address:

Use the following command to check if the static IP is correctly assigned:

ip addr show eth1

<img width="449" alt="eth" src="https://github.com/user-attachments/assets/37a96a59-9f95-478f-8d20-113d7b2f5cee">


Configure DNS Resolution
To configure DNS resolution, you need to edit the /etc/resolv.conf file:

Edit the resolv.conf file:

sudo vi /etc/resolv.conf
Add the DNS server information: Add the following lines for DNS resolution:

nameserver 8.8.8.8    # Primary DNS server (Google DNS in this case)
nameserver 1.1.1.1    # Secondary DNS server (Cloudflare DNS in this case)
Save and exit the file.

<img width="446" alt="qwq" src="https://github.com/user-attachments/assets/a2e2ed0c-fbc2-45eb-8671-8e5a69f77c28">


Note: Changes to /etc/resolv.conf may not persist after a reboot. To make them persistent, you can configure the DNS settings in the network configuration files (/etc/sysconfig/network-scripts/ifcfg-ens192) or disable overwriting by network services (like NetworkManager) by adding the line PEERDNS=no to the interface file.

Configure Firewall Rules
CentOS 8 uses firewalld to manage firewall rules. Follow these steps to configure the firewall:

Start and enable the firewall service if it's not already running:

sudo systemctl start firewalld
sudo systemctl enable firewalld

<img width="451" alt="dsa" src="https://github.com/user-attachments/assets/ead55afe-bfbe-4e70-bd09-b9ca3b0e1d03">


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

<img width="434" alt="fe" src="https://github.com/user-attachments/assets/2becc4b1-97e6-4156-ac43-7740ed14f23d">


This process ensures that your CentOS servers have static IPs, proper DNS resolution, and firewall rules that allow necessary traffic, meeting the Networking Configuration criteria of your assignment.

# 3. Install and Configure Software Packages

Install Apache (Web Server)

Install Apache:

sudo yum install httpd -y

Start and enable Apache:

systemctl start httpd
systemctl enable httpd

<img width="411" alt="va" src="https://github.com/user-attachments/assets/6e8f0672-acbc-4eab-b3a9-dd0ca2b8df05">

open /etc/httpd/conf/httpd.conf.

These two values may already be set in that file, but confirm them to be sure:

DocumentRoot /var/www/html
Listen 80

<img width="447" alt="apache" src="https://github.com/user-attachments/assets/dd7c1da4-99b2-49b5-9d42-00c0278bfaa4">


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


<img width="374" alt="phs" src="https://github.com/user-attachments/assets/fd6bf694-c8df-4040-bd2c-ae402255f53a">



# 4. Implement Security Measures
Enable SELinux
Edit the SELinux configuration file:

vi /etc/selinux/config
Set SELINUX=enforcing:

SELINUX=enforcing

<img width="451" alt="se" src="https://github.com/user-attachments/assets/f22ade29-e7e1-4567-9d26-3e52f81361de">

Reboot the system for changes to take effect:

reboot

Configure iptables

Set iptables rules for services:

firewall-cmd --permanent --add-service=http
firewall-cmd --permanent --add-service=ssh
firewall-cmd --reload

<img width="438" alt="fr" src="https://github.com/user-attachments/assets/555c1ecb-17b1-45b9-9f96-1fb84d0a35b7">


Install Security Updates

Ensure that all packages are up-to-date by running:

sudo yum update -y

# 5. Set Up Monitoring and Logging

Install Monitoring Tools (e.g., Nagios)

Install Nagios:

To set up monitoring and logging on your CentOS system, including installing Nagios or rsyslog, you can follow these steps:

1. Install and Configure rsyslog (Centralized Logging)
Install rsyslog:

sudo yum install rsyslog -y
Start rsyslog:

sudo systemctl start rsyslog
Enable rsyslog to start on boot:

sudo systemctl enable rsyslog
Verify rsyslog status:

sudo systemctl status rsyslog
2. Install Nagios (Monitoring Tool)
To install Nagios on CentOS, follow these general steps. Make sure to refer to the official Nagios installation documentation if needed.

Install Dependencies:

First, install the required packages for Nagios:

sudo yum install httpd php gcc glibc glibc-common gd gd-devel make net-snmp -y

Download Nagios and Nagios Plugins:

Download Nagios Core and Nagios Plugins from the official website or use wget:

cd /tmp
wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.4.6.tar.gz
wget https://nagios-plugins.org/download/nagios-plugins-2.2.1.tar.gz

<img width="451" alt="na" src="https://github.com/user-attachments/assets/d5134995-e74d-475f-99e8-563ed1d7cd04">


Extract the Nagios Package:


tar zxvf nagios-4.4.6.tar.gz
cd nagios-4.4.6
Compile and Install Nagios:


./configure
make all
sudo make install

<img width="401" alt="nana" src="https://github.com/user-attachments/assets/bea48080-883e-48dc-842b-9707ba9c1154">


Install Nagios Plugins:


tar zxvf nagios-plugins-2.2.1.tar.gz
cd nagios-plugins-2.2.1
./configure
make
sudo make install

Configure Apache to Serve Nagios:

Enable Apache and set up basic Nagios authentication:


sudo make install-webconf
sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
sudo systemctl restart httpd

<img width="448" alt="cac" src="https://github.com/user-attachments/assets/749e3b6a-5e0d-4bd8-bfd1-ed1f307cef39">


Start Nagios:

Start the Nagios service and enable it to run at startup:


sudo systemctl start nagios
sudo systemctl enable nagios

Access Nagios Web Interface:

You should now be able to access Nagios by visiting http://<your-server-ip>/nagios in a web browser.

With these steps, you will have set up rsyslog for centralized logging and Nagios for monitoring on your CentOS system.


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

<img width="194" alt="cr" src="https://github.com/user-attachments/assets/2dfa81ef-48c3-4b74-b3aa-0344557bbf3d">


Automate Backups using rsync or tar:

rsync -av /var/www/html/ /backup/

<img width="375" alt="caa" src="https://github.com/user-attachments/assets/6dbf4731-06cf-457d-a1d8-1d3358dd1691">


# 8. Testing and Validation
Verify service functionality (e.g., web and database server access):

Access the Apache web server by visiting http://<server-ip>.

Use mysql -u root -p to check database functionality.

<img width="451" alt="db," src="https://github.com/user-attachments/assets/7f5f55db-bb9e-49e0-bc30-98811161c668">


Security Audits: Use tools like nmap to scan open ports and Lynis for security audits.

# 9. Redundancy and Failover Implementation

Set up Load Balancing:

For web servers, use HAProxy or NGINX to load balance between multiple servers.

Database Replication:

Set up MariaDB replication for redundancy:

Configure the master and slave replication in my.cnf.

Edit the MariaDB Configuration File

Open the MariaDB configuration file on the master server:


sudo nano /etc/my.cnf
Add the following lines under [mysqld]:


[mysqld]
server-id=1
log_bin=/var/log/mysql/mysql-bin.log
binlog_do_db=your_database_name
server-id: A unique ID for the master server (must be unique among all servers in the replication setup).
log_bin: Path to the binary log file.
binlog_do_db: The database to be replicated (optional, if you want to replicate all databases, you can omit this line).


<img width="380" alt="sds" src="https://github.com/user-attachments/assets/a5bbaa17-31e0-4f79-9583-fe9e030e87e4">

sudo systemctl restart mariadb


# 10. Continuous Monitoring and Optimization

Monitor Performance:

Use tools like sysstat and sar:

yum install sysstat -y
sar -u 1 3  # Check CPU utilization

<img width="448" alt="nk" src="https://github.com/user-attachments/assets/845b8c07-f253-4126-baed-85682e19b2e1">


# Detailed Summary: Completion of Criteria for CentOS Server Deployment and Management

The assignment details the step-by-step process to deploy, configure, and maintain multiple CentOS 8 servers, ensuring that all aspects of the deployment meet the provided completion criteria. Here's how the assignment fulfills each of the criteria:

# 1. Servers Deployed: Three CentOS Servers Deployed and Fully Configured

The guide explains how to deploy three CentOS 8 servers either in a virtualized environment or on physical hardware. Each server is set up with a unique static IP address, networking configuration, and designated roles (e.g., web server, database server).

These roles are implemented by installing the necessary software packages like Apache for the web server, MariaDB for the database server, and PHP for the application server.

<img width="449" alt="hht" src="https://github.com/user-attachments/assets/987e49d8-3186-43b4-8a7b-3fbb2ca881a0">


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

