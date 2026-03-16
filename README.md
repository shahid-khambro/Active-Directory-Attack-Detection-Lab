# Active Directory Attack Detection Lab (SOC Analyst Project)

# Project Overview
This project simulates a real-world Active Directory attack scenario and demonstrates how a Security Operations Center (SOC) Analyst can detect malicious activity using Splunk SIEM.
The lab environment was built using multiple virtual machines connected to the same internal network. An attacker machine (Kali Linux) performs attacks against a Windows Active Directory environment, while security monitoring tools such as Sysmon, Splunk Forwarder, and Splunk SIEM collect and analyze logs to detect suspicious activities.

**The goal of this project was to practice:**
- Attack simulation
- Log collection
- Threat detection
- SIEM investigation
- SOC analyst workflow

# Lab Architecture
<img width="700" height="520" alt="active directory project drawio" src="https://github.com/user-attachments/assets/c7ed83b2-2a18-4543-9121-8f2be39c01cf" />

# Components Used

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
- Splunk: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SIEM platform for log monitoring and detection 
- Sysmon: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Advanced Windows event logging 
- Splunk Universal Forwarder: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Sends logs to Splunk server 
- Atomic Red Team:	 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;&nbsp;&nbsp;&nbsp; Attack simulation framework 
- Hydra:	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp; Password brute-force tool
- Kali Linux: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Penetration testing environment 


# Implementation Steps

## Phase 1: Environment Setup

**1 Virtual Network Setup**
All virtual machines were connected to the same internal network (192.168.10.0/24) to simulate a corporate environment.

Virtual Machines created:
- Kali Linux
- Windows Server (Active Directory)
- Windows 10
- Ubuntu Server (Splunk)

**2. Active Directory Setup**\
Windows Server was configured as the Domain Controller.
Steps:
- Install Active Directory Domain Services
- Promote server to Domain Controller
- Create domain mydfir.local
- Create domain users
- Join Windows 10 machine to domain

**3. Splunk SIEM Setup**
Ubuntu server was configured as a Splunk Server.
Steps:
- Install Splunk Enterprise
- Configure web interface
- Create data inputs
- configure receiving port for forwarders

**4. Sysmon Installation**
Sysmon was installed on both Windows machines to generate detailed logs.\
Sysmon64.exe -i sysmonconfig.xml

Sysmon provides logs for:
- Process creation
- Network connections
- File changes
- Registry modifications

**5. Splunk Universal Forwarder Setup**
- Installed on:
- Active Directory Server
- Windows 10 Host

Purpose:
- Forward Windows logs and Sysmon logs to Splunk server.

# Phase 2: Attack Simulation & Detection
**1. Brute Force Attack using Hydra**
The attacker machine (Kali Linux) attempted to brute force login credentials on the Windows host using Hydra.

<img width="908" height="247" alt="attack successful" src="https://github.com/user-attachments/assets/8e074d0f-7c75-4378-9409-829e51ca6876" />


Goal:
- Simulate a password brute force attack
- Generate authentication failure logs

**2. Attack Simulation using Atomic Red Team**
Atomic Red Team was installed on Windows 10 to simulate real-world attack techniques.

**Installation commands:**
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing)


**Atomic Red Team allows simulation of MITRE ATT&CK techniques.**
Example simulated activities:

- PowerShell attacks
- Credential access attempts
- Persistence techniques

<img width="1897" height="705" alt="atomic red team success" src="https://github.com/user-attachments/assets/6d9a2554-fe68-4266-b6a5-37f2378f383f" />


# Phase 3: Security Monitoring & Detection using Splunk

Splunk was used to collect and analyze logs from:

- Active Directory Server
- Windows 10 host
- Log Sources
- Windows Event Logs
- Sysmon logs
- Authentication logs

<img width="1238" height="682" alt="2 server connect with splunk server" src="https://github.com/user-attachments/assets/c52cbb30-038a-4a7e-b9a9-c6535de047d0" />

**Example Splunk Investigations**
**Failed Login Attempts** \
index=wineventlog EventCode=4625\
This query detects failed login attempts indicating possible brute force attacks.

**Successful Login** \
<img width="1317" height="516" alt="success event fount" src="https://github.com/user-attachments/assets/9f62c4bd-dddf-4364-9fce-7b0c6ff4360b" />


This shows successful authentications.

**PowerShell Activity** \
Used to detect suspicious PowerShell executions.

Observed Hosts in Splunk
<img width="1877" height="767" alt="after attacking the powershell attack serach in splunk powershell" src="https://github.com/user-attachments/assets/e783adb1-a7e2-487e-9364-71bdcc061de7" />


**Two active hosts were visible in Splunk:**
- ADDC1 (Active Directory Controller)
- Target-PC (Windows 10)

