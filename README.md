#Active Directory Attack Detection Lab (SOC Analyst Project)

#Project Overview\
This project simulates a real-world Active Directory attack scenario and demonstrates how a Security Operations Center (SOC) Analyst can detect malicious activity using Splunk SIEM.

The lab environment was built using multiple virtual machines connected to the same internal network. An attacker machine (Kali Linux) performs attacks against a Windows Active Directory environment, while security monitoring tools such as Sysmon, Splunk Forwarder, and Splunk SIEM collect and analyze logs to detect suspicious activities.

**The goal of this project was to practice:**

- Attack simulation
- Log collection
- Threat detection
- SIEM investigation
- SOC analyst workflow

#Lab Architecture
<img width="700" height="520" alt="active directory project drawio" src="https://github.com/user-attachments/assets/c7ed83b2-2a18-4543-9121-8f2be39c01cf" />


The following diagram represents the lab architecture used in this project.\
Network          |                                  Information |\
Component        |                                	IP Address | \
Active Directory Server	 |                         192.168.10.7 | \
Splunk Server        |                      	     192.168.10.10 | \
Attacker (Kali Linux)	  |                     192.168.10.250 | \
Windows Target Host	   |                       DHCP | \
Network Range	        |                       192.168.10.0/24 |



#Components Used

**Windows Server (Active Directory)**

- Domain Controller
- Domain Name: mydfir
- Central authentication server

**Windows 10**
- Domain joined host
- Target machine for attacks
- Installed Sysmon and Splunk Forwarder

**Kali Linux**
- Attacker machine
- Used for brute-force and penetration testing

**Ubuntu Server**
- Used as Splunk Server
- Collects logs from hosts


**Tools Used**

Tool  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |&nbsp;&nbsp;&nbsp; Purpose \
Splunk &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;SIEM platform for log monitoring and detection \
Sysmon &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp; Advanced Windows event logging \
Splunk Universal Forwarder &nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp; Sends logs to Splunk server \
Atomic Red Team	 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  |&nbsp;&nbsp;&nbsp;&nbsp; Attack simulation framework \
Hydra	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| &nbsp;&nbsp;&nbsp;&nbsp; Password brute-force tool\
Kali Linux &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Penetration testing environment 


#Implementation Steps

##Phase 1: Environment Setup
1. Virtual Network Setup
All virtual machines were connected to the same internal network (192.168.10.0/24) to simulate a corporate environment.
Virtual Machines created:
Kali Linux
Windows Server (Active Directory)
Windows 10
Ubuntu Server (Splunk)

2. Active Directory Setup
Windows Server was configured as the Domain Controller.

Steps:
Install Active Directory Domain Services
Promote server to Domain Controller
Create domain mydfir.local
Create domain users
Join Windows 10 machine to domain

3. Splunk SIEM Setup
Ubuntu server was configured as a Splunk Server.

Steps:
Install Splunk Enterprise
Configure web interface
Create data inputs
Configure receiving port for forwarders

4. Sysmon Installation
Sysmon was installed on both Windows machines to generate detailed logs.

Example command:
Sysmon64.exe -i sysmonconfig.xml

Sysmon provides logs for:
Process creation
Network connections
File changes
Registry modifications

5. Splunk Universal Forwarder Setup
Installed on:
Active Directory Server
Windows 10 Host

Purpose:
Forward Windows logs and Sysmon logs to Splunk server.
