# cloud-migration-and-cost-optimization
Securely execute a workload migration using native cloud tooling to estimate, track, and aggressively reduce resource spending

---

Before we begin, there are 3 major phases to this project: 

Phase 1: The "On-Premises" Simulation and Discovery
Phase 2: The Migration (Lift and Shift)
Phase 3: Cost Optimization and Automation

---

**Phase 1**

Before you can migrate to the cloud, I need something to migrate. I don't need a massive corporate data center for this; a Virtualized enviorment works perfectly.

The Enviroment: Spin up a couple of virtual machines locally using VirtualBox. VM 1 will host a simple web Nginx server, and the VM 2 will run a MySQL database.

The Networking: Use Netplan to configure static IP addresses on a Bridged network adapter so the VMs can communicate locally and reach the internet.

<img width="1614" height="764" alt="Local_Enviroment" src="https://github.com/user-attachments/assets/361fa9c9-5020-4884-bd5a-3f8f2bb8a018" />




<img width="937" height="715" alt="image" src="https://github.com/user-attachments/assets/5a2859c7-4186-4887-b999-9e9238447781" />


<img width="941" height="717" alt="image" src="https://github.com/user-attachments/assets/b4bf1495-b4b0-4215-b23f-c5b73c11376e" />


<img width="937" height="720" alt="image" src="https://github.com/user-attachments/assets/b0a0931a-06e6-4192-8cbd-d09076678258" />


<img width="937" height="717" alt="image" src="https://github.com/user-attachments/assets/a9069f42-60ad-4d1d-be60-710a3fbf2c53" />


<img width="942" height="720" alt="image" src="https://github.com/user-attachments/assets/e7975e58-d49e-4c6c-aa64-7ca1e6784d1c" />

Created two Ubuntu Server VMs:
Web Server (Nginx)
Database Server (MYSQL)

<img width="952" height="822" alt="image" src="https://github.com/user-attachments/assets/84b72912-bccd-45d9-ba97-6127cd7f811c" />

Set the network adapter to Bridged Adapter


Configured each VM to use a Bridged Network Adapter in VirtualBox, allowing the virtual machines to communicate with the local network and access the internet.



<img width="531" height="337" alt="image" src="https://github.com/user-attachments/assets/764d7c20-ebc8-4c2a-831c-a0eb27767d26" />

Modified the Netplan configuration (/etc/netplan/*.yaml) to replace DHCP with static IPv4 addressing.
Configured:
Static IP addresses
Default gateway
DNS servers

<img width="551" height="185" alt="image" src="https://github.com/user-attachments/assets/bb9a11c6-1c3d-48b5-8037-d3a49eb3874a" />


Applied and validated the configuration using netplan try and netplan apply.
Verified network connectivity between the VMs, the local network, and the internet using ip addr, ip route, and ping.

**Installing Nginx: **

sudo apt install nginx -y

<img width="1277" height="800" alt="image" src="https://github.com/user-attachments/assets/2490a01e-65a5-4681-bb6f-76e5abbb3254" />

Verify Nginx is running: 
sudo systemctl status nginx

<img width="996" height="302" alt="image" src="https://github.com/user-attachments/assets/48cb5a57-11e6-4ec5-a0bb-9cd0d0ac586f" />

Enable it on boot:
sudo systemctl enable nginx

<img width="957" height="62" alt="image" src="https://github.com/user-attachments/assets/4944cf81-2569-4c74-842e-558c3418ee15" />


Allow HTTP through firewall

sudo ufw allow 'Nginx Full'

<img width="471" height="177" alt="image" src="https://github.com/user-attachments/assets/2d677e8e-c029-4772-8125-c1cc09cd1054" />


**Test Nginx locally**

[curl localhost]

You should see HTML output as shown below:

<img width="635" height="401" alt="image" src="https://github.com/user-attachments/assets/60eee9d6-1d67-4527-8db2-6bf1380416a8" />


Testing from my host machine: 

<img width="952" height="665" alt="image" src="https://github.com/user-attachments/assets/ac205737-dd94-4fc9-952c-faa58608351d" />

Now lets configure Basic Reverse Proxy. A reverse proxy allows Nginx to receive client requests and forward them to another backend server.

<img width="686" height="305" alt="image" src="https://github.com/user-attachments/assets/f71ea71a-e403-4b48-93f3-36e8b56f0308" />

Enable it:
[sudo ln -s /etc/nginx/sites-available/reverse-proxy /etc/nginx/sites-enabled/]

Remove default:
[sudo rm /etc/nginx/sites-enabled/default]
<img width="797" height="106" alt="image" src="https://github.com/user-attachments/assets/3d622326-1cc1-401f-88cd-bd6d6db29871" />

Test configuration:
[sudo nginx -t]


**Installing MariaDB**

[sudo apt install mariadb-server mariadb-client -y]

Starting MariaDB
[sudo systemctl status mariadb]

Enabling startup
[sudo systemctl enable mariadb]

**Securing MariaDB**

[sudo mysql_secure_installation]

**Creating a Database**
Create database:
Create user:
Grant permissions:

**Allow Remote Database Connections**
[sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf]

Change to:

bind-address = 0.0.0.0

<img width="1080" height="785" alt="image" src="https://github.com/user-attachments/assets/045be2b2-7ac2-4a1d-9ebb-d5368b6697b9" />

**Test Web VM → Database VM Connectivity**
From Web Server: Install MariaDB client:

<img width="1127" height="157" alt="image" src="https://github.com/user-attachments/assets/c343b639-0a54-4876-956b-1a24c0c0d389" />

Built the application and database layers of a multi-tier Linux environment using two Ubuntu Server virtual machines hosted in Oracle VirtualBox.

Tasks Completed
Web Server (Nginx)
Updated the Ubuntu system and installed the Nginx web server.
Enabled and verified the Nginx service using systemctl.
Configured the server to start automatically on boot.
Verified web server functionality by accessing the default Nginx landing page from both the local VM and the host machine.
Created a basic reverse proxy configuration by defining a custom Nginx server block and enabling it through the sites-available and sites-enabled directories.
Validated the Nginx configuration using nginx -t and restarted the service to apply changes.
Database Server (MariaDB)
Installed MariaDB Server and Client packages.
Enabled and verified the MariaDB service.
Secured the database server using the mysql_secure_installation utility.
Created a dedicated database for the lab environment.
Created a database user with remote access permissions and assigned the appropriate privileges.
Configured MariaDB to listen for remote connections by updating the server bind address.
Restarted the database service to apply configuration changes.
Validation
Verified that the Nginx web server was operational and accessible over HTTP.
Confirmed that the MariaDB service was running and listening on TCP port 3306.
Tested network connectivity between the Web Server and Database Server using ICMP (ping) and database client connection attempts.
Validated the overall multi-tier architecture, preparing the environment for future web applications that communicate with a dedicated backend database.

---


