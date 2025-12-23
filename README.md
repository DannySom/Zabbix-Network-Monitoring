<p align="center">
<img width="600" height="300"  alt="download (1)" src="https://github.com/user-attachments/assets/17a37c1d-3eca-4976-a339-0e2fd80fe10e" />
</p>

<h1>Zabbix Monitoring Deployment</h1>
Zabbix is an open-source enterprise monitoring platform used by organizations to track the health, performance, and availability of their IT infrastructure. It provides real-time visibility across servers, virtual machines, networks, applications, and cloud environments — all from a single centralized dashboard. This demonstration will showcase a small monitoring environment using Zabbix to installing and configuring monitoring agents, setting up triggers, creating dashboards, receiving alerts, and responding to simulated incidents. <br />


<h2>Environments and Technologies Used</h2>

- Digital Ocean (Virtual Machines/Compute)
- Putty

<h2>Operating Systems Used </h2>

- Ubuntu 24.04 (LTS) x64
- Centos 9 Stream x64
- Windows 11 x64


<h2>Installing and Configuring Zabbix Server and Agents Steps</h2>

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/d19afaf5-1694-420e-bf65-1ce78a7c5aaf" />

</p>
<p>
First I deployed a Ubuntu Linux Virtual Machine on DigitalOcean to serve as the Zabbix Server, which I will install and configure to monitor multiple hosts in the lab environment.
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/dee0ff46-9589-4c10-ac61-43894bfcc16e" />

</p>
<p>
Then I installed PuTTY on my local machine and established a secure SSH connection to the Ubuntu VM, enabling remote administration and configuration of the Zabbix Server.
</p>
<br />

<p>
 <img width="600" alt="image" src="https://github.com/user-attachments/assets/ca098547-3c6c-4e0e-b40d-a987497c9c0e" /> <img width="600" alt="image" src="https://github.com/user-attachments/assets/019af30b-534f-4801-9e62-f90d8a216f95" /> <img width="600" alt="image" src="https://github.com/user-attachments/assets/e1beb233-c0bf-4e2a-bd9a-a8f616d9eee5" />

</p>
<p>
Next, we will go into the Zabbix download page and type in the commands(the link is shown below). It's important to note that if you select a different setting, the commands to install Zabbix will be different. </p> https://www.zabbix.com/download?zabbix=7.0&os_distribution=ubuntu&os_version=24.04&components=server_frontend_agent&db=mysql&ws=apache 
</p> After we finish installing Zabbix, we want to make sure Zabbix will still run in case if our computer reboots. To ensure that Zabbix Server, Agent and Apache restart in case of reboot, run this command.</p>

```systemctl enable zabbix-server zabbix-agent apache2```
</p>
<br />

<p>
<img width="400" height="350" alt="Screenshot 2025-12-01 172734" src="https://github.com/user-attachments/assets/6ac4d360-6627-4032-9ad6-b16d21802ce5" /> <img width="400" height="350" alt="Screenshot 2025-12-01 173939" src="https://github.com/user-attachments/assets/8642f14c-159e-4032-a192-bbbf26d5fa74" />

</p>
<p>
To access our new Zabbix server, I entered http://(your-server-ip-address)/zabbix. Now we are able to log in and configure the Zabbix Server Front End and set the user interface. 
</p>
<br />

<p>
<img width="400" height="350" alt="Screenshot 2025-12-01 184609" src="https://github.com/user-attachments/assets/8adbc798-a030-4386-a044-5bf4eae237ea" /> <img width="400" height="350" alt="Screenshot 2025-12-01 184802" src="https://github.com/user-attachments/assets/af8a829f-c1e8-4d91-a59c-305e1e773a06" />

</p>
<p>
Now, I will install Zabbix Agent on a new VM called Host-1 on the same network as my Zabbix Server. In the Zabbix download page, I will keep the current setting as my Zabbix server but I change Zabbix Component from "Server, Frontend, Agent" to "Agent"
</p>
<br />

<p>
<img width="600" alt="Screenshot 2025-12-01 185759" src="https://github.com/user-attachments/assets/f598be1f-51b1-4955-8066-6dd7eaf1d127" />
</p>
<p>
Now we will install Zabbix agent onto Host-1 VM. Also make sure that your agents are using the same version as your Zabbix server. The following command I typed are shown below: </p>


```wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_7.0-1+ubuntu24.04_all.deb``` </p>

```dpkg -i zabbix-release_7.0-1+ubuntu24.04_all.deb``` </p>

```apt update``` </p>
After that, I then run </p>

```apt install zabbix-agent``` </p>

Now to configure the agent, </p>


```nano /etc/zabbix/zabbix_agentd.conf``` </p>
Since the config file is configured to default, it is configured incorrectly. To fix this, edit parameters for Server → Host-1's private IP, ServerActive → Host-1's private IP, Hostname → Host-1 and save. </p>

After any changes to the configuration, you need to restart the agent. </p>


```service zabbix-agent restart```
</p>
<br />