Screenshots from the Splunk dashboard show these systems generating logs

<img width="1238" height="682" alt="2 server connect with splunk server" src="https://github.com/user-attachments/assets/3149f8f2-5fac-410b-adad-74b012740a9c" />



# Phase 4: Key Findings & Observations
During the attack simulation several important observations were made:

**1. Brute Force Detection** \
Multiple Event ID 4625 entries were generated due to failed login attempts.

**2. Successful Authentication** \
After password compromise, a successful login (Event ID 4624) was recorded.
<img width="1317" height="516" alt="success event fount" src="https://github.com/user-attachments/assets/42e717ef-e15c-4206-8225-80fa7643f6c8" />

**3. PowerShell Attack Detection** \
Atomic Red Team attacks generated Sysmon Event ID 1 logs showing PowerShell execution.
<img width="1897" height="705" alt="atomic red team success" src="https://github.com/user-attachments/assets/1b12579f-9e5f-4c1b-ada0-bfdde1fd93eb" />

**4. Endpoint Visibility**
- Splunk successfully collected logs from:
- Domain Controller
- Windows client host

# Skills Demonstrated
This project demonstrates several important SOC analyst skills.

**SIEM Monitoring** 
Using Splunk to investigate logs and identify suspicious activity.

**Threat Detection** 
Detecting brute force attacks and malicious PowerShell execution.

**Log Analysis** 

Analyzing Windows Event Logs and Sysmon logs.

**Incident Investigation** 

Tracing attack activity from attacker machine to target host.

**MITRE ATT&CK Simulation** 

Using Atomic Red Team to simulate adversary techniques.

**Key Learning Outcomes** 

- From this project I learned:
- How Active Directory environments are attacked
- How attack logs appear in SIEM systems
- How SOC analysts detect suspicious activities
- Importance of endpoint telemetry using Sysmon
- How to correlate logs between different systems


# Conclusion
This project successfully simulated an Active Directory attack environment and demonstrated how a SOC analyst can detect malicious activities using Splunk SIEM.
By combining attack simulation tools (Hydra and Atomic Red Team) with log monitoring tools (Sysmon and Splunk), it was possible to observe real attack patterns and investigate them using SIEM queries.

**This hands-on lab provided practical experience in:**
- SOC monitoring
- Threat detection
- Log analysis
- Incident investigation

Such practical labs are essential for developing the skills required for a Security Operations Center (SOC) Analyst role.


# MITRE ATT&CK Mapping
This project simulates multiple attacker behaviors that align with the MITRE ATT&CK framework, which is widely used by SOC teams for threat detection and threat hunting.

- Attack Activity	MITRE Technique	Technique ID
- Brute Force Password Attack	Brute Force	T1110
- PowerShell Execution	Command and Scripting Interpreter	T1059.001
- Credential Access Attempt	Credential Access	T1003
- Privilege Escalation	Valid Accounts	T1078
- Lateral Movement Simulation	Remote Services	T1021

**Example Mapping**
- Hydra Password Attack
- Technique: Brute Force
- MITRE ID: T1110

**Description:** \
The attacker used Hydra from Kali Linux to attempt multiple login attempts against the Windows system.

**PowerShell Attack using Atomic Red Team** \
Technique: Command and Scripting Interpreter
MITRE ID: T1059.001
Description:
Atomic Red Team executed PowerShell-based attack simulations that generated security logs detected by Sysmon and Splunk.

**Detection Rules (Splunk SPL Queries)** \
Below are the detection queries used in Splunk to identify suspicious activity.

**1 Failed Login Detection (Brute Force)** \
Detect multiple failed login attempts.
index=wineventlog EventCode=4625
| stats count by Account_Name, Source_Network_Address
| sort - count

**Security Event ID:**
Event ID	Description
4625	Failed login attempt

SOC Insight:\
Multiple failed logins may indicate a password brute force attack.

**2 Successful Login Detection** \
Detect successful authentication events.
index=wineventlog EventCode=4624
| stats count by Account_Name, Workstation_Name

Security Event ID:

Event ID	Description
4624	Successful login

SOC Insight:
A successful login after many failed attempts could indicate credential compromise.

**3 PowerShell Execution Detection** \
Detect PowerShell activity logged by Sysmon. \
index=sysmon EventCode=1 powershell
Sysmon Event:\
Event ID	Description \
1	Process Creation 

SOC Insight:
Attackers frequently abuse PowerShell for post-exploitation activities.

**4 Suspicious Network Connections** \
index=sysmon EventCode=3
| stats count by DestinationIp, Image
Sysmon Event:\
Event ID	Description \
3	Network Connection 

SOC Insight:
This helps detect suspicious outbound connections from compromised hosts.
