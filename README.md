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

**Azure Migrate Deployment**

Our next step is to deploy the Azure Migrate appliance into your local environment. Then Use Azure Migrate to scan our local VMs. Generate an assessment report to estimate the baseline Total Cost of Ownership (TCO) if you were to move these specific workloads into the cloud exactly as they are


Create the Azure Migrate Project

<img width="962" height="851" alt="image" src="https://github.com/user-attachments/assets/fb2e6b66-d7ef-4d83-82e5-5b6fd844e59b" />

Start Discovery

<img width="940" height="855" alt="image" src="https://github.com/user-attachments/assets/a8c5293a-39c8-4f0d-94ff-107b07a61be9" />


<img width="940" height="852" alt="image" src="https://github.com/user-attachments/assets/f121faa5-559f-4d87-a309-c23e4812d3ed" />

The files downloaded (.msi installers and AzureMigrateCollector.ps1) are installers that turn an existing Windows Server or Windows 11/10 machine into an Azure Migrate Collector. Create a third Windows Server VM

<img width="938" height="716" alt="Screenshot 2026-07-15 111340" src="https://github.com/user-attachments/assets/b3fb7813-3084-442b-ad3b-c3050ff24e92" />

Made the necessary network configuartion changes
<img width="1001" height="517" alt="image" src="https://github.com/user-attachments/assets/72f4a61a-5c25-4d31-9001-796c474c5322" />


Now that i have the Azure Migrator Collecter
<img width="822" height="155" alt="image" src="https://github.com/user-attachments/assets/7348de1b-f605-4c1f-b1c3-86a72629d304" />

Allow local PowerShell scripts:
[Set-ExecutionPolicy RemoteSigned -Scope Process]
<img width="452" height="20" alt="image" src="https://github.com/user-attachments/assets/309106e2-63ec-4f6c-858c-32b5e4247dc4" />

Now lets run the installer
[.\AzureMigrateCollector.ps1]
<img width="962" height="480" alt="image" src="https://github.com/user-attachments/assets/ce5162ea-f9bb-47fb-bbc1-a297999d4238" />

Microsoft's installer script will then:

Install IIS
Install all the MSI packages
Configure the Collector services
Create the Appliance Configuration Manager
Launch the Configuration Manager automatically when finished

This took around 18 minutes.


---

So I had to restart everything. Azure doesnt  play well well Oracle box.

Create MariaDB server
<img width="872" height="657" alt="image" src="https://github.com/user-attachments/assets/6bb3e178-8571-4d54-b482-fb3431ec3b1e" />

Create Nginx Server
<img width="872" height="657" alt="image" src="https://github.com/user-attachments/assets/a8815f46-dac7-4c8f-8a74-bd3d05c20eb8" />

Azure Migration Server
<img width="870" height="655" alt="image" src="https://github.com/user-attachments/assets/315f96b6-2321-4547-80e4-8ff9f21e9344" />

Install MariaDB
<img width="767" height="12" alt="image" src="https://github.com/user-attachments/assets/003e4fce-1f32-417a-8042-76265020b687" />

Enable MariaDB
<img width="882" height="50" alt="image" src="https://github.com/user-attachments/assets/117b8fa1-aa30-4346-85ea-33b4d0dd2666" />

MariaDB running 
<img width="1020" height="412" alt="image" src="https://github.com/user-attachments/assets/8bd7fbca-2cb2-48e1-9be2-6db5e50b0810" />

Nginx Firewall setup
<img width="621" height="220" alt="image" src="https://github.com/user-attachments/assets/e91e0364-8ad1-4063-b664-3afa001cc080" />

MariaDB setup

Create Database
<img width="331" height="31" alt="image" src="https://github.com/user-attachments/assets/97ef3b7c-362f-4622-a1b7-b03ccb34db30" />

Create User
<img width="667" height="32" alt="image" src="https://github.com/user-attachments/assets/26299bf7-9804-4af4-95c8-c46adc668b03" />