<p>
<img width="600" alt="Screenshot 2025-12-01 192338" src="https://github.com/user-attachments/assets/3d4f120f-f93e-49ab-885e-1e7c2b5fd1ee" />
</p>
<p>
Now to configure a new host using the Zabbix Server user interface. To do this, go to Data Collection → Hosts → Create Host. </p>
Set Host name → Host-1, set template to Linux by Zabbix Agent, Host Groups → Linux Servers, add interface to tell where the Agent is with Host-1's private IP address.
</p>
<br />

<p>
<img width="600" alt="Screenshot 2025-12-01 193106" src="https://github.com/user-attachments/assets/e2e73664-e995-4358-9ff4-69a099be54a0" />

</p>
<p>
Go to Monitoring → Latest Data → Select Host-1 </p>
Here, we can see that the Zabbix server is getting information back from Host A
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/d26a81f9-a78b-4bd9-9832-24fb0d7dc101" />
</p>
<p>
Things we can do to check that we have configured the right information is to use tools like ping or telnet. If we ping Host-1's IP address from Zabbix Server, I'm getting a response. If Ping doesn't work, then it could be that you have a firewall blocking ICMP or that you have the wrong IP address.
</p>
<br />

<p>
<img width="400" alt="Screenshot 2025-12-20 184226" src="https://github.com/user-attachments/assets/fb0fc5f5-2301-4967-a681-2d1a4398c9f5" />
 <img width="400" alt="Screenshot 2025-12-20 184803" src="https://github.com/user-attachments/assets/8abb00ce-1125-484c-bd61-3b61c532e5d8" />

</p>
<p>
Now we will add a Zabbix agent and will configure it to using Active Checks on a Windows host. Instead of installing it on a VM, I will install it on my personal computer.</p>
Active checks means that the agent initiates the connection and pushes data to the server on its own schedule. This approach is especially useful for hosts behind NAT or firewalls, where inbound connections from the server are blocked. By using active checks, the agent can send data outbound without requiring any firewall changes. </p>
In the setup, it has already found my host name of my computer so I just need to type in the Zabbix server's IP address.
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/1f607b12-9b3d-4061-bec2-f664c0b16ca0" />
</p>
<p>
Now if we go to Task Manager, click Zabbix Agent then Open Service, then double click Zabbix Agent inside there, we can see the path to executable and the configuration file. You can also start, stop, pause, and resume the service from there.
</p>
<br />

<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/3a2ad5b5-10dc-4712-81bb-031ad461bd4c" />
</p>
<p>
Next, go to "C:\Program Files\Zabbix Agent 2." From there, you can see the log file. Open the log file then if you go to the bottom there will be an error, "no active checks on server, host not found," but we do have a connection.
</p>
<br />

<p>
<img width="600" alt="Screenshot 2025-12-20 192631" src="https://github.com/user-attachments/assets/e4ca8523-c0f4-4592-98ba-5483c176d5ac" />
</p>
<p>
To solve this, go onto the Zabbix interface and creae a new host. </p>
Monitoring → Host → Create Host </p>
From there, type in the host name then select the template, "Windows by Zabbix agent active." and in the host group type, "Windows Server." then add host. Ensure your hostname in the Zabbix UI matches the Hostname in the agents' configuration file. </p>
Remember that Active checks are pushing data to the server and Passive checks is when zabbix makes a request to the agents. An agent running Active checks is less work for the Zabbix server because it doesn't have to manage a queue of items that its waiting for responses for.
</p>
<br />



<p>
<img width="600" alt="image" src="https://github.com/user-attachments/assets/0bc62333-f192-424b-9e5f-09ac6a14e549" />
</p>
<p>
I have now added my Windows host and all of the hosts I need. Host 1 and 2 are configured with Active Checks because they're in the same network but Windows host is configured with Active Checks because it's behind a NAT.</p>
I also added Host 2 in the background using Centos 9 Stream x64 from Digital Ocean. The commands are typed are shown below: </p>

```sudo yum install -y nano```

and edit the EPEL repo configuration file,

```sudo nano /etc/yum.repos.d/epel.repo```

You will see a blank file. Add the following:

```bash
[epel]
...
excludepkgs=zabbix*
```

```rpm -Uvh https://repo.zabbix.com/zabbix/7.0/centos/9/x86_64/zabbix-release-latest-7.0.el9.noarch.rpm```

Once you install the Zabbix repository package, the system will know where to find Zabbix packages so you can install the agent easily with these commands:

```dnf clean all```

```dnf install zabbix-agent```

Since the config file is configured to default as well, you will need to change the config file, </p>

```nano /etc/zabbix/zabbix_agentd.conf```


edit the following, </p>
Server=<ZABBIX_SERVER_IP> <p>
ServerActive=<ZABBIX_SERVER_IP> <p> Hostname=Host-2 </p>
Then verify the agent status and logs: agent successfully connected to the Zabbix server and began sending metrics.

```service zabbix-agent start```

```service zabbix-agent status```

</p>
<br />

<h2>Troubleshooting Agent Configuration</h2>

<p>
To read the Zabbix Server logs, on the Zabbix Server type, </p>

```tail -f /var/log/zabbix/zabbix_server.log```

To read the Zabbix Agent log files on the host, </p>

```tail -f /var/log/zabbix/zabbix_agentd.log```


</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
