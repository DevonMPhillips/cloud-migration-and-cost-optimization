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