Grant privileges
<img width="726" height="31" alt="image" src="https://github.com/user-attachments/assets/ed96f863-c380-4b3e-8a85-44f30ed3ef52" />

<img width="612" height="32" alt="image" src="https://github.com/user-attachments/assets/ac7d5886-3e87-423d-936d-6850152363af" />

Apply privilges
<img width="221" height="32" alt="image" src="https://github.com/user-attachments/assets/d689e290-8386-48b7-ba7b-5d99eb582ebe" />

Exit
<img width="341" height="32" alt="image" src="https://github.com/user-attachments/assets/a1c36899-0e19-4b59-b09b-84aa4aae3e16" />


Allow Remote Database Connections. By default MariaDB only listens locally.

Edit the bind-address:

[sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf]


<img width="1020" height="762" alt="image" src="https://github.com/user-attachments/assets/83b8eb0c-5611-4374-8c1e-9451e3f698ba" />

Restart MariaDB:
[sudo systemctl restart mariadb]
<img width="505" height="12" alt="image" src="https://github.com/user-attachments/assets/d560c2ba-7444-41c9-99bf-685953ab82cc" />


Phase 7 — Allow Database Traffic

If UFW is enabled:

[sudo ufw allow from 10.0.0.0/24 to any port 3306]

sudo ufw status

<img width="705" height="222" alt="image" src="https://github.com/user-attachments/assets/89034279-6062-4615-badb-64b9bc9a6336" />


Test Web VM → Database VM Connectivity From Web Server:

Install MariaDB client:

[sudo apt install mariadb-client -y]

Connect:

[mysql -h 10.0.0.17 -u webuser -p]

<img width="1019" height="172" alt="image" src="https://github.com/user-attachments/assets/0a3c9aee-0aac-4f2e-b249-b066ae335bc9" />

---
Add steps: 

Azure Migrate

Copy the files locally

Create a directory:

[mkdir C:\AzureMigrate]

<img width="592" height="212" alt="image" src="https://github.com/user-attachments/assets/32659b8b-2305-45db-870a-0aabdc2c7daa" />

Copy everything:

[xcopy Z:\* C:\AzureMigrate\ /E /I].

<img width="797" height="302" alt="image" src="https://github.com/user-attachments/assets/0404ca69-32a5-4a48-8626-9f342181c9da" />

Verify:

<img width="757" height="256" alt="image" src="https://github.com/user-attachments/assets/24768366-fb43-443c-b100-749dd83e605d" />

---

<img width="413" height="242" alt="image" src="https://github.com/user-attachments/assets/6e32e52a-51ff-49ef-8148-10c5bcbb6c57" />


Azure Discover 
generate project Key

<img width="947" height="847" alt="image" src="https://github.com/user-attachments/assets/e20ae520-d8ed-48b4-acf6-f53db7651f8c" />

<img width="952" height="866" alt="image" src="https://github.com/user-attachments/assets/44749e02-f77c-4eb7-81de-dc1477cd7a88" />

Configure the VM with the specs of the azure appilication .VHD
<img width="872" height="652" alt="image" src="https://github.com/user-attachments/assets/a76ddd06-26bb-4cd9-a319-5acdebd20982" />

<img width="872" height="656" alt="image" src="https://github.com/user-attachments/assets/cf8cde3a-8c62-40e5-b984-6b62d8c9e984" />

verify your Azure Migrate project key
<img width="952" height="887" alt="image" src="https://github.com/user-attachments/assets/28232889-2f74-4c17-8a2e-2aefbe93944f" />

Refresh
<img width="957" height="925" alt="image" src="https://github.com/user-attachments/assets/2bc5468c-9e2d-472a-a19a-955fa21b8ad1" />

<img width="955" height="865" alt="image" src="https://github.com/user-attachments/assets/a52611dc-96d3-4166-93ef-786e8ef68b4f" />
