# Deploying-Configuring-and-Maintaining-Systems

# Tier 3 Task: Deploying, Configuring, and Maintaining Systems

# 1. Deploying Three CentOS Servers
You can set up three CentOS 8 servers using a virtualization platform such as VirtualBox, VMware, or KVM. Once the servers are deployed:

Ensure each VM or physical machine has CentOS 8 installed.
Log in as the root user or use sudo for administrative tasks.

# 2. Networking Configuration
Assigning Static IP Addresses
Edit network interface configuration files (e.g., /etc/sysconfig/network-scripts/ifcfg-ens192):

vi /etc/sysconfig/network-scripts/ifcfg-ens192
Modify the content of the file as follows:

BOOTPROTO=static
IPADDR=192.168.1.10    # Static IP for the first server
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
DNS1=8.8.8.8
ONBOOT=yes

Save and exit the file. Repeat this for the other two servers, with unique IP addresses (e.g., 192.168.1.11 and 192.168.1.12).

Restart networking:

nmcli connection reload
systemctl restart NetworkManager
Configure DNS Resolution
Edit the /etc/resolv.conf file:

vi /etc/resolv.conf
Add your DNS server information:

nameserver 8.8.8.8
nameserver 1.1.1.1
Configure Firewall Rules
Install firewalld if not already installed:

sudo yum install firewalld
systemctl enable firewalld
systemctl start firewalld
Open necessary ports (e.g., HTTP, SSH):

firewall-cmd --permanent --add-port=80/tcp  # HTTP
firewall-cmd --permanent --add-port=443/tcp  # HTTPS
firewall-cmd --permanent --add-port=22/tcp  # SSH
firewall-cmd --reload

# 3. Install and Configure Software Packages
Install Apache (Web Server)
Install Apache:

sudo yum install httpd -y
Start and enable Apache:

systemctl start httpd
systemctl enable httpd
Install MariaDB (Database Server)
Install MariaDB:

sudo yum install mariadb-server -y
Start and enable MariaDB:

systemctl start mariadb
systemctl enable mariadb
Secure MariaDB:

mysql_secure_installation
Install PHP (Web Server)
Install PHP and necessary extensions:

sudo yum install php php-mysqlnd php-fpm php-opcache php-cli php-gd -y
Restart Apache to load PHP:

systemctl restart httpd

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

Optimize Configuration based on system load, using feedback from users and resource utilization data.

Completion Criteria

Servers Deployed: Three CentOS servers deployed and fully configured.
Networking Configured: Static IP, DNS, and firewall set up correctly.
Software Installed: Each server runs its designated services (web, database).

Security Implemented: SELinux, iptables, and updates configured.
Monitoring Setup: Tools like Nagios and rsyslog are installed.
Documentation: Deployment, configuration, and maintenance procedures are well documented.

Regular Maintenance: Automated updates and backups are configured.

Tested: Services are functional, secure, and performance tested.

Redundancy: Failover and redundancy mechanisms are in place.
Continuous Optimization: Monitoring and system tuning is ongoing.
With this guide, you will successfully meet the task's completion criteria for deploying and managing CentOS 8 servers.
